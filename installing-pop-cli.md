# Install Pop CLI

To use Pop CLI, you first need to install Rust and Cargo, then install Pop CLI, and finally run `pop install` to set up your environment.

## 1. Install Rust and Cargo

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

## 2. Install Pop CLI

```bash
cargo install --force --locked pop-cli
```

## 3. Set up your environment

```bash
pop install
```