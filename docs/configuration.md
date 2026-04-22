# Configuration

Tokei can be configured through a configuration file so you don't have to
repeat command-line options on every invocation.

## File names

Tokei looks for either of two file names:

| Name | Notes |
|------|-------|
| `tokei.toml` | Preferred; conventional TOML file name |
| `.tokeirc` | Hidden file on Unix; less idiomatic on Windows |

`tokei.toml` takes precedence over `.tokeirc` when both exist in the same
directory.

## Lookup order

Tokei searches three locations in order from **lowest** to **highest**
priority:

1. **Configuration directory** (`$XDG_CONFIG_HOME` on Linux/macOS,
   `{FOLDERID_RoamingAppData}` on Windows — typically `~/.config` on
   Linux and `~/Library/Application Support` on macOS)
2. **Home directory** (`$HOME`)
3. **Current directory** (where you run `tokei`)

Options in the current directory override those in the home directory, which
override those in the config directory. Command-line flags always take the
highest priority.

| Platform | Config directory |
|----------|-----------------|
| Linux | `$XDG_CONFIG_HOME` or `~/.config` |
| macOS | `$XDG_CONFIG_HOME` or `~/.config` |
| Windows | `C:\Users\<user>\AppData\Roaming` |

## Available options

All options are optional. Unset options use their default values.

### `columns`

**Type:** integer  
**Default:** auto-detected terminal width  
**CLI equivalent:** `--columns` / `-c`

Sets the output column width. Only applies to terminal (human-readable) output.

```toml
columns = 80
```

### `sort`

**Type:** string  
**Default:** alphabetical by language name  
**CLI equivalent:** `--sort` / `-s`

Sort languages by the specified column (descending).

```toml
# Possible values: "files", "lines", "blanks", "code", "comments"
sort = "lines"
```

### `types`

**Type:** array of strings  
**Default:** all languages  
**CLI equivalent:** `--types` / `-t`

Filter output to only the listed language types. Language names must match
tokei's internal enum names exactly (see `tokei --languages`).

```toml
types = ["Rust", "Python", "TypeScript"]
```

### `treat_doc_strings_as_comments`

**Type:** boolean  
**Default:** `false`  

When `true`, doc strings (e.g. Python `"""..."""` triple-quoted strings) are
counted as comment lines rather than code lines.

```toml
treat_doc_strings_as_comments = true
```

### `hidden`

**Type:** boolean  
**Default:** `false`  
**CLI equivalent:** `--hidden`

Count hidden files and directories (names starting with `.`).

```toml
hidden = true
```

### `no_ignore`

**Type:** boolean  
**Default:** `false`  
**CLI equivalent:** `--no-ignore`

Disables **all** ignore files (`.gitignore`, `.ignore`, etc.). Setting this
to `true` implies `no_ignore_parent`, `no_ignore_dot`, and `no_ignore_vcs`.

```toml
no_ignore = false
```

### `no_ignore_parent`

**Type:** boolean  
**Default:** `false`  
**CLI equivalent:** `--no-ignore-parent`

Don't respect ignore files in parent directories.

```toml
no_ignore_parent = false
```

### `no_ignore_dot`

**Type:** boolean  
**Default:** `false`  
**CLI equivalent:** `--no-ignore-dot`

Don't respect `.ignore` and `.tokeignore` files, including those in parent
directories.

```toml
no_ignore_dot = false
```

### `no_ignore_vcs`

**Type:** boolean  
**Default:** `false`  
**CLI equivalent:** `--no-ignore-vcs`

Don't respect VCS ignore files (`.gitignore`, `.hgignore`, etc.) in parent
directories.

```toml
no_ignore_vcs = false
```

## Full example

The following is a complete `tokei.toml` / `.tokeirc` example showing every
available option:

```toml
# The width of the terminal output in columns.
columns = 80

# Sort languages based on the specified column.
sort = "lines"

# If set, tokei will only show the languages in `types`.
types = ["Python"]

# Any doc strings (e.g. `"""hello"""` in Python) will be counted as comments.
treat_doc_strings_as_comments = true
```

> See [tokei.example.toml](../tokei.example.toml) in the repository root for
> the canonical annotated example.
