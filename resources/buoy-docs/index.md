---
title: "StringUtils"
summary: "Transform Unicode text, parse quoted records, and measure bounded edit distance."
category: "Overview"
order: 0
keywords: ["string", "unicode", "quoted", "padding", "levenshtein"]
related: ["getting-started.md", "api.md", "typed-api.md"]
---

# StringUtils

StringUtils supplies 20 original pure Buoy text helpers plus nine pinned Unicode APIs: repeating/reversing text, character-set removal and trimming, pattern padding, range replacement, quoted-field records, UTF-8 byte display, and bounded Levenshtein distance. Every operation returns a new value and leaves inputs unchanged.

## Text semantics

Characters are Unicode code points rather than grapheme clusters. Combining marks remain separate from their base characters, and reversing text can move them independently. Inputs have no legacy encoding tag. Comparisons are exact by default; Buoy's case-insensitive comparison is ASCII-only.

Ranges in `ReplaceRange` use a validated 1-based start and character length. This is independent of array slicing conventions. `SplitQuoted` understands strict quoted records with doubled-quote escaping; it is not a general streaming CSV import system.

## Limits and failures

`EditDistance` caps the product of character lengths before allocating its working row. Its default is one million cells, with O(n*m) time and O(m) row storage plus input-character arrays. The Unicode helpers bound input/folded scalar counts. Other original helpers do not impose output-size quotas, so callers should bound untrusted padding widths and repetition counts.

Invalid ranges, chunk widths, padding patterns, delimiters, field indices, quoted records, and work budgets raise `InvalidArgumentException`. The package has no native dependencies or operating-system integration.

Read [Getting started](getting-started.md) for a complete Unicode/quoted-field example and [API reference](api.md) for the exact operations.


[Unicode folding, search and whitespace](unicode.md) adds nine explicit Unicode 17 APIs, with scalar budgets and original-index search.
