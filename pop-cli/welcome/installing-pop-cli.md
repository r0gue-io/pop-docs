# Installing Pop CLI

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
