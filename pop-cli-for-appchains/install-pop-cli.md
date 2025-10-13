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

**Need help?**

Ask on [Polkadot Stack Exchange](https://polkadot.stackexchange.com/) (tag it [`pop`](https://substrate.stackexchange.com/tags/pop/info)) or drop by [our Telegram](https://t.me/onpopio). We're here to help!
