# Create a new chain

Use the interactive prompt to scaffold a chain:

```bash
pop new chain
```

If you run `pop new` without a subcommand, Pop CLI prompts you to choose between chain, pallet, and contract. You can also use the top-level alias `pop n`.

## Command overview

```bash
pop new --list
pop new chain --list
pop new chain <NAME>
```

`pop new --list` prints the available chain and contract templates (based on which features are enabled) and exits. `pop new chain --list` prints only chain templates and exits.

### Command map

- `pop new` (alias: `pop n`)
- `pop new chain` (aliases: `pop new C`, `pop new p`, `pop new parachain`)
- `pop new pallet` (alias: `pop new P`)
- `pop new contract` (alias: `pop new c`)

## Templates

Available templates:

- Pop: Standard, Assets, Contracts
- OpenZeppelin: Generic Runtime Template, EVM Template
- Parity: Polkadot SDK's Parachain Template

Pop CLI validates provider and template compatibility. If you choose a template that the provider does not support, the command exits with an error. Deprecated templates still appear in the prompt, but Pop CLI warns when you select them.

> Note: Some upstream template names and binaries still say "parachain", this is a Polkadot Chain.

## Options

| Flag | Type | Default | Description |
| --- | --- | --- | --- |
| `--provider` | string | `pop` | Template provider (`pop`, `openzeppelin`, `parity`). |
| `--template`, `-t` | string | provider default | Template name. |
| `--release-tag`, `-r` | string | latest | Release tag to use for the template. |
| `--symbol`, `-s` | string | `UNIT` | Token symbol. |
| `--decimals`, `-d` | number | `12` | Token decimals. |
| `--endowment`, `-e` | string | `1u64 << 60` | Initial endowment for dev accounts. |
| `--verify`, `-v` | boolean | `false` | Verify commit SHA when fetching license and releases. |
| `--list`, `-l` | boolean | `false` | List templates and exit. |
| `--with-frontend`, `-f[=<TEMPLATE>]` | string | none | Scaffold a frontend template. Use `=` when providing a value. |
| `--package-manager` | string | auto | Package manager for frontend scaffolding. Requires `--with-frontend`. |

Token customization options (`--symbol`, `--decimals`, `--endowment`) are only supported for Pop templates and the Parity Generic template. If you pass these options for other templates, Pop CLI warns and proceeds with defaults.

### Endowment validation

`--endowment` accepts a plain integer (for example `1000000`) or a left-shift expression (`1u64 << 60`). If Pop CLI cannot parse the value, it warns and asks whether to fall back to the default endowment. If you decline, the command exits without generating a chain.

### Release and license selection

Pop CLI prints the template license before generation. It then offers the latest three releases that match the template's supported versions. If no matching releases are found, Pop CLI warns and uses the default branch.

## Examples

```bash
# Interactive chain creation
pop new chain

# List chain templates
pop new chain --list

# Specify provider/template explicitly
pop new chain my-chain --provider pop --template r0gue-io/base-parachain

# Provide token customization in CLI mode
pop new chain my-chain --symbol DOT --decimals 10 --endowment 1000000
```

## Adding a frontend

You can scaffold your chain with a frontend template using the `--with-frontend` flag. Pop CLI supports the following community frontend template for chains:

- [create-dot-app](https://github.com/polkadot-developers/create-dot-app) - Full-stack dApp boilerplate for Polkadot chains

### Interactive mode

The interactive prompt will ask you if you want to include a frontend template:

```bash
pop new chain
```

If you choose to add a frontend, Pop CLI auto-selects the frontend template because only one exists. If you pass `--with-frontend` without a value, Pop CLI uses the same auto-selection behavior.

### CLI mode

You can specify the frontend template directly. When you provide a value, you must use `=`:

```bash
# With default frontend template selection
pop new chain my-chain --with-frontend

# With specific frontend template
pop new chain my-chain --with-frontend=create-dot-app
```

### Frontend dependencies

Pop CLI will automatically check for required dependencies and prompt you to install them if not present:

- Node.js (version 20 or later)

You can also install frontend dependencies separately using:

```bash
pop install -y --frontend
```

### Package manager selection

`--package-manager` only works with `--with-frontend`. If you do not provide it, Pop CLI auto-detects in this order: `pnpm`, `bun`, `yarn`, `npm`.

### Running the frontend

After scaffolding your chain with a frontend, you can start the frontend development server from your generated chain folder with:

```bash
pop up frontend
```

This command starts the frontend dev server for your chain project.

**Need help?**

Ask on [Polkadot Stack Exchange](https://polkadot.stackexchange.com/) (tag it [`pop`](https://substrate.stackexchange.com/tags/pop/info)) or drop by [our Telegram](https://t.me/onpopio). We're here to help!
