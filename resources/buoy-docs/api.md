---
title: "StringUtils API"
summary: "Immutable text helpers, pinned Unicode case folding and search, code-point ranges and strict quoted-field parsing."
category: "Reference"
order: 20
keywords: ["repeat", "trim", "quoted", "hexb", "count", "distance"]
symbols: ["Repeat", "Reverse", "Remove", "Squeeze", "PadLeft", "PadRight", "PadBoth", "Chop", "ReplaceRange", "SplitByLength", "Count", "EditDistance", "SplitQuoted", "JoinQuoted", "CountFieldsQuoted", "NthFieldQuoted", "HexB", "Trim", "LTrim", "RTrim"]
related: ["index.md", "getting-started.md", "typed-api.md"]
---

# API reference

## Transform and format

| Function | Contract |
| --- | --- |
| `Repeat(text, count)` | Repeat text; nonpositive counts return empty text |
| `Reverse(text)` | Reverse Unicode code points |
| `Remove(text, charset = " ", caseSensitive = True)` | Drop characters belonging to the set |
| `Squeeze(text, charset = " ")` | Collapse adjacent identical characters belonging to the set |
| `PadLeft`, `PadRight`, `PadBoth` | `(text, width, paddingText = " ")`; repeat/truncate padding to character width |
| `Chop(text, count)` | Drop characters from the right; nonpositive count leaves input unchanged |
| `ReplaceRange(text, start, length, replacement)` | Replace a validated 1-based character range; length zero inserts, including at end + 1 |
| `SplitByLength(text, width)` | Character-width chunks; empty input returns an empty array |
| `LTrim`, `RTrim`, `Trim` | `(text, charset)`; trim exact custom-set characters |
| `HexB(source)` | Uppercase, space-separated hexadecimal UTF-8 bytes |

PadBoth puts an odd extra padding character on the right. All padding functions reject an empty padding pattern, including when input already meets the requested width; otherwise, already-wide input is returned unchanged. Invalid replacement ranges or nonpositive split widths throw `InvalidArgumentException`.

## Counting and distance

`Count(text, needle, caseSensitive = True)` counts nonoverlapping occurrences. An empty needle matches every character boundary, giving character length plus one.

`EditDistance(leftText, rightText, maxCells = 1000000)` returns exact Unicode code-point Levenshtein distance. A nonpositive budget rejects. Nonempty inputs whose length product exceeds the budget reject before row allocation. Empty-side comparisons return the other length without quadratic work. Time is O(n*m); row storage is O(m), plus O(n+m) for input character arrays.

## Quoted records

| Function | Contract |
| --- | --- |
| `SplitQuoted(text, delimiter = ",", removeQuotes = True)` | Strict quoted-field parser with doubled quote escaping |
| `JoinQuoted(fields, delimiter = ",")` | Quote fields containing the delimiter’s first character (including overlapping prefixes), quotes, CR or LF; escape embedded quotes |
| `CountFieldsQuoted(text, delimiter = ",")` | Parse and count fields |
| `NthFieldQuoted(text, delimiter, index)` | Return the parsed field at a 1-based index |

Malformed records, invalid field indices, and empty or quote-containing delimiters throw `InvalidArgumentException`. These functions parse one complete record; no streaming parser or encoding conversion is implied. All functions leave their input values unchanged.

Read the [getting-started guide](getting-started.md) or return to [StringUtils](index.md).


[Full typed API: properties, parameters, defaults, and returns](typed-api.md)


[Unicode folding, search and whitespace](unicode.md) adds nine explicit Unicode 17 APIs, with scalar budgets and original-index search.
