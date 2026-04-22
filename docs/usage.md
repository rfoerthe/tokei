# CLI Usage

## Synopsis

```
tokei [FLAGS] [OPTIONS] [--] [input]...
```

When no `[input]` paths are given tokei defaults to the **current directory** (`.`).

---

## Basic usage

Count code in a single directory and all its sub-directories:

```shell
tokei ./src
```

Count code in multiple directories at once:

```shell
tokei ./src ./tests
# or with commas
tokei ./src, ./tests, ./docs
```

---

## Flags

| Flag | Short | Description |
|------|-------|-------------|
| `--files` | `-f` | Show per-file statistics in addition to per-language totals. |
| `--hidden` | | Count hidden files and directories (those beginning with `.`). |
| `--languages` | `-l` | Print a table of all supported languages and their file extensions, then exit. Cannot be used with `[input]`. |
| `--no-ignore` | | Ignore **all** ignore files (`.gitignore`, `.ignore`, etc.). Implies `--no-ignore-parent`, `--no-ignore-dot`, and `--no-ignore-vcs`. |
| `--no-ignore-dot` | | Don't respect `.ignore` and `.tokeignore` files (including those in parent directories). |
| `--no-ignore-parent` | | Don't respect ignore files in parent directories. |
| `--no-ignore-vcs` | | Don't respect VCS ignore files (`.gitignore`, `.hgignore`, etc.) in parent directories. |
| `--compact` | `-C` | Do not print statistics about embedded languages (e.g. Rust code blocks inside Markdown). |
| `--verbose` / `-v` | `-v` | Increase log output. Repeat up to three times: `-v` shows unknown extensions, `-vvv` enables file-level tracing. |
| `--version` | `-V` | Print version and exit. |
| `--help` | `-h` | Print help and exit. |

---

## Options

### `--columns` / `-c`

Set a fixed output column width (terminal output only; cannot be combined with
`--output`).

```shell
tokei . --columns 120
```

### `--exclude` / `-e`

Exclude files and directories matching a gitignore-style pattern. Can be
repeated.

```shell
tokei . --exclude "*.rs"
tokei . --exclude target --exclude "*.generated.js"
```

Paths can also be listed in a `.tokeignore` file in the project root using the
same [gitignore syntax](https://git-scm.com/docs/gitignore).

### `--input` / `-i`

Merge statistics from a previous tokei run. Accepts a file path, an inline
JSON/YAML/CBOR string, or the special value `stdin`.

```shell
# From file
tokei ./src --input ./previous.json

# From stdin
cat previous.json | tokei ./src --input stdin
```

See [Output Formats](output-formats.md) for the expected format.

### `--output` / `-o`

Print statistics in a machine-readable format instead of the human-readable
table.  Cannot be combined with `--columns`.

```shell
tokei . --output json
tokei . --output yaml
tokei . --output cbor
```

See [Output Formats](output-formats.md) for feature-flag requirements.

### `--sort` / `-s`

Sort languages by a column (descending). Mutually exclusive with `--rsort`.

```
Possible values: files, lines, blanks, code, comments
```

```shell
tokei . --sort code
tokei . --sort lines
```

### `--rsort` / `-r`

Same as `--sort` but sorts in ascending (reverse) order.

```shell
tokei . --rsort code
```

### `--types` / `-t`

Filter output to specific languages (comma-separated or repeated flag).

```shell
tokei . --types Rust,Markdown
tokei . -t Rust -t Python
```

Language names must match tokei's internal names exactly (see
`tokei --languages`).

### `--threads` / `-p`

Restrict the number of threads tokei uses. Defaults to the number of logical
CPU cores reported by the OS.

```shell
tokei . --threads 4
```

### `--streaming`

Output each file record immediately rather than buffering all results. Useful
for piping tokei's output to another tool.

```
Possible values: simple, json
```

`simple` emits space-separated columns preceded by a comment header:

```
#  language                          path             lines         code     comments       blanks
```

`json` emits one JSON object per line:

```json
{"language":"Rust","stats":{"name":"src/main.rs","stats":{"blanks":12,"code":88,"comments":3,"blobs":{}}}}
```

```shell
tokei . --streaming simple
tokei . --streaming json | jq '.stats.stats.code'
```

### `--num-format` / `-n`

Control how numbers are formatted in terminal output (cannot be combined with
`--output`).

| Value | Example |
|-------|---------|
| `dots` (default) | `1.234` |
| `commas` | `1,234` |
| `plain` | `1234` |
| `underscores` | `1_234` |

```shell
tokei . --num-format commas
```

---

## Embedded language statistics

When a language such as Markdown or HTML contains embedded code blocks (e.g.
a ` ```rust ` block in Markdown), tokei counts those lines under both the outer
language **and** the inner language. The indented `|- <Language>` rows in the
output show these embedded counts.

Use `--compact` to suppress the embedded-language breakdown.

---

## Examples

```shell
# Count everything in the current directory
tokei .

# Show per-file breakdown
tokei . --files

# Count only Rust and TOML files
tokei . --types Rust,Toml

# Sort by lines of code, descending
tokei . --sort code

# Output JSON and save to file
tokei . --output json > stats.json

# Merge two saved runs
tokei --input ./run1.json --input ./run2.json

# Count hidden files, ignoring the target directory
tokei . --hidden --exclude target
```
