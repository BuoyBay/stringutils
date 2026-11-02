# Unicode search and transform

Text helpers inspired by Joe Strout's public-domain StringUtils.
The package supplies 20 original helpers and nine Unicode APIs, with no native dependencies. See
[provenance and the complete original API gap map](PROVENANCE.md).

## Install

From a Buoy consumer project with BPM installed:

```sh
bpm add stringutils --git https://github.com/BuoyBay/stringutils.git --rev refs/heads/main
```

BPM records the resolved commit in its lockfile. Keep that lockfile with the
consumer project for reproducible installs.

## Example

```basic
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

## Build from source

With the native `build-packages` driver from [devtools](https://github.com/BuoyBay/devtools)
on your `PATH`, run from this checkout:

```sh
build-packages --root "$PWD" --compile-consumer stringutils
```

The driver checks source targets and compiles an import consumer. See the
[API guide](docs/API.md) for behavior, limits and any host prerequisites.

## Local Buoy setup

On your own machine, install Buoy and run:

```sh
buoy ai init --both
```

## Contributing

[Contributing](CONTRIBUTING.md) · [License](LICENSE)
