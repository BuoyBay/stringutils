# StringUtils 0.1.0

Pure Buoy text helpers inspired by Joe Strout's public-domain StringUtils.
The package supplies 20 original helpers and nine Unicode APIs, with no native dependencies. See
[provenance and the complete original API gap map](../PROVENANCE.md).

## Use with bpm

The directory is a compiler package (`manifest.json`, `modules/`) and a bpm
source package (`bpm.json`). A consumer can add a local dependency:

```json
{"schema":1,"dependencies":{"stringutils":{"path":"../libraries/stringutils"}}}
```

Run `bpm install`, then `Import stringutils`. Git publication can use bpm's
`subdir: "libraries/stringutils"` shape after committing and tagging; no package
has been published to a remote registry by this change.

A ready-to-import archive is `packages/stringutils-0.1.0.buipkg`. Rebuild it:

```sh
/usr/bin/buoy pkg seal . -o packages/stringutils-0.1.0.buipkg
```

The absolute compiler path works around this development machine's wrapper;
elsewhere use your compiler's `buoy pkg seal` command normally.

## API

All operations return a new value and leave inputs unchanged.

| Functions | Behavior |
| --- | --- |
| Repeat(text, count) | Concatenate count times; nonpositive counts give empty string |
| Reverse(text) | Reverse Unicode codepoints |
| Remove(text, charset = " ", caseSensitive = True) | Remove characters in charset |
| Squeeze(text, charset = " ") | Collapse adjacent identical characters from charset |
| PadLeft/PadRight/PadBoth(text, width, paddingText = " ") | Pad to character width, repeating the whole pattern and truncating only the pattern; odd center padding puts the extra character on the right |
| Chop(text, count) | Drop characters from the right; nonpositive count leaves input unchanged |
| ReplaceRange(text, start, length, replacement) | Replace a validated 1-based character range; length 0 inserts at boundaries including end + 1 |
| SplitByLength(text, width) | Split into character-width pieces; empty input gives empty array |
| Count(text, needle, caseSensitive = True) | Nonoverlapping occurrences; empty needle matches all character boundaries |
| EditDistance(leftText, rightText, maxCells = 1000000) | Exact Unicode codepoint Levenshtein distance; O(n*m) time, O(m) row storage |
| SplitQuoted(text, delimiter = ",", removeQuotes = True) | Strict quoted-field parser with doubled quote escaping |
| JoinQuoted(fields, delimiter = ",") | Quote fields containing the delimiter’s first character (including overlapping prefixes), quotes, CR or LF; escape embedded quotes |
| CountFieldsQuoted(text, delimiter = ",") | Count parsed fields |
| NthFieldQuoted(text, delimiter, index) | Return parsed field using a 1-based index |
| HexB(source) | Uppercase space-separated hexadecimal UTF-8 bytes |
| Trim/LTrim/RTrim(text, charset) | Trim exact characters from a custom character set |

Characters are Unicode codepoints, so combining marks are separate from their
base characters. Comparisons are exact by default. Buoy's case-insensitive
comparison is ASCII-only. Text has no legacy encoding tag.

Invalid ranges, nonpositive chunk widths, empty padding patterns, invalid field
indices, malformed quoted records, and empty or quote-containing delimiters throw
`InvalidArgumentException`. All padding functions validate the padding pattern before
returning already-wide input unchanged.
EditDistance rejects n*m greater than maxCells before its row allocation; maxCells
must be positive. Input character arrays still use O(n+m) storage. Empty-side
comparisons return the other side's length without quadratic work. Other helpers
have no output-size limit; applications should bound untrusted widths and counts.

## Example and checks

Use the text helpers for Unicode strings, padding, edit distance and quoted-field parsing.

## Unicode folding, search and whitespace

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
