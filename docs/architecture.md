# Architecture

This document describes the internal structure of the tokei codebase, the key
modules, and how data flows from the command line to the final output.

## Repository layout

```
tokei/
├── build.rs                  # Build script: code generation
├── languages.json            # Language definitions (source of truth)
├── tokei.example.toml        # Annotated example config file
├── src/
│   ├── main.rs               # CLI entry point
│   ├── cli.rs                # Argument parsing (clap)
│   ├── cli_utils.rs          # Printer, logging setup, number formatting
│   ├── config.rs             # Config struct + file loading
│   ├── consts.rs             # Column widths and other shared constants
│   ├── input.rs              # --input / --output format handling
│   ├── sort.rs               # Sort enum
│   ├── stats.rs              # CodeStats + Report types
│   ├── lib.rs                # Public library API (re-exports)
│   ├── language/
│   │   ├── mod.rs            # Language struct + AddAssign impl
│   │   ├── language_type.rs  # Placeholder; actual code is generated
│   │   ├── language_type.tera.rs  # Tera template for LanguageType enum
│   │   ├── languages.rs      # Languages newtype + get_statistics
│   │   ├── syntax.rs         # Core line-counting state machine
│   │   └── embedding.rs      # Embedded language detection
│   └── utils/                # Internal helper macros and utilities
├── tests/
│   ├── data/                 # Per-language test files
│   └── embedding/            # Embedded-language test files
└── fuzz/                     # Fuzz targets
```

---

## Key modules

### `build.rs` — build-time code generation

Cargo invokes `build.rs` before compiling the library. It performs two tasks:

1. **`generate_languages`** — reads `languages.json`, processes it with the
   [Tera](https://tera.netlify.app/) template engine using
   `src/language/language_type.tera.rs` as the template, and writes the
   generated `LanguageType` enum (with all ~320 variants, extension mappings,
   comment syntax tables, etc.) to `$OUT_DIR/language_type.rs`.

2. **`generate_tests`** — walks `tests/data/` and `tests/embedding/`,
   generates one `#[test]` function per test file, and writes them to
   `$OUT_DIR/tests.rs`. Each generated test reads the expected counts from the
   first line of the test file and asserts that tokei's output matches.

This design means **`languages.json` is the single source of truth** for all
language data. No Rust code needs to change when adding a new language.

---

### `src/language/language_type.rs` + `language_type.tera.rs`

`language_type.rs` is a thin wrapper that includes the generated code:

```rust
include!(concat!(env!("OUT_DIR"), "/language_type.rs"));
```

`language_type.tera.rs` is the Tera template that generates the `LanguageType`
enum and its associated `impl` blocks from `languages.json`. It produces:

- The `LanguageType` enum (one variant per language).
- `LanguageType::name()` — display name.
- `LanguageType::list()` — returns a `Vec<(LanguageType, Vec<&'static str>)>`
  of all variants with their extensions.
- `LanguageType::from_path()` — detects language from a file path.
- Comment/quote/string syntax tables consumed by the state machine.

---

### `src/language/syntax.rs` — line-counting state machine

This is the heart of tokei's accuracy. Rather than using regular expressions,
tokei uses a small **state machine** to count lines. The state tracks:

- Whether we are currently inside a string literal.
- Whether we are inside a block comment (with nesting depth).
- The type of literal or comment delimiter currently active.

This allows tokei to correctly handle:

- Multi-line strings that contain comment-like syntax.
- Nested block comments (e.g. `/* /* inner */ outer */` in Rust).
- String literals that contain unescaped quote characters (verbatim strings).
- Doc strings that may optionally be counted as comments.

---

### `src/language/languages.rs` — `Languages` and `get_statistics`

`Languages` is a newtype around `BTreeMap<LanguageType, Language>`.

`Languages::get_statistics` is the main entry point for counting:

1. Uses the [`ignore`](https://docs.rs/ignore) crate to walk the file system,
   respecting `.gitignore`, `.ignore`, and `.tokeignore` files.
2. Detects each file's language via `LanguageType::from_path`.
3. Dispatches file counting tasks to a
   [Rayon](https://docs.rs/rayon) parallel iterator.
4. For each file, runs the state machine from `syntax.rs` to produce a
   `CodeStats`.
5. Aggregates `CodeStats` into `Language` structs inside the `Languages` map.

---

### `src/language/mod.rs` — `Language`

`Language` holds the **aggregated** statistics for all files of a single
language type:

- `blanks`, `code`, `comments` — running totals.
- `reports` — per-file `Report` values (populated by `get_statistics`, then
  aggregated into the totals by `Language::total()`).
- `children` — per-language embedded statistics
  (e.g., Rust code blocks found inside Markdown files).

---

### `src/stats.rs` — `CodeStats` and `Report`

`CodeStats` is the primitive statistics type for one blob of text:

```
blanks + code + comments = lines (total)
```

It also contains a `blobs` map for embedded-language statistics found inside
the blob.

`Report` wraps `CodeStats` with the file path (`PathBuf`).

---

### `src/config.rs` — `Config`

`Config` is a plain `#[derive(Deserialize)]` struct. It is populated either
from `Config::default()` (all `None`) or from `Config::from_config_files()`
which merges three TOML config files in priority order (config dir < home dir
< current dir).

CLI flags are merged into `Config` via `Cli::override_config` before
`get_statistics` is called.

---

### `src/input.rs` — serialization formats

The `supported_formats!` macro generates the `Format` enum and its
`parse` / `print` methods for each compiled-in serialization format
(JSON always, CBOR and YAML as optional features).

`add_input` reads a file / stdin / inline string, auto-detects the format, and
merges the deserialized `Languages` map into the existing one.

---

### `src/main.rs` — CLI entry point

The `main` function orchestrates:

1. Parse CLI arguments → `Cli` struct.
2. Load config files → `Config`.
3. Merge CLI flags into `Config` via `override_config`.
4. Optionally load `--input` statistics.
5. Call `languages.get_statistics`.
6. Print results via `Printer` (terminal table) or `Format::print` (serialized).

---

## Data flow

```
     languages.json
           │
           ▼
     build.rs (Tera)
           │
           ▼
     LanguageType (generated enum + syntax tables)
           │
           ▼
  ┌────────────────────────────────────────────┐
  │           languages.get_statistics         │
  │                                            │
  │  file system walk (ignore crate)           │
  │       │                                    │
  │       ▼                                    │
  │  LanguageType::from_path  →  skip unknown  │
  │       │                                    │
  │       ▼                                    │
  │  Rayon parallel iterator                   │
  │       │                                    │
  │       ▼                                    │
  │  syntax state machine  →  CodeStats        │
  │       │                                    │
  │       ▼                                    │
  │  Language (aggregate per LanguageType)     │
  └────────────────────────────────────────────┘
           │
           ▼
  ┌───────────────────────────────┐
  │  Output                       │
  │  - Terminal table (Printer)   │
  │  - JSON / YAML / CBOR         │
  │  - Streaming (for_each_fn)    │
  └───────────────────────────────┘
```

---

## Testing approach

- **Per-language tests** are generated at build time from `tests/data/`.
  Each test file contains the expected counts in its first line and exercises
  all edge cases for that language.
- **Embedding tests** in `tests/embedding/` test embedded-language detection
  (e.g. Rust inside Markdown).
- **Integration tests** in the `tests/` directory test CLI behaviour.
- **Fuzz targets** in `fuzz/` use `cargo-fuzz` to exercise the state machine
  with arbitrary input.
