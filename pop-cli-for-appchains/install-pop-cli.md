# Install Pop CLI

> **Quick install (macOS/Linux with Homebrew)**
>
> ```bash
> brew install r0gue-io/pop-cli/pop
> ```
>
> Need Homebrew? Follow [Install Homebrew](#install-homebrew-if-not-installed).

## Other install options

**Using cargo-binstall** (cross-platform, downloads pre-built binaries when available):
```bash
# Install cargo-binstall if you don't have it
curl -L --proto '=https' --tlsv1.2 -sSf https://raw.githubusercontent.com/cargo-bins/cargo-binstall/main/install-from-binstall-release.sh | bash

# Install pop-cli
cargo binstall pop-cli --locked
```

**Using Ubuntu PPA:**
```bash
sudo add-apt-repository ppa:r0gue-io/pop
sudo apt-get update
sudo apt-get install pop-cli
```
> If `add-apt-repository` is not found, install it with `sudo apt-get install software-properties-common`.

**Debian/Ubuntu** (using `.deb` package from GitHub Releases):
```bash
sudo dpkg -i pop-cli_*.deb
```

**Nix/NixOS:**
```bash
# Run directly without installing
nix run github:r0gue-io/pop-cli

# Or install to your profile
nix profile install github:r0gue-io/pop-cli
```

**Arch Linux** (using `pacman` with a `.pkg.tar.zst` from GitHub Releases):
```bash
sudo pacman -U pop-cli-*.pkg.tar.zst
```

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

#### Install Pop CLI with Homebrew

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

> **Note:**  Pop CLI requires Rust 1.90 or later.

## 2. Set up your environment

> **Recommended next step:** Run `pop install` to set up OS packages, Rust tooling, and optional frontend dependencies.
>
```bash
pop install
```

> **Warning:** `pop install` may download and run external scripts when dependencies are missing, including the official installers for Homebrew, rustup, nvm, and Bun.

### Install command flags

| Flag | Description |
| --- | --- |
| `-y`, `--skip-confirm` | Skip confirmation prompts and install everything non-interactively. |
| `-f`, `--frontend` | Install frontend dependencies (Node.js v20+ and Bun). |

### JSON mode

Use global `--json` for structured automation output:

```bash
pop --json install --skip-confirm
pop --json completion --shell zsh --output ~/.zsh/completions/_pop
```

JSON mode notes:

- `pop --json install` requires `-y/--skip-confirm`.
- `pop --json completion` requires a shell via positional `SHELL` or `--shell`.

### Interactive prompts

By default, `pop install` prompts you before installing OS packages and frontend dependencies. Use `-y` to skip all prompts.

### Rust toolchain setup

`pop install` installs or updates rustup, then:
- Sets the default toolchain to `stable`
- Updates Rust
- Adds the `wasm32-unknown-unknown` target
- Installs components: `cargo`, `clippy`, `rust-analyzer`, `rust-src`, `rust-std`, `rustc`, `rustfmt`

### Installing frontend dependencies

If you plan to use frontend templates, you can install the required frontend dependencies:

```bash
pop install -y --frontend
```

This will install:
- Node.js (v20+) via `nvm` if your current `node --version` check fails or is too old
- Bun (required for certain frontend templates like [inkathon](https://github.com/scio-labs/inkathon))

If Bun installs successfully but is not on your PATH yet, Pop CLI checks the default location at `~/.bun/bin/bun`.

### Set up shell completion

Use `pop completion` to generate shell completion scripts for Bash, Zsh, Fish, PowerShell, or Elvish.

#### Usage

```bash
pop completion [SHELL] [--shell <SHELL>] [--output <PATH>]
```

#### Arguments and flags

| Argument/flag | Description |
| --- | --- |
| `SHELL` | Optional positional shell (`bash`, `zsh`, `fish`, `powershell`, `elvish`). Mutually exclusive with `--shell`. |
| `--shell <SHELL>` | Shell to generate completions for (same values as `SHELL`). |
| `-o`, `--output <PATH>` | Write the completion script to a file instead of stdout. |

#### Behavior and defaults

- If you omit `--output`, Pop writes the completion script to stdout.
- If you pass `--output`, Pop writes the file and prints post-install steps for the selected shell.
- If you pass `--output` without a shell, Pop tries to detect the shell from `$SHELL`. If it detects one, it uses that shell and continues. If it cannot detect one and stdin is a TTY, it falls back to interactive setup. If stdin is not a TTY, it returns an error.
- In interactive setup, Pop asks you to choose a shell, suggests a default output path, and confirms before writing the file.

#### Default output paths (interactive)

| Shell | Default path |
| --- | --- |
| `bash` | `~/.local/share/bash-completion/completions/pop` |
| `zsh` | `~/.zsh/completions/_pop` |
| `fish` | `~/.config/fish/completions/pop.fish` |
| `powershell` | `~/.config/powershell/Completions/pop.ps1` |
| `elvish` | `~/.config/elvish/lib/pop.elv` |

#### Post-install steps by shell

| Shell | Steps |
| --- | --- |
| `bash` | Source the file once, add `source <PATH>` to `~/.bashrc`, then restart your shell. |
| `zsh` | Add the completion directory (the parent of the output file) to `fpath`, run `autoload -Uz compinit && compinit`, then restart your shell. |
| `fish` | Restart your shell; Fish auto-loads completions from the standard completions directory. |
| `powershell` | Ensure your PowerShell profile loads the completion file, then restart your shell. |
| `elvish` | Ensure your Elvish config loads the completion file, then restart your shell. |

#### Examples

```bash
pop completion zsh --output ~/.zsh/completions/_pop
```

```bash
pop completion bash > ~/.local/share/bash-completion/completions/pop
```

```bash
pop completion --shell fish --output ~/.config/fish/completions/pop.fish
```

#### Errors and constraints

- `--output` requires a shell if `$SHELL` is not set and stdin is not a TTY.
- The output path cannot be empty.
- Interactive setup requires a resolved home directory.

## 3. Advanced details

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

**Need help?**

Ask on [Polkadot Stack Exchange](https://polkadot.stackexchange.com/) (tag it [`pop`](https://substrate.stackexchange.com/tags/pop/info)) or drop by [our Telegram](https://t.me/onpopio). We're here to help!
