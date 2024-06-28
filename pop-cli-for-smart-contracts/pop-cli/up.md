---
description: Upload and instantiate a smart contract
---

# up

```bash
pop up contract [OPTIONS]
```

To deploy (upload and instantiate) a smart contract:

```bash
pop up contract -p ./my_contract --constructor new --args "false" --suri //Alice
```

> ℹ️ If you don't specify a live chain, `pop` will automatically spawn a local node for testing purposes.

Some of the options available are:

* Specify the contract `constructor` to use, which in this example is `new()`.
* Specify the argument (`args`) to the constructor, which in this example is `false`.
* Specify the account uploading and instantiating the contract with `--suri`, which in this example is the default development account of `//Alice`. For other accounts, the actual secret key must be provided e.g. an 0x prefixed 64 bit hex string, or the seed phrase.

> ⚠️ **Use only for development**: Use a safer method of signing here before using this feature with production projects. We will be looking to provide alternative solutions in the future!

* You also can specify the url of your node with `--url ws://your-endpoint`, by default it is using `ws://localhost:9944`.

For more information about the options, check [cargo-contract documentation](https://github.com/paritytech/cargo-contract/blob/master/crates/extrinsics/README.md#instantiate).



**Additional options:**

<pre><code>pop up contract --help

Deploy a smart contract to a node

<strong>Usage: pop up contract [OPTIONS]
</strong>
Options:
  -p, --path &#x3C;PATH>
          Path to the contract build folder

      --constructor &#x3C;constructor>
          The name of the contract constructor to call
          
          [default: new]

      --args [&#x3C;ARGS>...]
          The constructor arguments, encoded as strings

      --value &#x3C;value>
          Transfers an initial balance to the instantiated contract
          
          [default: 0]

      --gas &#x3C;gas>
          Maximum amount of gas to be used for this command. If not specified it will perform a dry-run to estimate the gas consumed for the instantiation

      --proof-size &#x3C;PROOF_SIZE>
          Maximum proof size for the instantiation. If not specified it will perform a dry-run to estimate the proof size required

      --salt &#x3C;SALT>
          A salt used in the address derivation of the new contract. Use to create multiple instances of the same contract code from the same account

      --url &#x3C;url>
          Websocket endpoint of a node
          
          [default: ws://localhost:9944]

  -s, --suri &#x3C;suri>
          Secret key URI for the account deploying the contract.
          
          e.g. - for a dev account "//Alice" - with a password "//Alice///SECRET_PASSWORD"
          
          [default: //Alice]

      --dry-run
          Perform a dry-run via RPC to estimate the gas usage. This does not submit a transaction

  -y, --skip-confirm
          Before start a local node, do not ask the user for confirmation
</code></pre>
