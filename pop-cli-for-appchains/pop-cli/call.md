---
description: Call a chain
---

# call

```bash
pop call <COMMAND>
```

**Interacting with the chain**

```bash
pop call chain
```

You will be prompted to select a pallet, the dispatchable function and its required arguments, connection details for the chain, and the account to sign the transaction.

If interactive guidance is not desired, you can proceed manually as follows:

```bash
pop call chain --pallet System --function remark --args "0x11" --url ws://localhost:9944/ --suri //Alice --skip-confirm
```

To dispatch a call with Root origin when the chain's runtime includes `pallet-sudo`, you can wrap the call in a `sudo.sudo()` call by using the `--sudo` flag:

```bash
pop call chain --pallet System --function remark --args "0x11" --url ws://localhost:9944/ --suri //Alice --sudo
```

If you already have the `SCALE`-encoded call data and want to submit the extrinsic directly, use `--chain` as shown below:

```shell
pop call chain --call 0x00000411 --url ws://localhost:9944/ --suri //Alice
```

**Wallet Signing Portal for Browser Extension Signing**  
Pop CLI includes a `--use-wallet` option that opens a signing portal, allowing you to sign transactions using a browser extension wallet.
This eliminates the need to provide your account's private key directly in the command line. `--suri` can not be used with `--use-wallet`.

Example usage of `--use-wallet`:
```bash
pop call chain --call 0x00000411 --url ws://localhost:9944/ --use-wallet
```

### Additional options:

``` bash
pop call chain --help                                                     

Call a chain

Usage: pop call chain [OPTIONS]

Options:
  -p, --pallet <PALLET>
          The pallet containing the dispatchable function to execute

  -f, --function <FUNCTION>
          The dispatchable function to execute within the specified pallet

  -a, --args [<ARGS>...]
          The dispatchable function arguments, encoded as strings

  -u, --url <URL>
          Websocket endpoint of a node

  -s, --suri <SURI>
          Secret key URI for the account signing the extrinsic.
          
          e.g. - for a dev account "//Alice" - with a password "//Alice///SECRET_PASSWORD"

  -w, --use-wallet
          Use a browser extension wallet to sign the extrinsic

  -c, --call <call>
          SCALE encoded bytes representing the call data of the extrinsic

  -S, --sudo
          Authenticates the sudo key and dispatches a function call with `Root` origin

  -y, --skip-confirm
          Automatically signs and submits the extrinsic without prompting for confirmation

  -h, --help
          Print help (see a summary with '-h')

```
