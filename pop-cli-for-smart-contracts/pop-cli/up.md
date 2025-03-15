---
description: Upload and instantiate a smart contract
---

# up

```bash
pop up [OPTIONS]
```

{% hint style="info" %}
For Pop CLI versions <`0.7.0` the `pop up` command is `pop up contract`
{% endhint %}

To deploy (upload and instantiate) a smart contract:

```bash
pop up -p ./my_contract --constructor new --args "false" --suri //Alice
```

Alternatively, you can use the `--use-wallet` option for a more secure signing method:
```bash
pop up -p ./my_contract --constructor new --args "false" --use-wallet
```
> ℹ️ If you don't specify a live chain, `pop` will automatically spawn a local node for testing purposes.

Some of the options available are:

* Specify the contract `constructor` to use, which in this example is `new()`.
* Specify the argument (`args`) to the constructor, which in this example is `false`.
* Specify the account uploading and instantiating the contract with `--suri`, which in this example is the default development account of `//Alice`. For other accounts, the actual secret key must be provided e.g. an 0x prefixed 64 bit hex string, or the seed phrase.
    * Alternatively, use the `--use-wallet` option to sign transactions using a browser extension wallet. This is more secure than providing your account's private key directly in the command line. 

> ⚠️ **Use `--suri` only for development**: Using `--suri` should only be done with development accounts. Consider using `--use-wallet` for production accounts.

* You also can specify the url of your node with `--url ws://your-endpoint`, by default it is using `ws://localhost:9944`.

For more information about the options, check [cargo-contract documentation](https://github.com/paritytech/cargo-contract/blob/master/crates/extrinsics/README.md#instantiate).

### Additional options:

``` bash
pop up --help

Usage: pop up [OPTIONS]

Options:
  -p, --path <PATH>
          Path to the contract build directory

  -c, --constructor <CONSTRUCTOR>
          The name of the contract constructor to call
          
          [default: new]

  -a, --args [<ARGS>...]
          The constructor arguments, encoded as strings

  -v, --value <VALUE>
          Transfers an initial balance to the instantiated contract
          
          [default: 0]

  -g, --gas <gas>
          Maximum amount of gas to be used for this command. If not specified it will perform a dry-run to estimate the gas consumed for the instantiation

  -P, --proof-size <PROOF_SIZE>
          Maximum proof size for the instantiation. If not specified it will perform a dry-run to estimate the proof size required

  -S, --salt <SALT>
          A salt used in the address derivation of the new contract. Use to create multiple instances of the same contract code from the same account

  -u, --url <URL>
          Websocket endpoint of a chain
          
          [default: ws://localhost:9944/]

  -s, --suri <SURI>
          Secret key URI for the account deploying the contract.
          
          e.g. - for a dev account "//Alice" - with a password "//Alice///SECRET_PASSWORD"
          
          [default: //Alice]

  -w, --use-wallet
          Use your browser wallet to sign a transaction

  -D, --dry-run
          Perform a dry-run via RPC to estimate the gas usage. This does not submit a transaction

  -U, --upload-only
          Uploads the contract only, without instantiation

  -y, --skip-confirm
          Automatically source or update the needed binary required without prompting for confirmation

  -h, --help
          Print help (see a summary with '-h')
```
