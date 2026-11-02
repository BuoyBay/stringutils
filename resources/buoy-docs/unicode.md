---
title: "StringUtils Unicode folding and search"
summary: "Pinned Unicode 17 full case folding, original-index caseless search and White_Space trimming."
category: "Reference"
order: 22
keywords: ["casefold", "caseless", "unicode", "whitespace", "search"]
symbols: ["StringUtilsUnicodeVersion", "CaseFold", "CaselessCompare", "CaselessEquals", "CaselessIndexOf", "CaselessContains", "UnicodeTrim", "UnicodeLTrim", "UnicodeRTrim"]
related: ["index.md", "api.md", "typed-api.md"]
---

# Unicode folding, search and whitespace

The existing `Import stringutils` also exposes these explicit Unicode helpers.
Existing functions and builtin String comparisons retain their previous behavior.
Data is pinned to **Unicode 17.0.0** and identical on all desktop targets.

| Function | Contract |
| --- | --- |
| `StringUtilsUnicodeVersion()` | Returns `17.0.0` for folding and whitespace data |
| `CaseFold(text, maxScalars = 1000000)` | Unicode default full case fold (C/F mappings); `Straße` becomes `strasse`, `ς` becomes `σ`, and `ﬃ` becomes `ffi` |
| `CaselessCompare(leftText, rightText, maxScalars = 1000000)` | Compare folded scalar sequences; returns -1, 0 or 1 |
| `CaselessEquals(leftText, rightText, maxScalars = 1000000)` | Folded equality |
| `CaselessIndexOf(text, needle, startIndex = 0, maxScalars = 1000000)` | First match at/after the original zero-based scalar index, or -1 |
| `CaselessContains(text, needle, maxScalars = 1000000)` | Whether a complete source-scalar match exists |
| `UnicodeTrim`, `UnicodeLTrim`, `UnicodeRTrim` `(text, maxScalars = 1000000)` | Trim Unicode `White_Space` at both, left or right edges |

Search returns indexes in the original text, not the expanded folded text:
`CaselessIndexOf("Straße", "SS")` returns 4. Both ends must coincide with
original scalar boundaries: `ss` matches `ß`, while `s` cannot match half of
its expansion. Empty needles match `startIndex`, including the end of text.
A start outside `0..String.Len(text)` throws `InvalidArgumentException`.
Search builds a prefix table and scans with KMP, so repeated prefixes do not
cause quadratic search. Table lookups use binary search.

Case folding is **locale independent** and does not apply Turkic tailoring:
`I` folds to `i`, `İ` to `i` plus combining dot, and `ı` remains distinct.
It does not normalize: `é` and `e` plus combining acute remain distinct. Apply
NormalizationKit explicitly when canonical equivalence is required. These APIs
provide neither grapheme segmentation nor linguistic collation and do not
promise the older Xojo runtime's exact historical case tables.

Whitespace uses all 25 Unicode `White_Space` scalars, including NBSP, NEL,
narrow NBSP and ideographic space. Zero-width space and BOM are not whitespace.
Interior text is unchanged. Custom `Trim(text, charset)` remains available.

`maxScalars` bounds the input and fully folded output **per operand**, including
both operands of search/comparison. Negative budgets and exceeded budgets throw
`InvalidArgumentException`; zero allows empty text. Search validates and folds
the whole source even for an empty needle or a late start. Storage is O(n+m)
in bounded input/folded scalars; these are whole-string operations. Inputs must
be valid, NUL-free UTF-8 text: Buoy's inherited String NUL/invalid-UTF-8 behavior
is outside this package's validation contract.
