---
description: Upgrade Polkadot SDK dependency versions in a chain project with Pop CLI.
---

# Upgrade Polkadot SDK dependencies

Use `pop upgrade` to update Polkadot SDK dependency versions in a chain project. It rewrites entries in your `Cargo.toml` using a known version mapping.

You can also run the command as `pop ug`.
Use this command in a chain project that has a `Cargo.toml`. Network access is required to fetch available Polkadot SDK versions.

## Usage

```bash
pop upgrade [--path <PATH>] [--version <VERSION>]
```

### JSON mode

Use global `--json` for structured output in automation:

```bash
pop --json upgrade --path /path/to/project --version polkadot-stable2509-1
```

In JSON mode, `--version` is required because interactive version selection is disabled.

## Examples

```bash
# Upgrade the current project and select a version interactively
pop upgrade

# Upgrade a specific project directory
pop upgrade --path /path/to/project

# Upgrade a specific Cargo.toml
pop upgrade --path /path/to/project/Cargo.toml

# Upgrade to a specific Polkadot SDK tag
pop upgrade --version polkadot-stable2509-1
```

## How version selection works

If you omit `--version`, Pop fetches available Polkadot SDK release tags from GitHub and prompts you to select one. The prompt is filterable as you type.

If you pass `--version`, Pop skips the prompt and uses your value directly.

## Path resolution

- If `--path` points to a `Cargo.toml`, Pop uses it directly.
- If `--path` points to a directory, Pop appends `Cargo.toml`.
- If you omit `--path`, Pop uses the `Cargo.toml` in your current directory.

## Output and follow-up

After the update, Pop prints a warning that the upgrade may introduce compile errors and recommends running `pop build`. It also prints a success message with the version you selected.

## Errors and constraints

- Missing `Cargo.toml`: `Cargo.toml file not found at specified path`.
- Version mapping fetch failed: `Failed to get version mapping: <error>`.
- Dependency update failed: `Failed to update dependencies: <error>`.
