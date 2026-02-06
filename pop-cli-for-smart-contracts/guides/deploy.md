# Deploy

Deploy an ink! smart contract with `pop up`.

## Prerequisites

- A contract project built with `pop build`.
- Pop CLI (Command Line Interface) installed.

## Local deployment (default)

If you omit `--url`, Pop CLI targets `ws://127.0.0.1:9944`. If that endpoint is not running, Pop CLI can start a local [ink-node](https://github.com/use-ink/ink-node) (and Ethereum RPC) in the background. Use `--skip-confirm` to auto-start when needed.

```bash
pop up --path ./path-to-contract \
  --constructor <constructor_name> \
  --args <arg_1> <arg_2> ... \
  --suri //Alice \
  --execute
```

### Common options

* `--path`: points to the contract directory
* `--constructor`: method name (default: `new`)
* `--args`: constructor arguments
* `--value`: balance transferred to the contract (default: `0`)
* `--execute`: deploys the contract (otherwise Pop CLI runs a dry run)
* `--suri`: secret key URI (default: `//Alice`)
* No `--url`: defaults to the local endpoint and can start a local ink-node if needed
* `--use-wallet`: sign with a browser wallet (conflicts with `--suri`)
* `--upload-only`: upload without instantiating
* `--gas` and `--proof-size`: override the dry-run estimate (must be provided together)
* `--skip-build`: skip building (Pop CLI still builds if artifacts are missing)
* `--skip-confirm`: skip prompts and auto-start a local ink-node if needed

> Tip: Use `pop up ink-node --detach` to keep a local node running and follow the printed `kill -9` command to shut it down.

When deployment succeeds, Pop CLI prints the contract address. Save it for calls and queries.

## Deploy to a custom or public network

Provide a WebSocket endpoint with `--url`:

```bash
pop up --path ./my_contract \
  --url ws://<network-endpoint> \
  --constructor <name> \
  --args <arg_1> <arg_2>

# Option A: sign with a suri
pop up --path ./my_contract --url ws://<network-endpoint> --constructor <name> \
  --args <arg_1> <arg_2> --suri //Alice --execute

# Option B: sign with a browser wallet
pop up --path ./my_contract --url ws://<network-endpoint> --constructor <name> \
  --args <arg_1> <arg_2> --use-wallet --execute
```

To sign via a browser wallet, see [Securely sign transactions from CLI](securely-sign-transactions-from-cli.md).

## Gas estimation

Pop CLI performs a dry run to estimate [gas](https://use.ink/basics/gas). If you do not pass `--execute`, Pop CLI keeps the dry-run result and asks for confirmation before deploying.

```bash
pop up --path ./my_contract --constructor new --args false --suri //Alice
```

To override the estimate, provide both `--gas` and `--proof-size`:

```bash
pop up --path ./my_contract --constructor new --args false --suri //Alice \
  --gas 1000000 --proof-size 20000 --execute
```

> You can also upload without instantiating by adding `--upload-only`. Use `--execute` to submit the upload. For more details, see the [ink! docs](https://use.ink/docs/v6/getting-started/deploy-your-contract).

**Need help?**

Ask on [Polkadot Stack Exchange](https://polkadot.stackexchange.com/) (tag it [`pop`](https://substrate.stackexchange.com/tags/pop/info)) or drop by [our Telegram](https://t.me/onpopio). We're here to help!
