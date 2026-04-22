# Output Formats

Tokei can serialize its results to machine-readable formats and deserialize
previously saved results to accumulate statistics across multiple runs.

## Enabling serialization support

JSON support is compiled in by default. CBOR and YAML require optional
feature flags:

```console
# All formats
cargo install tokei --features all

# CBOR only
cargo install tokei --features cbor

# YAML only
cargo install tokei --features yaml
```

If you try to use a format that was not compiled in, tokei will print an
informative error message and exit.

## Outputting statistics

Use `--output` / `-o` followed by the format name:

```shell
tokei . --output json
tokei . --output yaml
tokei . --output cbor
```

The output is written to **stdout**, so redirect it to a file as needed:

```shell
tokei . --output json > stats.json
```

### JSON

```json
{
  "Rust": {
    "blanks": 460,
    "code": 2840,
    "comments": 116,
    "reports": [...],
    "children": {},
    "inaccurate": false
  },
  "Total": {
    "blanks": 829,
    "code": 4410,
    "comments": 1506,
    "reports": [],
    "children": {},
    "inaccurate": false
  }
}
```

### YAML

```yaml
Rust:
  blanks: 460
  code: 2840
  comments: 116
  reports: [...]
  children: {}
  inaccurate: false
Total:
  blanks: 829
  code: 4410
  comments: 1506
  reports: []
  children: {}
  inaccurate: false
```

### CBOR

CBOR is output as a hex-encoded string. It is the most compact binary format
but is not human-readable.

## Reading previously saved statistics

Use `--input` / `-i` to merge a saved result into the current run. Tokei
accepts:

- **A file path** — reads and parses the file.
- **The string `stdin`** — reads from standard input.
- **An inline string** — the raw serialized content passed directly as the
  option value.

```shell
# Merge a previous JSON file with the current directory
tokei ./src --input ./previous.json

# Read from stdin
cat previous.json | tokei ./src --input stdin
```

Tokei auto-detects the format (JSON, YAML, or CBOR) when parsing input, so
you do not need to specify the format explicitly.

### Use case: accumulating statistics

```shell
# Day 1
tokei ./src --output json > run1.json

# Day 2 — merge previous stats with new counts
tokei ./src --input run1.json --output json > run2.json
```

## Streaming output

For real-time processing (e.g., piping into another tool without waiting for
tokei to finish), use `--streaming`:

```shell
# One space-separated record per file
tokei . --streaming simple

# One JSON object per file, written to stdout immediately
tokei . --streaming json
```

See [CLI Usage — `--streaming`](usage.md#--streaming) for details.
