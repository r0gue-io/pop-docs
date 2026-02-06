# Deploy a chain with pop up

Use `pop up` to deploy a chain project and optionally register it on a relay chain.

## Prerequisites

- A chain project directory.
- Pop CLI (Command Line Interface) installed.

## Usage

```shell
pop up [--path PATH | PATH] [OPTIONS]
```

## Options

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

## Behavior

- Pop CLI detects a chain project from `PATH` and runs the chain deployment flow.
- If you omit `--id`, Pop CLI reserves a new chain ID during registration.
- If chain specs or genesis artifacts are missing, Pop CLI generates them.
- If you select a deployment provider, Pop CLI requests an API key (reads `PDP_API_KEY` if set).

## Example

```shell
pop up --path ./my-chain --id 2000 --chain-spec ./chain-spec.json \
  --genesis-state ./para-2000-genesis-state --genesis-code ./para-2000.wasm \
  --relay-chain-url wss://rpc.paseo.network --skip-registration
```

## Related guides

- [Launch a local development network](running-your-chain.md)
- [Launch a known chain locally](launch-a-known-chain.md)
- [Deploy a chain to Paseo](launch-a-chain-to-paseo.md)
- [Deploy with Polkadot Deployment Portal](deploy-a-chain-polkadot-deployment-portal.md)
