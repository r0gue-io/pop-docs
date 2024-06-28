---
description: Call a contract
---

# call

```bash
pop call <COMMAND>
```

**Interacting with the smart contract**

Read-only Operations: For operations that only require reading from the blockchain state. This approach does not require to submit an extrinsic. Example using the get() message:

```bash
pop call contract -p ./my_contract --contract $INSTANTIATED_CONTRACT_ADDRESS --message get --suri //Alice
```

2. State-modifying Operations: For operations that change a storage value, thus altering the blockchain state. Include the `x / --execute` flag to submit an extrinsic on-chain.

Example executing the `flip()` message:

```bash
pop call contract -p ./my_contract --contract $INSTANTIATED_CONTRACT_ADDRESS --message flip --suri //Alice -x
```

**Additional options:**

```
pop call contract --help

Call a contract

Usage: pop call contract [OPTIONS] --contract <contract> --message <MESSAGE> --suri <suri>

Options:
  -p, --path <PATH>
          Path to the contract build folder

      --contract <contract>
          The address of the contract to call
          
          [env: CONTRACT=]

  -m, --message <MESSAGE>
          The name of the contract message to call

      --args [<ARGS>...]
          The constructor arguments, encoded as strings

      --value <value>
          Transfers an initial balance to the instantiated contract
          
          [default: 0]

      --gas <gas>
          Maximum amount of gas to be used for this command. If not specified it will perform a dry-run to estimate the gas consumed for the instantiation

      --proof-size <PROOF_SIZE>
          Maximum proof size for this command. If not specified it will perform a dry-run to estimate the proof size required

      --url <url>
          Websocket endpoint of a node
          
          [default: ws://localhost:9944]

  -s, --suri <suri>
          Secret key URI for the account deploying the contract.
          
          e.g. - for a dev account "//Alice" - with a password "//Alice///SECRET_PASSWORD"

  -x, --execute
          Submit an extrinsic for on-chain execution

      --dry-run
          Perform a dry-run via RPC to estimate the gas usage. This does not submit a transaction
```
