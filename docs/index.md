# Tokei ([時計](https://en.wiktionary.org/wiki/%E6%99%82%E8%A8%88))

Tokei is a fast, accurate command-line tool and Rust library for counting lines of code. Given one or more paths it reports — grouped by language — the number of files, total lines, lines of code, comment lines, and blank lines.

## Quick-start example

```console
$ tokei .
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
 Language            Files        Lines         Code     Comments       Blanks
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
 BASH                    4           49           30           10            9
 JSON                    1         1332         1332            0            0
 TOML                    2           77           64            4            9
───────────────────────────────────────────────────────────────────────────────
 Markdown                5         1355            0         1074          281
 |- JSON                 1           41           41            0            0
 |- Rust                 2           53           42            6            5
 (Total)                           1471          101         1080          290
───────────────────────────────────────────────────────────────────────────────
 Rust                   19         3416         2840          116          460
 |- Markdown            12          351            5          295           51
 (Total)                           3767         2845          411          511
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
 Total                  32         6745         4410         1506          829
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

## Key features

- **Very fast** — uses Rayon for parallel file processing and counts millions of lines in seconds.
- **Accurate** — handles multi-line comments, nested comments, and comments inside string literals correctly via a small state machine.
- **150+ languages** — see [Supported Languages](supported-languages.md).
- **Multiple output formats** — JSON, YAML, CBOR; output can be saved and fed back into a later run to accumulate statistics.
- **Cross-platform** — macOS, Linux, and Windows.
- **Usable as a library** — embed tokei in your own Rust project; see [Library Usage](library.md).
- **Configurable** — via `tokei.toml` / `.tokeirc`; see [Configuration](configuration.md).
- **No-colour mode** — set `NO_COLOR=1` to disable ANSI colour output.

## Documentation

| Document | Description |
|----------|-------------|
| [Installation](installation.md) | All package managers and manual build instructions |
| [CLI Usage](usage.md) | Every flag and option with examples |
| [Configuration](configuration.md) | Config file format and lookup order |
| [Output Formats](output-formats.md) | JSON / YAML / CBOR output and input reuse |
| [Library Usage](library.md) | Using tokei as a Rust crate |
| [Contributing](contributing.md) | Adding languages, writing tests, filing bugs |
| [Supported Languages](supported-languages.md) | Full language list with extensions |
| [Architecture](architecture.md) | Source layout and internal data flow |

## Links

- Homepage: <https://tokei.rs>
- Repository: <https://github.com/XAMPPRocky/tokei>
- API docs: <https://docs.rs/tokei>
- Changelog: [CHANGELOG.md](../CHANGELOG.md)
