---
title: "StringUtils getting started"
summary: "Format Unicode strings and round-trip quoted fields without changing inputs."
category: "Guides"
order: 10
keywords: ["quote", "delimiter", "editdistance", "unicode", "padding"]
related: ["index.md", "api.md", "typed-api.md"]
---

# Getting started

Install the `stringutils` source dependency through BPM or copy `stringutils-0.1.0.buipkg` into a consumer's `packages/`. `Import stringutils` exposes its pure Buoy functions.

## Work with text and quoted fields

This complete console example reverses Unicode code points, pads text, removes vowels, calculates edit distance, and parses/joins a quoted record.

```buoy
Import stringutils

Sub Main()
    Print Reverse("Hello, Buoy 😀")
    Print PadBoth("StringUtils", 23, "-")
    Print Remove("a small example", "aeiou")
    Print "kitten → sitting: " + String.From(EditDistance("kitten", "sitting"))
    Dim fields As Array Of String = SplitQuoted("name,\"hello, world\",\"say \"\"yes\"\"\"")
    For Each field As String In fields
        Print field
    Next
    Print JoinQuoted(fields)
End Sub
```

The source uses Buoy backslash escapes for quotes. Doubled quotes inside the record are data consumed by the quoted-field parser, not a second syntax for Buoy string literals.

## Select the matching operation

Use `Remove` to drop every matching character and `Squeeze` to collapse adjacent identical members of a character set. Squeeze does not merge different whitespace characters into one canonical space. Padding repeats a whole pattern and truncates only the added padding; it does not truncate already-wide input.

Quoted records accept multi-character delimiters but reject empty delimiters or delimiters containing a quote. `NthFieldQuoted` indexes fields from one. Explicit empty fields and quoted newlines survive `JoinQuoted`/`SplitQuoted` round-trips.

## Bound caller-controlled input

Keep repetition counts and pad widths reasonable before allocating output. Choose `EditDistance`'s `maxCells` for the workload; a positive budget is required. Compare empty-side strings without quadratic work, but remember that Unicode character arrays still require input-sized storage.

See [API reference](api.md) for defaults and error rules, or return to [StringUtils](index.md).
