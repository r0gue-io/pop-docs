# Install Pop CLI

## 1. Install Pop CLI

### 1.1 For MacOS

```bash
brew install r0gue-io/pop-cli/pop
```

### 1.2 For any other OS

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
