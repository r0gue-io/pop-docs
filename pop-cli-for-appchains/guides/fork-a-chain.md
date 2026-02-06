---
description: Fork a live chain and run a local RPC server with pop fork.
---

# Fork a chain with Pop CLI

Use `pop fork` (alias: `pop f`) to fork a live chain locally and start an RPC server.

## Prerequisites

- Pop CLI (Command Line Interface) built with the `chain` feature. See [Install Pop CLI](../install-pop-cli.md) or run `cargo install --force --locked pop-cli --features chain`.

## Usage

```bash
pop fork -e <ENDPOINT>... [options]
```

## Flags and arguments

| Flag or argument | Required | Description |
| --- | --- | --- |
| `-e, --endpoint <ENDPOINT>...` | Yes | RPC endpoint URL(s) to fork. Repeat for multiple chains. Parsed as a URL. |
| `-c, --cache <PATH>` | No | Path to a SQLite cache file. If omitted, Pop uses an in-memory cache. When you fork multiple endpoints, Pop appends `_{index}` before the file extension. |
| `-p, --port <PORT>` | No | Starting port for RPC servers. When you fork multiple endpoints, Pop increments the port after each server starts. |
| `--mock-all-signatures` | No | Accept all signatures as valid for dev/testing. Default is to accept only magic signatures (`0xdeadbeef`). Do not use for production or security-sensitive scenarios. |
| `--log-level <LOG_LEVEL>` | No | Log level for block building. Default: `info`. Values: `off`, `error`, `warn`, `info`, `debug`, `trace`. |
| `-d, --detach` | No | Run the fork in the background and return immediately. Pop prints the process ID (PID) and a log file path. |

## Behavior notes

- Pop waits for Ctrl+C. On shutdown, it stops all servers, clears local storage, and warns if cleanup fails.
- When you use `--detach`, Pop starts a background process, writes output to a log file, and returns immediately. To stop it, run `pop clean node --pid <PID>` or `kill -9 <PID>`.
- If you do not specify a fork point, Pop uses the latest finalized block from the remote RPC as the fork point.
- Storage is fetched lazily from the live chain and cached in SQLite. With `--cache`, the cache is persisted on disk. Without it, Pop uses an in-memory cache.

## Examples

```bash
# Fork a live chain (uses latest finalized block)
pop fork -e wss://rpc.polkadot.io
```

```bash
# Fork at a specific port
pop fork -e wss://rpc.polkadot.io --port 9944
```

```bash
# Fork with persistent cache for fast restarts
pop fork -e wss://rpc.polkadot.io --cache ./polkadot-fork.db
```

```bash
# Fork with signature mocking (accept any signature)
pop fork -e wss://rpc.polkadot.io --mock-all-signatures
```

```bash
# Control log verbosity
pop fork -e wss://rpc.polkadot.io --log-level info
```

```bash
# Fork multiple chains
pop fork -e wss://rpc.polkadot.io -e wss://asset-hub-polkadot-rpc.polkadot.io
```

```bash
# Fork in the background and return immediately
pop fork -e wss://rpc.polkadot.io --detach
```

## Example workflow

1. Start the fork.

```bash
pop fork -e wss://westend-rpc.polkadot.io --port 9944
```

2. Connect a client to the local RPC endpoint in Polkadot.js Apps.

```text
ws://127.0.0.1:9944
```

3. Submit transactions and query state against the local RPC endpoint.

## Supported RPC methods

Use `rpc_methods` to list every supported method at runtime. The fork RPC server exposes these namespaces and methods:

| Namespace | Methods |
| --- | --- |
| `chain` | `chain_getBlockHash`, `chain_getHeader`, `chain_getBlock`, `chain_getFinalizedHead`, `chain_subscribeNewHeads`, `chain_unsubscribeNewHeads`, `chain_subscribeFinalizedHeads`, `chain_unsubscribeFinalizedHeads`, `chain_subscribeAllHeads`, `chain_unsubscribeAllHeads` |
| `state` | `state_getStorage`, `state_getMetadata`, `state_getRuntimeVersion`, `state_getKeysPaged`, `state_call`, `state_queryStorageAt`, `state_subscribeRuntimeVersion`, `state_unsubscribeRuntimeVersion`, `state_subscribeStorage`, `state_unsubscribeStorage` |
| `system` | `system_chain`, `system_name`, `system_version`, `system_health`, `system_properties`, `system_localPeerId`, `system_nodeRoles`, `system_localListenAddresses`, `system_chainType`, `system_syncState`, `system_accountNextIndex` |
| `author` | `author_submitExtrinsic`, `author_pendingExtrinsics` |
| `dev` | `dev_newBlock` |

Additional namespaces are available, including `archive`, `chainHead`, `chainSpec`, `payment`, and `transaction`. Use `rpc_methods` to see the full list.

Polkadot.js compatibility aliases are also registered for `chain_subscribeNewHead` and `chain_unsubscribeNewHead`.
```
