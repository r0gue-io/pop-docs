# Install Pop CLI

## 1. Install Pop CLI

### 1.1 For macOS and Linux (Homebrew)

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

> Homebrew is available on both macOS and Linux. If you prefer or are on another OS, you can build from source instead.

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
