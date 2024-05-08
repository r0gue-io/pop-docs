---
description: >-
  These instructions will guide you on installing the necessary environment to
  install Pop CLI and use all its features.
---

# macOS

First we will need the [`brew`](https://brew.sh/) - the macOS package manager. Run the following in your Terminal.

```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
```

Make sure `brew` installed properly.

```
brew --version
```

And let's make sure `brew` is up-to-date with the latest packages.

```
brew update
```

Now let's install the necessary packages.

```
brew install protobuf
```

```
brew install openssl
```

```
brew install cmake
```

Cool. Now let's install Rust.

```
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

Don't forget to update the shell to include the necessary environment variables.

```
source ~/.cargo/env
```

You should now have Rust installed. Double check.

```
rustc --version
```

Now let's configure the Rust toolchain.

```
rustup default stable
rustup update
rustup target add wasm32-unknown-unknown
```

And add nightly.

```
rustup update nightly
rustup target add wasm32-unknown-unknown --toolchain nightly
```

You can confrim your Rust toolchain with the following commands.

```
rustup show
rustup +nightly show
```

The environment has now been setup. We can now install and use Pop CLI.

### Install Pop CLI

To install Pop CLI run the following command

```
cargo install --locked --git https://github.com/r0gue-io/pop-cli
```

Confirm that Pop CLI is installed by running `pop --help`in your terminal

```
pop --help
```

You should get output like the following

```
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
