# StringUtils Unicode implementation plan

Extend the existing package with an independent `StringUtilsUnicode.bui` module,
without changing existing StringUtils functions or builtin operator semantics.

API: `StringUtilsUnicodeVersion`, `CaseFold`, `CaselessCompare`,
`CaselessEquals`, `CaselessIndexOf`, `CaselessContains`, `UnicodeTrim`,
`UnicodeLTrim`, `UnicodeRTrim`. Defaults bound input and folded output to
1,000,000 scalars per operand; callers can choose a smaller budget.

Use pinned official Unicode 17.0.0 CaseFolding C/F mappings (default full fold,
excluding Turkic T tailoring) and PropList White_Space. Check in raw sources,
SHA-256 manifest, Unicode license and deterministic generator. Binary-search
compact generated records; no OS Unicode dependency or normalization dependency.

Search uses KMP over folded scalar sequences and a boundary-to-original-index
map. Return original zero-based scalar indexes, reject matches splitting a
single source scalar's expansion (`s` does not match half of `ß`, `ss` does).
Nonempty searches are linear in folded input size. Empty needle matches the
specified original boundary. Normalization is explicit caller work and not
performed by these functions. No grapheme/locale collation claims.

Validate all module and test sources with actual compiler JSON on six desktop
targets. Execute fresh sealed Linux consumer against every C/F mapping, all
Unicode scalars (batched identity/folding oracle), every White_Space character,
negative/zero/input/expansion budgets, original-index and expansion-boundary
search cases, repeated-prefix adversarial search, and deterministic randomized
searches independently calculated in Python. Existing StringUtils regression
suite must still pass. Seal refreshed package only after success.

Risks: runtime String handling of NUL/invalid UTF-8 remains inherited; public
contract requires valid NUL-free text. Case folding is not locale-aware or
canonical-equivalence comparison; normalize explicitly when needed. Generated
tables must be reproducible and version-matched; resource limits are scalar
counts rather than constant-memory guarantees. Source should be reviewed for
boundary math, expansion budgets and exact comparisons rather than Buoy's
ASCII-insensitive defaults.
