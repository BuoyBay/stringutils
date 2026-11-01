# StringUtils provenance and parity

Source page: https://strout.net/info/coding/rb/intro.html

Downloaded archive: https://www.strout.net/files/rb/stringutils.zip

Retrieved 2026-10-31. Archive SHA-256: `53ff48a12b111d9d978b5626bc3c7b8a61e67dbbb3b3bee64061c37004159a8a`.
The archive contains `stringutils/StringUtils.xml` (RBProject 2011r4.3) and
`StringUtilsHarness.rbp`. The XML's License note explicitly puts the module in
the public domain, for any purpose, without a warranty. The implementations
here are independently written Buoy adaptations informed by the source APIs;
the original archive is not redistributed.

## Scope

This is a focused first package, not a complete source-compatible port. Functions
use imported module calls, not REALbasic extension methods. Character operations
use Unicode codepoints, not grapheme clusters. Defaults are exact case-sensitive
matching, unlike several original REALbasic text helpers. Set `caseSensitive` to
False on Count/Remove for Buoy's ASCII case folding.

| Original feature | Buoy package treatment |
| --- | --- |
| Repeat, Reverse | Implemented; Reverse handles UTF-8 codepoints |
| EditDistance | Implemented, exact comparison, O(m) integer row and maxCells work bound |
| PadLeft/Right/Both | Implemented with repeated multi-character patterns; never truncates input |
| Chop (count), ReplaceRange | Implemented; 1-based validated range, zero-length insertion allowed |
| Squeeze, Remove | Implemented with exact charset comparison; Squeeze retains source case |
| Count | Implemented; nonoverlapping matches, empty needle gives character count + 1 |
| SplitByLength, custom Trim/LTrim/RTrim | Implemented on codepoints; width must be positive |
| HexB | Implemented using UTF-8 bytes, uppercase space-separated pairs |
| SplitQuoted, JoinQuoted, CountFieldsQuoted, NthFieldQuoted | Implemented with strict CSV-style whole-field quotes, doubled quote escaping and exact multi-character delimiters |
| Contains, StartsWith, EndsWith, IsEmpty, reverse search | Built-in String namespace already covers these; no redundant wrappers |
| CountB, byte slicing/chop/replacement/split, ReverseB, SqueezeB, byte swaps | Not ported; raw binary should use Bytes rather than manufacture invalid UTF-8 strings |
| Soundex, Double Metaphone, EncodeCase/DecodeCase, MatchCase, WrapLines | Not yet ported; good follow-up package work |
| Hash | Not ported: original weak hash should not become a security-sensitive default |
| SplitToInt/Val/CDbl | Not ported; use String.Split plus typed Integer.Parse/Double.Parse results |
| CountRegEx, SplitByRegEx | Not ported; regex iteration/splitting needs its own explicit empty-match contract |
| SQLify | Not ported; parameter binding is preferable to SQL literal escaping |
| Sprintf | Built-in String.Format covers formatting; no Variant/printf compatibility layer |
| ThousandsSeparator, DecimalSeparator | Built-in Locale supplies locale data; no process-global cached separators |
| ControlCharacters, LineEnding, Random, InStrQuoted, GuessEncoding, Chop suffix overload | Not yet ported; Bytes.DetectEncoding already supplies explicit encoding detection |

Quoted parsing deliberately rejects unmatched quotes, quotes in an unquoted
field, and trailing text after a closing quote. The source SplitQuoted accepted
unterminated quotes and JoinQuoted did not escape embedded quotes. This package
fixes that roundtrip gap and is not a drop-in parser for those permissive inputs.
Empty input means one empty field. A final delimiter preserves an empty last
field. Newlines can occur inside fields; this API parses one supplied record and
does not stream records from a file.

## Original API inventory

The following declarations were read directly from the downloaded XML. Repeated
names represent overloads; private Metaphone helpers are included for traceability.

```text
Function Repeat(s as String, repeatCount as Integer) As String
Function EditDistance(s1 As String, s2 As String) As Integer
Function Soundex(s As String, stripPrefix As Boolean = true) As String
Function Hash(s As String) As Integer
Function Count(source As String, substr As String) As Integer
Function CountB(source As String, substr As String) As Integer
Function PadBoth(s as String, width as Integer, padding as String = " ") As String
Function PadLeft(s as String, width as Integer, padding as String = " ") As String
Function PadRight(s as String, width as Integer, padding as String = " ") As String
Function CountFieldsQuoted(src as string, sep as string) As integer
Function NthFieldQuoted(src as string, sep as string, index as integer) As string
Function Contains(extends s As String, what As String) As Boolean
Function CountRegEx(s As String, pattern As String) As Integer
Function StartsWith(extends s As String, withWhat As String) As Boolean
Function ContainsB(extends s As String, what As String) As Boolean
Function StartsWithB(extends s As String, withWhat As String) As Boolean
Function IsEmpty(extends s As String) As Boolean
Function InStrReverse(startPos As Integer=-1, source As String, substr As String) As Integer
Function InStrReverseB(startPosB As Integer=-1, source As String, substr As String) As Integer
Function SplitToInt(source As String, delimiter As String=" ") As Integer()
Function SplitToVal(source As String, delimiter As String=" ") As Double()
Function SplitToCDbl(source As String, delimiter As String=" ") As Double()
Function SplitByRegEx(source As String, delimPattern As String) As String()
Function SQLify(s As String) As String
Function Chop(s As String, charsToCut As Integer) As String
Function ChopB(s As String, bytesToCut As Integer) As String
Function Squeeze(s As String, charSet As String=" ") As String
Function Remove(s As String, charSet As String=" ") As String
Function ReplaceRange(s As String, start As Integer, length As Integer, newText As String) As String
Function ReplaceRangeB(s As String, startB As Integer, lengthB As Integer, newText As String) As String
Sub Metaphone(source As String, ByRef outPrimary As String, ByRef outAlternate As String)
Function MIsVowel(source As String, atPos As Integer) As Boolean
Function MStringAt(source As String, start As Integer, length As Integer,  paramArray args As String) As Boolean
Function ReverseB(s As String) As String
Function EndsWith(extends s As String, withWhat As String) As Boolean
Function EndsWithB(extends s As String, withWhat As String) As Boolean
Function Sprintf(src as string, ParamArray data as Variant) As string
Function ThousandsSeparator() As String
Function DecimalSeparator() As String
Function SplitByLength(s As String, fieldWidth As Integer) As String()
Function Trim(source As String, charsToTrim As String) As String
Function SplitByLengthB(s As String, fieldWidth As Integer) As String()
Function ControlCharacters() As String
Function LTrim(source As String, charsToTrim As String) As String
Function RTrim(source As String, charsToTrim As String) As String
Function HexB(s As String) As String
Function MatchCase(textToChange As String, sampleText As String) As String
Function Random(length As Integer = 8, charset As String = "0123456789ABCDEFGHJKLMNPQRSTUVWXYZ") As String
Function SplitQuoted(source As String, delimiter As String, removeQuotes as Boolean) As String()
Function JoinQuoted(fields() as String, delimiter as String) As String
Function EncodeCase(s As String) As String
Function DecodeCase(s As String) As String
Function Reverse(s As String) As String
Function LineEnding(s As String) As String
Function InStrQuoted(start As Integer = 1, source As String, find As String) As Integer
Function Chop(s As String, stringToCut As String) As String
Function ChopB(s As String, stringToCut As String) As String
Function SqueezeB(s As String, charSet As String=" ") As String
Sub WrapLines(lines() As String, charsPerLine As Integer = 72, paragraphFill As Boolean = true)
Function GuessEncoding(s As String) As TextEncoding
Function GuessEncoding(s As String, ByRef outWrongOrder As Boolean) As TextEncoding
Function SwapBytePairs(s As String) As String
Function SwapByteQuads(s As String) As String
```

## Unicode extension

The Unicode helpers are independently implemented additions, not adaptations of
Joe Strout's API. Data comes directly from Unicode 17.0.0 `CaseFolding.txt`
(default full C/F mappings, excluding simple-only S and Turkic T) and
`PropList.txt` (`White_Space` property only). Original files and source URL/SHA-256
records are retained in `data/unicode-17.0.0/`. The Unicode data license is
`resources/unicode-LICENSE.txt`, copied from the workspace's pinned Unicode 17
normalization data license. `tools/generate_stringutils_unicode.py` verifies
source hashes before regenerating the embedded tables. This package does not
reuse ICU/OS tables or Python's version-dependent Unicode tables at runtime.
