# `awk`

A utility designed for data extraction

## General

The `awk` version may different depending on the operating systems. For all intents and purposes, this guide is going to focus on `gawk` ([GNU `awk`](https://www.gnu.org/software/gawk/)), while simply referring to it as `awk` throughout.

## Installation

* macOS (via Homebrew): `brew install gawk`

## Examples

### Convert line endings from Unix to Windows

```sh
awk 'sub("$", "\r")' «source file» > «output file»
```

### Split one file into many files with different content

Suppose you have a file with the following contents:

```
one 123
one 124
one 125
two 246
two 257
two 268
six 670
six 681
six 693
```

And you want to end up with three files grouped by the first column with contents from the second column, like so:

| `one.txt` | `two.txt` | `six.txt` |
| :-------- | :-------- | :-------- |
| `123`     | `246`     | `670`     |
| `124`     | `257`     | `681`     |
| `125`     | `268`     | `693`     |

The command is:

```sh
awk '{FS="«delimiter»"; print $2>$1".«extension»"}' «source file»
```
