---
description: Score one column of a CSV file with hunch_score and write the result back
argument-hint: <file.csv> <column> <question> <level1,level2,...levelN>
---

Parse `$ARGUMENTS` as four parts: a CSV file path, a column name (or 1-based index), a question describing what is being scored, and a comma-separated list of 2 to 10 levels ordered low to high (for example `angry,disappointed,neutral,happy,delighted`).

1. Read the CSV file and pull out the named column as an ordered list of texts, keeping blank cells as empty strings so row order is preserved.
2. Call `hunch_balance` and mention the credits available before scoring a large file.
3. Call `hunch_score` with the texts, the question, and the levels split from the comma-separated argument. A call takes at most 500 texts, so for a longer column call it once per block of 500, in row order.
4. Match each result back to its row by position (not by re-matching text), and write two new columns back into the CSV next to the original: `<column>_label` (the level label) and `<column>_confidence`.
5. Save the updated CSV (in place unless the user asked for a different output path) and report how many rows were scored, how many were skipped and why, and the credits left.

If the argument string is missing a part, ask for it rather than guessing the column or the levels.
