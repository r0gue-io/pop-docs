---
description: Clean Pop CLI caches, local nodes, or running networks.
---

# Clean local resources

Use `pop clean` to remove cached artifacts, stop local nodes, or shut down running networks created by Pop.

You can also run the command as `pop C`.

## Subcommands

- `cache` (alias: `c`): remove cached artifacts.
- `node` (alias: `n`): stop local `ink-node` and `eth-rpc` processes.
- `network` (alias: `net`): stop local networks created by `pop up`.

## Usage

```bash
pop clean <cache|node|network> [OPTIONS] [PATH]
```

## Examples

```bash
# Clean cache entries interactively
pop clean cache

# Remove all cache entries without prompting
pop clean cache --all

# Stop all detected local nodes
pop clean node --all

# Stop specific node PIDs (all must be valid)
pop clean node --pid 1234 5678

# Stop a single network by path to zombie.json
pop clean network /path/to/zombie.json

# Stop a network by directory containing zombie.json
pop clean network /path/to/network-dir

# Stop all networks
pop clean network --all

# Stop a network but keep its state on disk
pop clean network --keep-state
```

## Cache cleanup

`pop clean cache` uses the Pop cache directory at `~/.cache/pop` (or your OS equivalent).

- Without `--all`, Pop prompts you to select which artifacts to delete and asks for confirmation.
- With `--all`, Pop deletes everything without confirmation.
- `--pid` is accepted but has no effect for cache cleanup.

If the cache directory does not exist or is empty, Pop prints a message and exits without changes.

## Node cleanup

`pop clean node` looks for local `ink-node`, `eth-rpc`, and detached `pop fork` processes.

- With `--all`, Pop kills every detected process without confirmation.
- With `--pid`, Pop validates every PID against the detected list. If any PID is invalid, Pop cancels and kills nothing.
- Without `--all` or `--pid`, Pop prompts you to select processes and confirm.

Pop uses `pgrep`, `lsof`, and `kill -9` for detection and shutdown.

If you started a detached fork with `pop fork --detach`, you can stop it with `pop clean node --pid <PID>`.

## Network cleanup

`pop clean network` stops networks created by Pop using Zombienet configs.

- If you provide a `PATH`, it must be a `zombie.json` file or a directory containing `zombie.json`.
- With `--all`, Pop stops all discovered networks without prompting.
- Without `PATH` or `--all`, Pop searches your temp directory for `zombie-<uuid>/zombie.json` entries (skipping any marked with `.CLEARED`) and prompts you to select and confirm.
- With `--keep-state`, Pop stops the network but keeps its state on disk and writes a `.CLEARED` marker.

`pop clean network` requires the `chain` feature. If you installed a contract-only build, this subcommand fails with: `network cleanup requires the \`chain\` feature`.

## Errors and constraints

- Missing `zombie.json` path: the path must point to `zombie.json` or a directory that contains it.
- Missing cache directory or empty cache: Pop exits without making changes.
