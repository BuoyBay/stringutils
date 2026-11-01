---
title: "stringutils typed API"
summary: "Source-derived public signatures, properties, parameters, defaults, and return values."
category: "Reference"
order: 21
types: []
dependencies: []
symbols: ["CaseFold", "CaselessCompare", "CaselessContains", "CaselessEquals", "CaselessIndexOf", "Chop", "Count", "CountFieldsQuoted", "EditDistance", "HexB", "JoinQuoted", "LTrim", "NthFieldQuoted", "PadBoth", "PadLeft", "PadRight", "RTrim", "Remove", "Repeat", "ReplaceRange", "Reverse", "SplitByLength", "SplitQuoted", "Squeeze", "StringUtilsUnicodeVersion", "Trim", "UnicodeLTrim", "UnicodeRTrim", "UnicodeTrim"]
related: ["api.md", "getting-started.md", "index.md"]
---

# stringutils typed API

Generated from the package source declarations. Platform guards are shown where present; inherited members are documented on the base type. See the [API contracts](api.md) for behavior and limitations.

## StringUtils functions

### Repeat

`Public Function Repeat(text As String, count As Integer) As String`

Concatenate text count times. Nonpositive counts produce empty text.

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `text` | `String` | `ByVal` | Required |
| `count` | `Integer` | `ByVal` | Required |

#### Returns

| Type |
| --- |
| `String` |

### Reverse

`Public Function Reverse(text As String) As String`

Reverse Unicode codepoints, not grapheme clusters.

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `text` | `String` | `ByVal` | Required |

#### Returns

| Type |
| --- |
| `String` |

### Remove

`Public Function Remove(text As String, charset As String = " ", caseSensitive As Boolean = True) As String`

Remove characters in charset. Matching is exact by default.

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `text` | `String` | `ByVal` | Required |
| `charset` | `String` | `ByVal` | `" "` |
| `caseSensitive` | `Boolean` | `ByVal` | `True` |

#### Returns

| Type |
| --- |
| `String` |

### Squeeze

`Public Function Squeeze(text As String, charset As String = " ") As String`

Collapse repeated identical characters belonging to charset.

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `text` | `String` | `ByVal` | Required |
| `charset` | `String` | `ByVal` | `" "` |

#### Returns

| Type |
| --- |
| `String` |

### PadLeft

`Public Function PadLeft(text As String, width As Integer, paddingText As String = " ") As String`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `text` | `String` | `ByVal` | Required |
| `width` | `Integer` | `ByVal` | Required |
| `paddingText` | `String` | `ByVal` | `" "` |

#### Returns

| Type |
| --- |
| `String` |

### PadRight

`Public Function PadRight(text As String, width As Integer, paddingText As String = " ") As String`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `text` | `String` | `ByVal` | Required |
| `width` | `Integer` | `ByVal` | Required |
| `paddingText` | `String` | `ByVal` | `" "` |

#### Returns

| Type |
| --- |
| `String` |

### PadBoth

`Public Function PadBoth(text As String, width As Integer, paddingText As String = " ") As String`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `text` | `String` | `ByVal` | Required |
| `width` | `Integer` | `ByVal` | Required |
| `paddingText` | `String` | `ByVal` | `" "` |

#### Returns

| Type |
| --- |
| `String` |

### Chop

`Public Function Chop(text As String, count As Integer) As String`

Drop characters from the right; nonpositive counts leave text unchanged.

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `text` | `String` | `ByVal` | Required |
| `count` | `Integer` | `ByVal` | Required |

#### Returns

| Type |
| --- |
| `String` |

### ReplaceRange

`Public Function ReplaceRange(text As String, start As Integer, length As Integer, replacement As String) As String`

Replace a validated 1-based range; zero length inserts at a boundary.

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `text` | `String` | `ByVal` | Required |
| `start` | `Integer` | `ByVal` | Required |
| `length` | `Integer` | `ByVal` | Required |
| `replacement` | `String` | `ByVal` | Required |

#### Returns

| Type |
| --- |
| `String` |

### SplitByLength

`Public Function SplitByLength(text As String, width As Integer) As Array Of String`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `text` | `String` | `ByVal` | Required |
| `width` | `Integer` | `ByVal` | Required |

#### Returns

| Type |
| --- |
| `Array Of String` |

### Count

`Public Function Count(text As String, needle As String, caseSensitive As Boolean = True) As Integer`

Count nonoverlapping occurrences; empty needle matches every boundary.

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `text` | `String` | `ByVal` | Required |
| `needle` | `String` | `ByVal` | Required |
| `caseSensitive` | `Boolean` | `ByVal` | `True` |

#### Returns

| Type |
| --- |
| `Integer` |

### EditDistance

`Public Function EditDistance(leftText As String, rightText As String, maxCells As Integer = 1000000) As Integer`

Exact codepoint Levenshtein distance, with an explicit CPU work bound.

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `leftText` | `String` | `ByVal` | Required |
| `rightText` | `String` | `ByVal` | Required |
| `maxCells` | `Integer` | `ByVal` | `1000000` |

#### Returns

| Type |
| --- |
| `Integer` |

### SplitQuoted

`Public Function SplitQuoted(text As String, delimiter As String = ",", removeQuotes As Boolean = True) As Array Of String`

Parse one record, respecting quoted fields and doubled embedded quotes.

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `text` | `String` | `ByVal` | Required |
| `delimiter` | `String` | `ByVal` | `","` |
| `removeQuotes` | `Boolean` | `ByVal` | `True` |

#### Returns

| Type |
| --- |
| `Array Of String` |

### JoinQuoted

`Public Function JoinQuoted(fields As Array Of String, delimiter As String = ",") As String`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `fields` | `Array Of String` | `ByVal` | Required |
| `delimiter` | `String` | `ByVal` | `","` |

#### Returns

| Type |
| --- |
| `String` |

### CountFieldsQuoted

`Public Function CountFieldsQuoted(text As String, delimiter As String = ",") As Integer`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `text` | `String` | `ByVal` | Required |
| `delimiter` | `String` | `ByVal` | `","` |

#### Returns

| Type |
| --- |
| `Integer` |

### NthFieldQuoted

`Public Function NthFieldQuoted(text As String, delimiter As String, index As Integer) As String`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `text` | `String` | `ByVal` | Required |
| `delimiter` | `String` | `ByVal` | Required |
| `index` | `Integer` | `ByVal` | Required |

#### Returns

| Type |
| --- |
| `String` |

### HexB

`Public Function HexB(source As String) As String`

Uppercase space-separated hexadecimal of the UTF-8 bytes.

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `source` | `String` | `ByVal` | Required |

#### Returns

| Type |
| --- |
| `String` |

### LTrim

`Public Function LTrim(text As String, charset As String) As String`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `text` | `String` | `ByVal` | Required |
| `charset` | `String` | `ByVal` | Required |

#### Returns

| Type |
| --- |
| `String` |

### RTrim

`Public Function RTrim(text As String, charset As String) As String`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `text` | `String` | `ByVal` | Required |
| `charset` | `String` | `ByVal` | Required |

#### Returns

| Type |
| --- |
| `String` |

### Trim

`Public Function Trim(text As String, charset As String) As String`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `text` | `String` | `ByVal` | Required |
| `charset` | `String` | `ByVal` | Required |

#### Returns

| Type |
| --- |
| `String` |

## StringUtilsUnicode functions

### StringUtilsUnicodeVersion

`Public Function StringUtilsUnicodeVersion() As String`

Unicode version for StringUtils folding and White_Space classification.

No parameters.

#### Returns

| Type |
| --- |
| `String` |

### CaseFold

`Public Function CaseFold(text As String, maxScalars As Integer = 1000000) As String`

Unicode default full case fold, without normalization or locale-specific tailoring.

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `text` | `String` | `ByVal` | Required |
| `maxScalars` | `Integer` | `ByVal` | `1000000` |

#### Returns

| Type |
| --- |
| `String` |

### CaselessCompare

`Public Function CaselessCompare(leftText As String, rightText As String, maxScalars As Integer = 1000000) As Integer`

Compare full case-folded scalar sequences; returns -1, 0 or 1.

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `leftText` | `String` | `ByVal` | Required |
| `rightText` | `String` | `ByVal` | Required |
| `maxScalars` | `Integer` | `ByVal` | `1000000` |

#### Returns

| Type |
| --- |
| `Integer` |

### CaselessEquals

`Public Function CaselessEquals(leftText As String, rightText As String, maxScalars As Integer = 1000000) As Boolean`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `leftText` | `String` | `ByVal` | Required |
| `rightText` | `String` | `ByVal` | Required |
| `maxScalars` | `Integer` | `ByVal` | `1000000` |

#### Returns

| Type |
| --- |
| `Boolean` |

### CaselessIndexOf

`Public Function CaselessIndexOf(text As String, needle As String, startIndex As Integer = 0, maxScalars As Integer = 1000000) As Integer`

First caseless match at complete source-scalar boundaries; returns original zero-based index or -1.

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `text` | `String` | `ByVal` | Required |
| `needle` | `String` | `ByVal` | Required |
| `startIndex` | `Integer` | `ByVal` | `0` |
| `maxScalars` | `Integer` | `ByVal` | `1000000` |

#### Returns

| Type |
| --- |
| `Integer` |

### CaselessContains

`Public Function CaselessContains(text As String, needle As String, maxScalars As Integer = 1000000) As Boolean`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `text` | `String` | `ByVal` | Required |
| `needle` | `String` | `ByVal` | Required |
| `maxScalars` | `Integer` | `ByVal` | `1000000` |

#### Returns

| Type |
| --- |
| `Boolean` |

### UnicodeTrim

`Public Function UnicodeTrim(text As String, maxScalars As Integer = 1000000) As String`

Trim exactly Unicode White_Space, including NBSP, without changing interior text.

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `text` | `String` | `ByVal` | Required |
| `maxScalars` | `Integer` | `ByVal` | `1000000` |

#### Returns

| Type |
| --- |
| `String` |

### UnicodeLTrim

`Public Function UnicodeLTrim(text As String, maxScalars As Integer = 1000000) As String`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `text` | `String` | `ByVal` | Required |
| `maxScalars` | `Integer` | `ByVal` | `1000000` |

#### Returns

| Type |
| --- |
| `String` |

### UnicodeRTrim

`Public Function UnicodeRTrim(text As String, maxScalars As Integer = 1000000) As String`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `text` | `String` | `ByVal` | Required |
| `maxScalars` | `Integer` | `ByVal` | `1000000` |

#### Returns

| Type |
| --- |
| `String` |
