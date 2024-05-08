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

Make sure the Rust toolchain was installed properly

```
rustup show
rustup +nightly show
```

Awesome. We are now ready to install Pop CLI.

### Install Pop CLI

```
cargo install --locked --git https://github.com/r0gue-io/pop-cli
```

Test out the `pop` command.

```
pop --help
```

You should get output similar to.

<pre><code><strong>An all-in-one tool for Polkadot development.
</strong>
Usage: pop &#x3C;COMMAND>

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
</code></pre>
