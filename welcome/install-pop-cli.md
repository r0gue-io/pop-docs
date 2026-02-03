---
layout:
  width: default
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: false
---

# Install Pop CLI

## 1. Install Pop CLI

### 1.1 For macOS and Linux (Homebrew)

#### Install Homebrew (if not installed)

Run the official installer (more info [here](https://brew.sh)):

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

Add Homebrew to your PATH (if the installer didn’t do it for you):
- macOS (Apple Silicon):
```bash
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.zprofile
eval "$([ -x /opt/homebrew/bin/brew ] && /opt/homebrew/bin/brew shellenv)"
```

- Linux:
```bash
echo 'eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"' >> ~/.profile
eval "$([ -x /home/linuxbrew/.linuxbrew/bin/brew ] && /home/linuxbrew/.linuxbrew/bin/brew shellenv)"
```

Verify:
```bash
brew --version
```

#### Install Pop CLI with Homebrew:

```bash
brew install r0gue-io/pop-cli/pop
```

### 1.2 Build from source (any OS)

Firstly, install Rust:
```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

And now build pop-cli from source and install it:
```bash
cargo install --force --locked pop-cli
```

## 2. Set up your environment

```bash
pop install
```

`pop install` (alias `pop i`) installs OS packages, Rust tooling, and optional frontend dependencies.

### Install command flags

| Flag | Description |
| --- | --- |
| `-y`, `--skip-confirm` | Skip confirmation prompts and install everything non-interactively. |
| `-f`, `--frontend` | Install frontend dependencies (Node.js v20+ and Bun). |

### Interactive prompts

By default, `pop install` prompts you before installing OS packages and frontend dependencies. Use `-y` to skip all prompts.

### OS packages and behavior

If your OS/distro is unsupported, `pop install` prints a warning and exits without installing OS packages or tooling.

| OS | Package manager | Packages installed |
| --- | --- | --- |
| macOS | Homebrew | `homebrew`, `protobuf`, `openssl`, `cmake`, `rustup` |
| Arch (or compatible) | `pacman` | `curl`, `git`, `clang`, `make`, `protobuf`, `rustup` |
| Ubuntu (or compatible) | `apt` | `git`, `clang`, `curl`, `libssl-dev`, `protobuf-compiler`, `lsof`, `pkg-config`, `rustup` |
| Debian (or compatible) | `apt` | `git`, `clang`, `curl`, `libssl-dev`, `llvm`, `libudev-dev`, `make`, `protobuf-compiler`, `lsof`, `rustup` |
| Red Hat (or compatible) | `yum` | `gcc`, `gcc-c++`, `make`, `cmake`, `pkgconf`, `pkgconf-pkg-config`, `clang`, `curl`, `git`, `openssl-devel`, `protobuf-compiler`, `lsof`, `rustup` |

When you pass `--frontend`, `pop install` also installs `unzip` on Arch, Ubuntu, and Debian.

### Rust toolchain setup

`pop install` installs or updates rustup, then:
- Sets the default toolchain to `stable`
- Updates Rust
- Adds the `wasm32-unknown-unknown` target
- Installs components: `cargo`, `clippy`, `rust-analyzer`, `rust-src`, `rust-std`, `rustc`, `rustfmt`

> Pop CLI targets ink! v6 by default in the latest releases. If you need ink! v5 support, install version `0.10.0`:
>
> ```bash
> cargo install --locked pop-cli --version 0.10.0
> ```

### Installing frontend dependencies

If you plan to use frontend templates with your chains or contracts, you can install the required frontend dependencies:

```bash
pop install -y --frontend
```

This will install:
- Node.js (v20+) via `nvm` if your current `node --version` check fails or is too old
- Bun (required for certain frontend templates like [inkathon](https://github.com/scio-labs/inkathon))

If Bun installs successfully but is not on your PATH yet, Pop CLI checks the default location at `~/.bun/bin/bun`.

These dependencies are automatically checked when you use the `--with-frontend` flag with `pop new chain` or `pop new contract`.

### External scripts

`pop install` may download and run external scripts when dependencies are missing, including the official installers for Homebrew, rustup, nvm, and Bun.
**Need help?**

Ask on [Polkadot Stack Exchange](https://polkadot.stackexchange.com/) (tag it [`pop`](https://substrate.stackexchange.com/tags/pop/info)) or drop by [our Telegram](https://t.me/onpopio). We're here to help!
