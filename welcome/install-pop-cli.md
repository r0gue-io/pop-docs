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

### 1. Install Pop CLI (macOS and Linux via Homebrew)

#### Install Homebrew (if not installed)

- Run the official installer:

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

More info: https://brew.sh

- Add Homebrew to your PATH (if the installer didn’t do it for you):

macOS (Apple Silicon):
```bash
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.zprofile
eval "$([ -x /opt/homebrew/bin/brew ] && /opt/homebrew/bin/brew shellenv)"
```

Linux:
```bash
echo 'eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"' >> ~/.profile
eval "$([ -x /home/linuxbrew/.linuxbrew/bin/brew ] && /home/linuxbrew/.linuxbrew/bin/brew shellenv)"
```

Verify:
```bash
brew --version
```

Now install Pop CLI with Homebrew:

```bash
brew install r0gue-io/pop-cli/pop
```

> Homebrew is available on both macOS and Linux. If you prefer or are on another OS, or want to build from source, use the steps below.

### 2. Install Rust and Cargo (for building from source)

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

### 3. Install Pop CLI from source

> **Note:** If you want to install Pop CLI for ink! v6 support, see [Getting Started with ink! v6](https://app.gitbook.com/s/CzgRNwlfNkPLJDU555lw/guides/migrating-to-inkv6).

```bash
cargo install --force --locked pop-cli
```

### 4. Set up your environment

```bash
pop install
```
