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

> Pop CLI targets ink! v6 by default in the latest releases. If you need ink! v5 support, install version `0.10.0`:
>
> ```bash
> cargo install --locked pop-cli --version 0.10.0
> ```

**Need help?**

Ask on [Polkadot Stack Exchange](https://polkadot.stackexchange.com/) (tag it [`pop`](https://substrate.stackexchange.com/tags/pop/info)) or drop by [our Telegram](https://t.me/onpopio). We're here to help!
