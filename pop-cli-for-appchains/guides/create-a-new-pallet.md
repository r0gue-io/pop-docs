# Create a new pallet

Use the interactive prompt to scaffold a pallet:

```bash
pop new pallet
```

If you run `pop new` without a subcommand, Pop CLI prompts you to choose between chain, pallet, and contract. You can also use the top-level alias `pop n`.

## Command overview

```bash
pop new pallet
pop new pallet <PATH>
pop new pallet advanced
```

## Options

| Argument/Flag | Type | Default | Description |
| --- | --- | --- | --- |
| `<PATH>` | string | interactive | Path or name for the pallet. If omitted, Pop CLI prompts for a path. |
| `--authors`, `-a` | string | `Anonymous` | Author name(s) for the pallet metadata. |
| `--description`, `-d` | string | `Frame Pallet` | Pallet description. |

## Advanced mode

Use `advanced` to unlock more customization:

```bash
pop new pallet <PATH> advanced
```

If you do not pass any advanced flags, Pop CLI runs an interactive prompt. If you pass any advanced flags, Pop CLI skips prompts and uses your inputs.

| Flag | Type | Default | Description |
| --- | --- | --- | --- |
| `--config-common-types`, `-c` | list | none | Add common config types to the config trait. Pass multiple values separated by spaces. |
| `--default-config`, `-d` | boolean | `false` | Add default config implementation (requires at least one config common type). |
| `--storage`, `-s` | list | none | Add storage items to the pallet. Pass multiple values separated by spaces. |
| `--genesis-config`, `-g` | boolean | `false` | Add a genesis config. |
| `--custom-origin`, `-o` | boolean | `false` | Add a custom origin. |

Run `pop new pallet advanced --help` to see the available values for `--config-common-types` and `--storage`.

## Behavior notes

- If the pallet path is inside a Rust workspace, Pop CLI adds the pallet to the workspace manifest.
- Pop CLI runs `cargo fmt --all` after generation. If formatting fails, the pallet is still generated.

## Examples

```bash
# Interactive pallet creation
pop new pallet

# Create a pallet in the current directory
pop new pallet my-pallet

# Create a pallet in a nested directory
pop new pallet pallets/my-pallet

# Advanced interactive mode
pop new pallet my-pallet advanced

# Advanced non-interactive mode
pop new pallet my-pallet advanced --config-common-types runtime-origin currency --storage storage-value storage-map --default-config --genesis-config --custom-origin
```
