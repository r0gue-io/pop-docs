---
description: Remove cached artifacts, stop local nodes, and stop local networks with pop clean.
---

# Clean with Pop CLI

Use `pop clean` to remove cached artifacts, stop local nodes, or stop local networks. The command supports interactive selection and non-interactive cleanup with flags.

## Prerequisites

- Pop CLI (Command Line Interface) installed.
- For `pop clean network`, use Pop CLI built with the `chain` feature. If your CLI does not include it, you will see `network cleanup requires the \`chain\` feature`.
- For `pop clean node`, you need `pgrep`, `lsof`, and `kill` available in your `PATH`.

## Usage

```bash
pop clean <subcommand> [options]
```

## Subcommands

| Subcommand | Alias | Purpose |
| --- | --- | --- |
| `cache` | `c` | Remove cached artifacts from the Pop cache directory. |
| `node` | `n` | Stop local `ink-node`, `eth-rpc`, and `pop fork` processes. |
| `network` | `net` | Stop running networks and optionally remove their state. |

## Clean cached artifacts

`pop clean cache` removes cached artifacts from the Pop cache directory. Pop resolves the cache path with `dirs::cache_dir()` and uses a `pop` subdirectory. It creates the directory if it does not exist.

If the cache directory is missing or empty, Pop prints an info message and exits.

### Flags

| Flag | Description |
| --- | --- |
| `--all` | Remove all cached artifacts without prompting. |
| `--pid <PID...>` | Accepted but ignored for cache cleanup. |

### Interactive flow

If you do not pass `--all`, Pop shows a multiselect list of artifacts and their sizes in MiB. Pop ignores dotfiles. After you select artifacts, Pop asks for confirmation before removal.

### Examples

```bash
# Select specific cache artifacts to remove
pop clean cache

# Remove all cached artifacts without prompting
pop clean cache --all
```

## Clean running nodes

`pop clean node` stops local `ink-node`, `eth-rpc`, and `pop fork` processes. Pop uses `pgrep` to find PIDs, `lsof` to determine listening ports, and `kill -9` to terminate processes.

If no matching processes are running, Pop prints an info message and exits.

### Flags

| Flag | Description |
| --- | --- |
| `--all` | Kill all matching processes without prompting. |
| `--pid <PID...>` | Kill the specified process IDs (PIDs). All PIDs must be valid or Pop cancels the operation. |

### Interactive flow

If you do not pass `--all` or `--pid`, Pop shows a multiselect list of processes and their listening ports. After you select processes, Pop asks for confirmation before killing them.

Use `pop clean node --pid <PID>` to stop a specific process by PID.

### Examples

```bash
# Select which local nodes to stop
pop clean node

# Stop all local nodes without prompting
pop clean node --all

# Stop specific node processes by PID
pop clean node --pid 1234 5678
```

## Clean running networks

`pop clean network` stops running networks based on `zombie.json` files. Pop discovers networks by scanning the system temporary directory for folders named `zombie-<uuid>` that contain `zombie.json`, and it skips any folder that contains a `.CLEARED` marker file.

If no running networks are found, Pop prints an info message and exits.

### Flags and arguments

| Flag or argument | Description |
| --- | --- |
| `PATH` | Optional path to a `zombie.json` file or a directory that contains one. |
| `--all` | Stop all discovered networks without prompting. |
| `--keep-state` | Stop networks but keep their state on disk. Pop writes a `.CLEARED` marker to the network directory. |

### Interactive flow

If you do not pass `--all` or `PATH`, Pop selects the only running network automatically or shows a multiselect list when multiple networks exist. Pop prompts for confirmation when you make a selection in an interactive terminal.

### Behavior notes

- If you pass a `PATH`, Pop errors when it cannot find a `zombie.json` file at the path.
- When `--keep-state` is set, Pop does not remove the network directory and writes `.CLEARED`. This also keeps the network from showing up in future `--all` scans.
- If stopping a network fails with an attached or provider error, Pop falls back to killing the process IDs listed in `zombie.json`.

### Examples

```bash
# Select which networks to stop
pop clean network

# Stop all networks without prompting
pop clean network --all

# Stop a network using a known zombie.json path
pop clean network /tmp/zombie-123e4567-e89b-12d3-a456-426614174000/zombie.json

# Stop a network but keep its state on disk
pop clean network --keep-state
```
