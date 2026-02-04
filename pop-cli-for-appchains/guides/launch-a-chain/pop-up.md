# pop up

Use `pop up` to launch local networks, deploy a chain, deploy a smart contract, start a frontend dev server, or boot a local ink! node.

Aliases: `pop u`, `pop deploy`.

## Usage

```shell
pop up [--path PATH | PATH] [OPTIONS]
pop up network <FILE> [OPTIONS]
pop up paseo [OPTIONS]
pop up kusama [OPTIONS]
pop up polkadot [OPTIONS]
pop up westend [OPTIONS]
pop up frontend [--path PATH]
pop up ink-node [OPTIONS]
```

## Quick Path

Run these if you want the most common flows:

1. `pop up network ./network.toml`
2. `pop up paseo --parachain asset-hub`
3. `pop up --path ./my-chain --id 2000 --skip-registration`
4. `pop up --path ./my-contract --constructor new --args false --suri //Alice --execute`
5. `pop up ink-node --detach`

## Choose Your Flow

1. Launch a local network from a config file: `pop up network <FILE>`
2. Launch a known relay chain locally: `pop up paseo|kusama|polkadot|westend`
3. Deploy a chain project: `pop up --path PATH`
4. Deploy a contract project: `pop up --path PATH`
5. Start a frontend dev server: `pop up frontend [--path PATH]`
6. Start a local ink! node: `pop up ink-node [OPTIONS]`

## Argument Rules

- `--path` and positional `PATH` conflict. Use one or the other.
- Top-level arguments conflict with subcommands. For example, `pop up --path ./my-chain network` is invalid.
If you do not pass a subcommand, Pop CLI detects what to do in this order:
1. If `PATH` is a file, it runs `pop up network <FILE>`.
2. If `PATH` is a contract project, it deploys a smart contract.
3. If `PATH` is a chain project, it deploys a chain.
4. Otherwise, it warns that no contract or chain was detected.

## Deploy a Chain (No Subcommand)

Run `pop up` in a chain project (or pass `--path PATH`) to deploy a chain.

```shell
pop up --path ./my-chain
```

### Options

| Option | Description |
| --- | --- |
| `--id <ID>` | Chain ID to use. If omitted, Pop CLI reserves a new ID. |
| `--skip-registration` | Skip registration and only deploy. Requires `--id`. |
| `--chain-spec <FILE>` | Chain spec file to use when generating genesis artifacts. |
| `--genesis-state <FILE>` | Genesis state file to use. If omitted, Pop CLI generates it. |
| `--genesis-code <FILE>` | Genesis code file to use. If omitted, Pop CLI generates it. |
| `--relay-chain-url <URL>` | Relay chain WebSocket endpoint. |
| `--proxy <ADDRESS>` | Proxied address to act on behalf of. |
| `--profile <PROFILE>` | Build profile (default: release). |

### Prompts and Side Effects

- Prompts you to choose a deployment method (provider or registration only).
- Prompts you to select a relay chain and a relay chain RPC URL.
- Prompts for an API key when using a deployment provider (reads `PDP_API_KEY` if set).
- Prompts for a pure proxy address unless you pass `--proxy` (required for some providers).
- Reserves a new chain ID when `--id` is omitted.
- Generates chain specs and genesis artifacts when files are missing and may prompt for the template used.
- Registers the chain unless you pass `--skip-registration`.

Note: output text may show `pop up rollup`, but there is no `rollup` subcommand. Use `pop up` with a chain project instead.

### Example

```shell
pop up --path ./my-chain --id 2000 --chain-spec ./chain-spec.json \
  --genesis-state ./para-2000-genesis-state --genesis-code ./para-2000.wasm \
  --relay-chain-url wss://rpc.paseo.network --skip-registration
```

## pop up network

Run a local network from a Zombienet configuration file.

```shell
pop up network ./network.toml
```

### Options

| Option | Description |
| --- | --- |
| `<FILE>` | Zombienet network configuration file (positional). |
| `--relay-chain <TAG>` | Relay chain binary version (Polkadot SDK release tag). |
| `--relay-chain-runtime <TAG>` | Relay chain runtime version (Fellows runtime release tag). |
| `--system-parachain <TAG>` | System chain binary version (defaults to relay chain version). |
| `--system-parachain-runtime <TAG>` | System chain runtime version. |
| `--parachain <REPO>` | Git repo URL for a chain, with optional `#ref` and `?binaryname`. Repeatable. |
| `--cmd <COMMAND>` | Command to run after the network launches. |
| `--verbose` | Enable verbose output. |
| `--skip-confirm`, `-y` | Auto-source binaries without prompting. |
| `--rm` | Remove network state on teardown. |
| `--detach` | Run in the background. |

### Prompts and Side Effects

- Warns when required binaries are missing or stale and prompts to source them.
- Downloads binaries into the Pop CLI cache when needed.
- Runs the network using Zombienet.

### Example

```shell
pop up network ./network.toml --relay-chain stable2503 --relay-chain-runtime v1.4.1 \
  --system-parachain stable2503 --system-parachain-runtime v1.4.1 \
  --parachain https://github.com/org/repo?binaryname#v1.2.3 \
  --cmd "ls -la" --verbose --skip-confirm --rm
```

## pop up paseo | kusama | polkadot | westend

Launch a local network using a supported relay chain.

```shell
pop up paseo --parachain asset-hub
```

### Options

| Option | Description |
| --- | --- |
| `--relay-chain <TAG>` | Relay chain binary version (Polkadot SDK release tag). |
| `--relay-chain-runtime <TAG>` | Relay chain runtime version (Fellows runtime release tag). |
| `--system-parachain <TAG>` | System chain binary version (defaults to relay chain version). |
| `--system-parachain-runtime <TAG>` | System chain runtime version. |
| `--parachain <CHAIN>` | Supported chain name. Use `#id` and `:port` when needed (e.g., `asset-hub#1000:9944`). Comma-separated. |
| `--port <PORT>` | Port for the first relay chain validator. |
| `--cmd <COMMAND>` | Command to run after the network launches. |
| `--verbose` | Enable verbose output. |
| `--skip-confirm`, `-y` | Auto-source binaries without prompting. |
| `--rm` | Remove network state on teardown. |
| `--detach` | Run in the background. |

### Prompts and Side Effects

- Auto-adds required dependencies for selected chains and reports what it added.
- Warns when required binaries are missing or stale and prompts to source them.
- Downloads binaries into the Pop CLI cache when needed.

### Example

```shell
pop up kusama --port 8833 --parachain asset-hub#2000:9944 -r stable2503 --skip-confirm
```

## pop up frontend

Start a frontend dev server.

```shell
pop up frontend --path ./frontend
```

### Options

| Option | Description |
| --- | --- |
| `--path <PATH>` | Frontend directory. Defaults to `./frontend` or the current directory if it looks like a frontend project. |

### Prompts and Side Effects

- Prompts you for a path when `./frontend` is missing and the current directory is not a frontend project.
- Runs `install` and `run dev` using the detected package manager.
- Detects package managers in this order: pnpm, bun, yarn, npm.

## pop up ink-node

Start a local ink! node and Ethereum RPC node.

```shell
pop up ink-node --ink-node-port 9944 --eth-rpc-port 8545
```

### Options

| Option | Description |
| --- | --- |
| `--ink-node-port <PORT>` | ink! node port (default: 9944). |
| `--eth-rpc-port <PORT>` | Ethereum RPC port (default: 8545). |
| `--skip-confirm`, `-y` | Auto-source binaries without prompting. |
| `--detach` | Run in the background. |

### Prompts and Side Effects

- Errors if both ports are the same or an explicitly provided port is already in use.
- Downloads ink! node and Ethereum RPC binaries when missing.
- Writes log files to a temporary directory and prints `tail -f` paths.
- Prints a `kill -9` command when running with `--detach`.

## Smart Contract Deployment (No Subcommand)

Run `pop up` in a contract project (or pass `--path PATH`) to deploy a smart contract.

```shell
pop up --path ./my-contract --constructor new --args false --suri //Alice --execute
```

### Options

| Option | Description |
| --- | --- |
| `--constructor <NAME>` | Constructor name (default: `new`). |
| `--args <ARGS...>` | Constructor arguments. |
| `--value <VALUE>` | Balance to transfer to the contract (default: `0`). |
| `--gas <GAS>` | Gas limit. Requires `--proof-size`. |
| `--proof-size <SIZE>` | Proof size limit. Requires `--gas`. |
| `--url <URL>` | Chain WebSocket endpoint. |
| `--suri <SURI>` | Secret key URI (conflicts with `--use-wallet`). |
| `--use-wallet`, `-w` | Use a browser wallet to sign. |
| `--execute`, `-x` | Execute the deployment. Otherwise, Pop CLI runs a dry run. |
| `--upload-only`, `-U` | Upload without instantiating. |
| `--skip-confirm`, `-y` | Skip prompts and auto-start a local ink! node when needed. |
| `--skip-build` | Skip building the contract before deployment. |

### Prompts and Side Effects

- Builds your contract if artifacts are missing (even when `--skip-build` is set).
- Prompts you to pick a signer unless you pass `--suri` or `--use-wallet`.
- Prompts you to select a chain RPC when `--url` is omitted.
- Starts a local ink! node if you target the local endpoint and it is not running.
- Runs a dry run first, then prompts to deploy when `--execute` is not set.
- Prompts to keep interacting with the contract after deployment (unless `--skip-confirm`).
