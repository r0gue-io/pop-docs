---
description: >-
  These instructions will guide you on installing the necessary prerequisites
  for macOS to install Pop CLI and use all its features.
---

# macOS

> Help improve this documentation. Check our [contribution guidelines](../how-to-contribute.md).

### Install

First we will need [Homebrew](https://brew.sh/) - the macOS package manager. Run the following in your Terminal.

```shell
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
```

Make sure `brew` installed properly.

```shell
brew --version
```

And let's make sure `brew` is up-to-date with the latest packages.

```shell
brew update
```

Now let's install the necessary packages.

```shell
brew install cmake openssl protobuf
```

Cool. Now let's install Rust.

```shell
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

Don't forget to update the shell to include the necessary environment variables.

```shell
source ~/.cargo/env
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

You should get output like the following:

```shell
An all-in-one tool for Polkadot development.

Usage: pop <COMMAND>

Commands:
  new    Generate a new parachain, pallet or smart contract
  build  Build a parachain or smart contract
  call   Call a smart contract
  up     Deploy a parachain or smart contract
  test   Test a smart contract
  help   Print this message or the help of the given subcommand(s)

Options:
  -h, --help     Print help
  -V, --version  Print version
```
