# Contributing

Thank you for your interest in contributing to Tokei!

- [Adding a language](#adding-a-language)
  - [Language definition fields](#language-definition-fields)
  - [Writing a test file](#writing-a-test-file)
- [Bug reports](#bug-reports)

---

## Adding a language

Languages are defined in [`languages.json`](../languages.json) at the
repository root. Adding a language is a matter of:

1. Adding a JSON entry to `languages.json`.
2. Adding a test file under `tests/data/`.
3. Running `cargo test` to verify the expected counts.

### Language definition fields

Each language is identified by a **key** that follows [Rust's enum naming
convention](https://github.com/rust-lang/rfcs/blob/master/text/0430-finalizing-naming-conventions.md#general-naming-conventions)
(PascalCase). The key is used as the enum variant name in the generated code.

```json
"JavaScript": {
    "line_comment": ["//"],
    "multi_line_comments": [["/*", "*/"]],
    "quotes": [["\\\"", "\\\""], ["'", "'"], ["`", "`"]],
    "extensions": ["js", "mjs"]
}
```

#### `name` _(optional)_

Override the display name shown in tokei's output. Use this when the enum key
does not match the canonical display name (e.g. `"JSON"` vs. `"Json"`).

```json
"Json": {
    "name": "JSON",
    ...
}
```

#### `line_comment` _(optional)_

Array of strings that start a single-line comment. Multiple syntaxes are
supported (e.g. PHP accepts both `"#"` and `"//"``).

```json
"Php": {
    "line_comment": ["#", "//"],
    ...
}
```

#### `multi_line_comments` _(optional)_

Array of `[start, end]` string pairs for block comments. Nested comments are
handled automatically when start and end tokens are the same.

```json
"Rust": {
    "multi_line_comments": [["/*", "*/"]],
    ...
}
```

#### `quotes` _(optional)_

Array of `[open, close]` string pairs for regular string literals. Characters
inside strings are not counted as comments.

```json
"Python": {
    "quotes": [["\\\"", "\\\""], ["'", "'"]],
    ...
}
```

#### `verbatim_quotes` _(optional)_

Like `quotes`, but for verbatim strings that may contain unescaped quote
characters (e.g. C# `@"..."` literals).

```json
"CSharp": {
    "verbatim_quotes": [["@\\\"", "\\\""]],
    ...
}
```

#### `doc_quotes` _(optional)_

Array of `[open, close]` pairs for documentation string literals (e.g. Python
`"""..."""`). These are counted as code by default but as comments when
`treat_doc_strings_as_comments` is enabled.

```json
"Python": {
    "doc_quotes": [["\\\"\\\"\\\"", "\\\"\\\"\\\""], ["'''", "'''"]],
    ...
}
```

#### `extensions` _(optional)_

Array of lowercase file extensions (without the leading `.`) that identify
files of this language.

```json
"Rust": {
    "extensions": ["rs"]
}
```

#### `filenames` _(optional)_

Array of **lowercase** filenames (no extension required) that identify files
of this language. Filename matches take priority over extension matches.

```json
"Makefile": {
    "filenames": ["makefile"],
    "extensions": ["makefile", "mak", "mk"]
}
```

A file named `CMakeLists.txt` is detected as `CMake` (not `Text`) when:

```json
"CMake": {
    "filenames": ["cmakelists.txt"]
}
```

#### `important` _(optional, boolean)_

Mark a language as important. Important languages are listed first in some
output modes.

#### `literate` _(optional, boolean)_

Mark a language as literate (e.g. Haskell Bird-style). Non-blank lines in
literate embedded sections are counted as comments in the parent language's
summary.

---

### Writing a test file

Every language addition **requires** a test file. Place it in `tests/data/`
with a name matching the language (e.g. `rust.rs`, `python.py`).

The test file must:

1. Have a **comment on the very first line** (using the language's own comment
   syntax) containing the manually verified expected counts:

   ```
   NUM lines NUM code NUM comments NUM blanks
   ```

2. Include examples of every comment and quote variant the language supports,
   so that all parsing edge cases are exercised.

**Rust example** (`tests/data/rust.rs`):

```rust
//! 48 lines 36 code 6 comments 6 blanks
//! ```rust
//! fn main () {
//!     // Comment
//!
//!     println!("Hello World!");
//! }
//! ```

/* /**/ */
fn main() {
    let start = r##"/*##\"
\"##;
    // comment
    loop {
        if x.len() >= 2 && x[0] == '*' && x[1] == '/' {
            break;
        }
    }
}
```

Run tests with:

```console
cargo test
```

---

## Bug reports

Please include:

1. The exact error message tokei prints.
2. A **minimal reproducer** — the smallest file or directory structure that
   triggers the bug.

Use the following template when opening an issue:

````
This file crashes / produces wrong results:

<filename>
```
<file contents or directory structure>
```

Expected output:
<what you expected>

Actual output:
<what tokei produced>
````

File issues at: <https://github.com/XAMPPRocky/tokei/issues>
