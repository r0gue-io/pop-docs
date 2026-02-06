---
description: Fork a live chain and run a local RPC server with pop fork.
---

# Fork a chain with Pop CLI

Use `pop fork` (alias: `pop f`) to fork a live chain locally and start an RPC server.

## Prerequisites

- Pop CLI (Command Line Interface) built with the `chain` feature.

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
| `--mock-all-signatures` | No | Accept all signatures as valid. Default is to accept only magic signatures (`0xdeadbeef`). |
| `--log-level <LOG_LEVEL>` | No | Log level for block building. Default: `info`. Values: `off`, `error`, `warn`, `info`, `debug`, `trace`. |

## Behavior notes

- Pop prints an intro, forks each endpoint, starts an RPC server, and prints a success message for each fork.
- Pop waits for Ctrl+C. On shutdown, it stops all servers, clears local storage, and warns if cleanup fails.

## Examples

```bash
# Fork a single chain
pop fork -e wss://rpc.polkadot.io
```

```bash
# Fork multiple chains with cache and a starting port
pop fork -e wss://rpc.polkadot.io -e wss://rpc.paseo.io -c /tmp/pop-fork.db -p 9000 --log-level debug
```
