---
description: >-
  These instructions will guide you on installing the necessary prerequisites
  for Linux to install Pop CLI and use all its features.
---

# Linux

> Help improve this documentation. Check our [contribution guidelines](../how-to-contribute.md).

### For Ubuntu

```shell
sudo apt install build-essential
```

OR

```shell
sudo apt install --assume-yes git clang curl libssl-dev protobuf-compiler
```

### For Arch Linux

```shell
pacman -Syu --needed --noconfirm curl git clang make protobuf
```

### For Fedora

```shell
sudo dnf update
sudo dnf install clang curl git openssl-devel make protobuf-compiler
```

### For OpenSUSE

```shell
sudo zypper install clang curl git openssl-devel llvm-devel libudev-devel make protobuf
```

### Install Rust

```shell
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

Don't forget to update the shell to include the necessary environment variables.

```shell
source $HOME/.cargo/env
```

You should now have Rust installed. Double check.

```shell
rustc --version
```

Now let's configure the Rust toolchain.

```shell
rustup default stable
rustup update
rustup target add wasm32-unknown-unknown
```

And add the nightly release channel.

```shell
rustup update nightly
rustup target add wasm32-unknown-unknown --toolchain nightly
```

You can confirm your Rust toolchain was installed properly with the following commands.

```shell
rustup show
rustup +nightly show
```

Awesome. The environment has now been setup. We are now ready to install and use Pop CLI.

### Install Pop CLI

To install Pop CLI run the following command

```shell
cargo install --locked --git https://github.com/r0gue-io/pop-cli
```

Confirm that Pop CLI is installed by running `pop --help` in your terminal

```shell
pop --help
```
