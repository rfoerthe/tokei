# Library Usage

Tokei can be used as a Rust library so you can embed code counting directly
in your own tools or analysis pipelines.

Full auto-generated API documentation is available on
[docs.rs/tokei](https://docs.rs/tokei).

## Adding tokei as a dependency

```toml
[dependencies]
tokei = "13"
```

For serialization support add the relevant features:

```toml
[dependencies]
tokei = { version = "13", features = ["all"] }
# or individually: features = ["cbor", "yaml"]
```

---

## Core types

### `Config`

`Config` controls how `Languages::get_statistics` searches and counts files.
All fields are `Option<T>`, so you only need to set the fields you care about;
everything else takes a sensible default.

```rust
use tokei::Config;

let config = Config {
    treat_doc_strings_as_comments: Some(true),
    hidden: Some(true),
    ..Config::default()
};
```

| Field | Type | Default | Description |
|-------|------|---------|-------------|
| `columns` | `Option<usize>` | Terminal width | Column width for terminal output (ignored by the library). |
| `hidden` | `Option<bool>` | `false` | Count hidden files and directories. |
| `no_ignore` | `Option<bool>` | `false` | Ignore all ignore files. |
| `no_ignore_parent` | `Option<bool>` | `false` | Ignore parent-directory ignore files. |
| `no_ignore_dot` | `Option<bool>` | `false` | Ignore `.ignore` / `.tokeignore` files. |
| `no_ignore_vcs` | `Option<bool>` | `false` | Ignore VCS ignore files. |
| `treat_doc_strings_as_comments` | `Option<bool>` | `false` | Count doc strings as comments. |
| `sort` | `Option<Sort>` | `None` | Sort order for languages. |
| `types` | `Option<Vec<LanguageType>>` | `None` | Restrict counting to these language types. |
| `for_each_fn` | `Option<fn(LanguageType, Report)>` | `None` | Callback invoked for every file as it is counted (streaming). |

`Config` can also be loaded from config files on disk:

```rust
let config = tokei::Config::from_config_files();
```

---

### `Languages`

`Languages` is a newtype wrapping a `BTreeMap<LanguageType, Language>`. Call
`get_statistics` to populate it.

```rust
use tokei::{Config, Languages};

let mut languages = Languages::new();
languages.get_statistics(&["src", "tests"], &["target"], &Config::default());
```

**Parameters of `get_statistics`:**

| Parameter | Type | Description |
|-----------|------|-------------|
| `paths` | `&[impl AsRef<Path>]` | Paths to search (relative, absolute, or glob). |
| `ignored` | `&[impl AsRef<str>]` | Gitignore-style patterns for paths to skip. |
| `config` | `&Config` | Configuration to apply. |

After calling `get_statistics`, iterate over the results:

```rust
for (language_type, language) in &languages {
    println!("{}: {} lines of code", language_type, language.code);
}
```

Compute the grand total:

```rust
let total = languages.total();
println!("Total: {} lines", total.lines());
```

Merge another `Languages` map (e.g., from a previous serialized run):

```rust
languages += previous_languages;
```

---

### `LanguageType`

`LanguageType` is an enum with one variant per supported language. Common
operations:

```rust
use tokei::LanguageType;

// Access by variant
let rust = LanguageType::Rust;

// Display name
println!("{}", rust.name()); // "Rust"

// Parse from string
let py: LanguageType = "Python".parse().unwrap();

// List all variants with their extensions
for (lang, exts) in LanguageType::list() {
    println!("{}: {:?}", lang.name(), exts);
}
```

---

### `Language`

`Language` aggregates statistics for all files of a single language.

| Field | Type | Description |
|-------|------|-------------|
| `blanks` | `usize` | Total blank lines across all files. |
| `code` | `usize` | Total code lines. |
| `comments` | `usize` | Total comment lines. |
| `reports` | `Vec<Report>` | Per-file statistics. |
| `children` | `BTreeMap<LanguageType, Vec<Report>>` | Embedded language statistics. |
| `inaccurate` | `bool` | `true` if file parsing had issues. |

```rust
use tokei::{Config, Languages, LanguageType};

let mut langs = Languages::new();
langs.get_statistics(&["."], &["target"], &Config::default());

if let Some(rust) = langs.get(&LanguageType::Rust) {
    println!("Rust — code: {}, comments: {}, blanks: {}",
             rust.code, rust.comments, rust.blanks);
    println!("Total lines: {}", rust.lines());
}
```

---

### `CodeStats`

`CodeStats` holds the statistics for a single blob of source text (one file or
one embedded section).

| Field | Type | Description |
|-------|------|-------------|
| `blanks` | `usize` | Blank lines. |
| `code` | `usize` | Code lines. |
| `comments` | `usize` | Comment lines. |
| `blobs` | `BTreeMap<LanguageType, CodeStats>` | Embedded language blobs. |

```rust
use tokei::CodeStats;

let stats = CodeStats::new();
println!("lines: {}", stats.lines()); // blanks + code + comments
```

---

### `Report`

`Report` wraps `CodeStats` with the file path.

| Field | Type | Description |
|-------|------|-------------|
| `name` | `PathBuf` | Path to the file. |
| `stats` | `CodeStats` | Statistics for this file. |

---

## Complete example

```rust
use std::collections::BTreeMap;
use tokei::{Config, Languages, LanguageType};

fn main() {
    let config = Config::default();

    let mut languages = Languages::new();
    languages.get_statistics(&["src", "tests"], &["target"], &config);

    // Print per-language totals
    for (lang_type, lang) in &languages {
        println!(
            "{:<20} code: {:>8}  comments: {:>8}  blanks: {:>8}",
            lang_type.name(),
            lang.code,
            lang.comments,
            lang.blanks,
        );
    }

    // Grand total
    let total = languages.total();
    println!("\nTotal lines: {}", total.lines());
}
```

---

## Streaming / callback usage

Set `Config::for_each_fn` to process file results as they are produced rather
than collecting them all first:

```rust
use tokei::{Config, Languages, LanguageType, Report};

let config = Config {
    for_each_fn: Some(|lang: LanguageType, report: Report| {
        println!("{}: {} ({})", lang.name(), report.name.display(), report.stats.code);
    }),
    ..Config::default()
};

let mut languages = Languages::new();
languages.get_statistics(&["."], &["target"], &config);
// When for_each_fn is set, Languages is not populated and the process exits
// after the callback has been called for every file.
```
