---
description: Tag every row of a CSV column with one of your labels (hunch_pick) and write the tags back
argument-hint: <file.csv> <column> <label1,label2,...> [question]
---

Parse `$ARGUMENTS` as a CSV file path, a column name (or 1-based index), a comma-separated list of 2 to 255 labels, and an optional question that says what is being decided (for example `"Which team should handle this ticket?"`). A label can carry a short definition after a colon, like `billing: invoices and charges`.

1. Read the CSV file and pull out the named column as an ordered list of texts, keeping blank cells as empty strings so row order is preserved.
2. Call `hunch_balance` and mention the credits available before tagging a large file.
3. Call `hunch_pick` with the texts, the labels as `options`, and the question if one was given. A call takes at most 500 texts, so for a longer column call it once per block of 500, in row order.
4. Match each result back to its row by position (not by re-matching text), and write two new columns next to the original: `<column>_tag` (the chosen label, without its definition) and `<column>_confidence`.
5. Save the updated CSV (in place unless the user asked for a different output path) and report how many rows were tagged, how many were skipped and why, the count per tag, and the credits left.

If the argument string is missing a part, ask for it rather than guessing the column or the labels.
