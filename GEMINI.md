# Hunch

This context is loaded whenever the Hunch extension is active. Hunch is a remote MCP server that turns a batch of short texts into a calibrated number instead of generated prose: a probability, a pick from your own options, a position on your own scale, or several yes/no answers at once. Reach for it whenever the job is to judge, tag, rank, or classify many similar texts the same way, not to write or summarize.

Hunch does not read spreadsheets or files itself. Read the texts with whatever tool already has them (a file read, a CSV parser, a Google Sheets MCP server), pass them to a Hunch tool, then write the results back with that same tool.

## The five tools

| Tool | Use it for | Cost |
| --- | --- | --- |
| `hunch_ask` | One yes/no question per text: "is this urgent," "is this a decision maker" | 1 credit per answered text |
| `hunch_pick` | Sorting texts into your own categories | 1 credit per answered text |
| `hunch_score` | Placing texts on your own ordered scale (sentiment, fit, severity) | 1 credit per answered text |
| `hunch_multi` | Several yes/no facts about the same text in one call (up to 10 questions) | 1 credit per question per answered text |
| `hunch_balance` | Checking credits left before a big batch | Free |

Blank texts and texts repeated elsewhere in the same call are free. The same text asked again in a later call is charged again, so keep the results instead of asking twice.

## Picking a tool

- A single yes/no question, one answer per text: `hunch_ask`.
- Sorting into named buckets (billing, refund, bug, other): `hunch_pick`. Give 2 to 255 options.
- A low-to-high position (angry to delighted, no-fit to perfect): `hunch_score`. Give 2 to 10 levels, ordered low to high.
- Several yes/no facts read off the same text at once (can they buy, are they angry, is it urgent): `hunch_multi`, one call instead of several separate `hunch_ask` calls. Up to 10 questions.
- Before any batch worth worrying about the bill, or after a tool reports texts were skipped: `hunch_balance`.

## Writing questions and options that work

The model reads the text and nothing else about it: no surrounding context, no column headers, no company knowledge. Put the whole definition of what counts as "yes," or what separates one option or level from the next, inside the question or option text itself.

- Weak: `"Is this urgent?"`
- Better: `"Is this support ticket urgent: does it mention an outage, data loss, or a blocked payment?"`

- Weak option: `"billing"`
- Better option: `"billing: invoices, charges, or refund requests"` (the `"label: description"` form disambiguates a short label without changing what gets written back, which is always the label).

Other limits: the model judges text only, no math, counting, or dates. English works best; other languages work with lower accuracy. Up to 500 texts per tool call (Hunch chunks internally); `hunch_multi` takes 1 to 10 questions; `hunch_pick` takes 2 to 255 options; `hunch_score` takes 2 to 10 levels.

## Cost awareness

- Call `hunch_balance` first for any batch in the hundreds of rows, and stop to say if credits look short rather than burning through them.
- `hunch_multi` is cheaper than separate calls whenever more than one yes/no fact is needed from the same text: one call, one credit per question per text, instead of one full pass per question.
- Deduplicate before you're surprised: identical texts in the same call are already free, but texts that differ in casing or inner whitespace are not deduplicated by Hunch (it only trims leading and trailing spaces), so normalize them yourself if the source data is messy.
- A tool result reports how many texts were actually answered versus skipped, and why (usually out of credits). Read that before assuming every row got a value.

## Reading a CSV or spreadsheet export and writing results back

1. Read the source (the CSV file, or the relevant column from a spreadsheet MCP server's read tool) into an ordered list of texts, keeping blanks as empty strings rather than dropping rows. Row order is how results get matched back up.
2. Call the right Hunch tool with the whole list, up to 500 texts per call, not once per row. Duplicates inside one call are free, and it is far fewer round trips.
3. Match each result back to its row **by position**, not by re-matching the text, since duplicate texts in the input collapse to one answer internally but the tool still returns one result per input row in order.
4. Write the new column (or columns, for `hunch_multi`) back with the same tool that read the source, then report what was skipped and why, if anything was.

Example: "Read column A of Sheet1, use hunch_pick with options billing, refund, bug, other to sort each row, then write the picks into column B": the spreadsheet MCP server handles steps 1 and 4, Hunch handles step 2.
