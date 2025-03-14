---
description: Call a contract
---

# call

```bash
pop call <COMMAND>
```

```bash
pop call contract
```

The above will provide interactive guidance to interact with the smart contract.

If interactive guidance is not required, you have to provide all the required arguments.

Read-only Operations: For operations that only require reading from the blockchain state. This approach does not require to submit an extrinsic. Example using the get() message:

```bash
pop call contract -p ./my_contract --contract $INSTANTIATED_CONTRACT_ADDRESS --message get --suri //Alice
```

State-modifying Operations: For operations that change a storage value, thus altering the blockchain state. Include the `x / --execute` flag to submit an extrinsic on-chain.

Example executing the `flip()` message:

```bash
pop call contract -p ./my_contract --contract $INSTANTIATED_CONTRACT_ADDRESS --message flip --suri //Alice -x
```

**Wallet Signing Portal for Browser Extension Signing**\
Pop CLI includes a `--use-wallet` option that opens a signing portal, allowing you to sign transactions using a browser extension wallet. This eliminates the need to provide your account's private key directly in the command line. `--suri` can not be used with `--use-wallet`

Example usage of `--use-wallet`:

```bash
pop call contract -p ./my_contract --use-wallet --contract $INSTANTIATED_CONTRACT_ADDRESS --message flip
```

### Additional options:

````bash

``` bash
pop call contract --help

Usage: pop call contract [OPTIONS]

Options:
  -p, --path <PATH>
          Path to the contract build directory or a contract artifact

  -c, --contract <CONTRACT>
          The address of the contract to call

          [env: CONTRACT=]

  -m, --message <MESSAGE>
          The name of the contract message to call

  -a, --args [<ARGS>...]
          The message arguments, encoded as strings

  -v, --value <VALUE>
          The value to be transferred as part of the call

          [default: 0]

  -g, --gas <gas>
          Maximum amount of gas to be used for this command. If not specified it will perform a dry-run to estimate the gas consumed for the call

  -P, --proof-size <PROOF_SIZE>
          Maximum proof size for this command. If not specified it will perform a dry-run to estimate the proof size required

  -u, --url <URL>
          Websocket endpoint of a node

          [default: ws://localhost:9944/]

  -s, --suri <SURI>
          Secret key URI for the account calling the contract.

          e.g. - for a dev account "//Alice" - with a password "//Alice///SECRET_PASSWORD"

          [default: //Alice]

  -w, --use-wallet
          Use a browser extension wallet to sign the extrinsic

  -x, --execute
          Submit an extrinsic for on-chain execution

  -D, --dry-run
          Perform a dry-run via RPC to estimate the gas usage. This does not submit a transaction

  -d, --dev
          Enables developer mode, bypassing certain user prompts for faster testing. Recommended for testing and local development only

  -h, --help
          Print help (see a summary with '-h')
````
