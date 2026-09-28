# Hunch

Hunch gives Claude calibrated judgments about text. For each text you give it, Hunch returns the probability of a yes/no answer, one option from your own list, a position on your own scale, or several yes/no answers at once. This plugin connects Claude to the Hunch MCP server, adds a skill that tells Claude when and how to use it, and adds three commands that judge one column of a CSV file.

## What you need

A Hunch key, which starts with `hunch_`. New keys include 100 free rows and are created at https://hunchsheet.app. The plugin stores no key and no file in it holds one. The first time you use Hunch, Claude opens a Hunch sign-in page in your browser. Paste the key there and click Connect. Claude receives a token and never sees the key.

In Claude Code, run `/mcp`, choose `hunch` and select Authenticate. In claude.ai and Cowork, connect Hunch from the plugin's Connectors tab.

## Commands

- `/hunch:hunch-check <file.csv> <column> <yes/no question>` adds a `<column>_yes` column with the probability of yes, from 0 to 1.
- `/hunch:hunch-tag <file.csv> <column> <label1,label2,...> [question]` adds `<column>_tag` and `<column>_confidence`.
- `/hunch:hunch-score <file.csv> <column> <question> <level1,...,levelN>` adds `<column>_label` and `<column>_confidence`.

Each command checks your credit balance first, sends the column in blocks of up to 500 rows, adds the new columns, and reports the credits left. By default it saves over the file you named, so give a different output path if you want to keep the original. The skill also helps when you ask Claude in plain words to tag, rate or rank many short texts and write the results back to a CSV or a spreadsheet.

## What this plugin runs, sends and fetches

- It runs no scripts, hooks or local servers, and it fetches nothing itself. Claude uses its own file tools to read and write your CSV.
- The texts in the column you name, plus your question, labels or levels, are sent to https://hunchsheet.app/mcp. Hunch passes them to TypeSafe's Jev model to compute the answers. Hunch does not store the texts, questions or answers.
- Hunch keeps your key, its credit balance, usage counts and the time it was last used. The privacy policy is at https://hunchsheet.app/privacy.
- The plugin sends nothing to any other address.

## Cost and limits

One credit per answered text, and one per question per text when you ask several questions at once. Blank cells and repeated texts inside one call are free. Asking about the same text again in a later call is charged again. Hunch reads the text only. It does no arithmetic, counting or date reasoning, and English works best. Please do not send payment card numbers, health records, government ID numbers, passwords or API keys.

## Support

Contact: https://hunchsheet.app/contact. Terms: https://hunchsheet.app/terms. Licensed under MIT.
