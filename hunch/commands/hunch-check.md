---
description: Ask one yes/no question about every row of a CSV column (hunch_ask) and write the probabilities back
argument-hint: <file.csv> <column> <yes/no question>
---

Parse `$ARGUMENTS` as a CSV file path, a column name (or 1-based index), and a yes/no question, for example `"Is this email urgent, meaning the sender needs a reply today?"`. The model sees only the text and the question, so the question has to say exactly what counts as yes.

1. Read the CSV file and pull out the named column as an ordered list of texts, keeping blank cells as empty strings so row order is preserved.
2. Call `hunch_balance` and mention the credits available before checking a large file.
3. Call `hunch_ask` with the texts and the question. A call takes at most 500 texts, so for a longer column call it once per block of 500, in row order.
4. Match each result back to its row by position (not by re-matching text), and write one new column next to the original: `<column>_yes`, the probability of yes from 0 to 1, rounded to two decimals.
5. Save the updated CSV (in place unless the user asked for a different output path) and report how many rows came out above 0.5, the five rows with the highest probability, how many were skipped and why, and the credits left.

If the argument string is missing a part, ask for it rather than guessing the column or the question.
