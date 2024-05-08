---
description: >-
  These instructions will guide you on installing the necessary environment to
  install Pop CLI and use all its features.
---

# Linux

### For Ubuntu

```
sudo apt install build-essential
```

OR

```
sudo apt install --assume-yes git clang curl libssl-dev protobuf-compiler
```



### For Arch Linux

```
pacman -Syu --needed --noconfirm curl git clang make protobuf
```

### For Fedora

```
sudo dnf update
sudo dnf install clang curl git openssl-devel make protobuf-compiler
```

### For Opensuse

```
sudo zypper install clang curl git openssl-devel llvm-devel libudev-devel make protobuf
```

### Install Rust

```
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

```
source $HOME/.cargo/env
```

```
rustc --version
```

```
rustup default stable
rustup update
```

```
rustup update nightly
rustup target add wasm32-unknown-unknown --toolchain nightly
```

Make sure the Rust toolchain was installled properly

```
rustup show
rustup +nightly show
```
