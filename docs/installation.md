# Installation

## Package managers

### Unix / Linux

```console
# Alpine Linux (≥ 3.13)
apk add tokei

# Arch Linux
pacman -S tokei

# Cargo (any platform with Rust toolchain)
cargo install tokei

# Conda
conda install -c conda-forge tokei

# Fedora
sudo dnf install tokei

# FreeBSD
pkg install tokei

# NetBSD
pkgin install tokei

# Nix / NixOS
nix-env -i tokei

# OpenSUSE
sudo zypper install tokei

# Void Linux
sudo xbps-install tokei
```

### macOS

```console
# Homebrew
brew install tokei

# MacPorts
sudo port selfupdate
sudo port install tokei
```

### Windows

```console
# Winget
winget install XAMPPRocky.tokei

# Scoop
scoop install tokei

# Chocolatey
choco install tokei
```

## Pre-built binaries

Pre-built binaries for all major platforms are attached to every release on
the [GitHub Releases page](https://github.com/XAMPPRocky/tokei/releases).
Download the archive for your platform, extract it, and place the `tokei`
(or `tokei.exe`) binary somewhere on your `PATH`.

## Building from source

Tokei requires the **latest stable** [Rust](https://www.rust-lang.org) toolchain
(minimum version specified in `Cargo.toml` under `rust-version`).

### Install via Cargo directly from Git

```console
cargo install --git https://github.com/XAMPPRocky/tokei.git tokei
```

### Clone and build locally

```console
git clone https://github.com/XAMPPRocky/tokei.git
cd tokei
cargo build --release
# Binary is at: target/release/tokei
```

### Optional serialization features

By default, only JSON output is compiled in. To enable additional output
formats pass feature flags at install/build time:

```console
# All serialization formats (JSON + CBOR + YAML)
cargo install tokei --features all

# CBOR only
cargo install tokei --features cbor

# YAML only
cargo install tokei --features yaml
```

See [Output Formats](output-formats.md) for more details.

## Docker

A minimal Alpine-based Docker image can be built with
[Earthly](https://github.com/earthly/earthly):

```bash
earthly +docker
```

Run the image against an arbitrary directory:

```bash
docker run --rm -v /path/to/analyze:/src tokei .
```

Analyse the current directory (Linux / macOS):

```bash
docker run --rm -v "$(pwd)":/src tokei .
```
