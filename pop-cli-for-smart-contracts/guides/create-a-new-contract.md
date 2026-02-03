# Create a new contract

Use the interactive prompt to scaffold a contract:

```bash
pop new contract
```

If you run `pop new` without a subcommand, Pop CLI prompts you to choose between chain, pallet, and contract. You can also use the top-level alias `pop n`.

## Command overview

```bash
pop new --list
pop new contract --list
pop new contract <NAME>
```

`pop new --list` prints the available chain and contract templates (based on which features are enabled) and exits. `pop new contract --list` prints only contract templates and exits.

### Command map

- `pop new` (alias: `pop n`)
- `pop new chain` (aliases: `pop new C`, `pop new p`, `pop new parachain`)
- `pop new pallet` (alias: `pop new P`)
- `pop new contract` (alias: `pop new c`)

## Options

| Flag | Type | Default | Description |
| --- | --- | --- | --- |
| `--template`, `-t` | string | template default | Contract template name. |
| `--list`, `-l` | boolean | `false` | List templates and exit. |
| `--with-frontend`, `-f[=<TEMPLATE>]` | string | none | Scaffold a frontend template. Use `=` when providing a value. |
| `--package-manager` | string | auto | Package manager for frontend scaffolding. Requires `--with-frontend`. |

## Examples

```bash
# Interactive contract creation
pop new contract

# List contract templates
pop new contract --list

# Create a new contract using a template
pop new contract my_erc20 --template erc20
```

Pop CLI validates the contract name derived from your path. If it is invalid, the command stops without generating a contract.

## Adding a frontend

You can scaffold your contract with a frontend template using the `--with-frontend` flag. Pop CLI supports the following community frontend templates for contracts:

- [inkathon](https://github.com/scio-labs/inkathon) - Full-stack dApp boilerplate for ink! smart contracts
- [typink](https://github.com/dedotdev/typink) - Type-safe frontend framework for ink! contracts

### Interactive mode

The interactive prompt will ask you to select a frontend template:

```bash
pop new contract my_contract
```

If you pass `--with-frontend` without a value, Pop CLI prompts you to pick a frontend template.

### CLI mode

You can specify the frontend template directly. When you provide a value, you must use `=`:

```bash
# With default frontend template selection
pop new contract my_contract --with-frontend

# With specific frontend template
pop new contract flipper --with-frontend=typink
pop new contract my_contract --with-frontend=inkathon
```

### Frontend dependencies

Pop CLI will automatically check for required dependencies and prompt you to install them if not present:

- Node.js (version 20 or later)
- Bun (required for inkathon template)

You can also install frontend dependencies separately using:

```bash
pop install -y --frontend
```

### Package manager selection

`--package-manager` only works with `--with-frontend`. If you do not provide it, Pop CLI auto-detects in this order: `pnpm`, `bun`, `yarn`, `npm`. The `inkathon` template requires Bun, so Pop CLI ignores any other package manager you specify for `inkathon`.

## Behavior notes

- If the contract path is inside a Rust workspace, Pop CLI adds the contract to the workspace manifest.

### Running the frontend

After scaffolding your contract with a frontend, you can start the frontend development server from your generated chain folder with:

```bash
pop up frontend
```

This command starts the frontend dev server for your contract project.

**Need help?**

Ask on [Polkadot Stack Exchange](https://polkadot.stackexchange.com/) (tag it [`pop`](https://substrate.stackexchange.com/tags/pop/info)) or drop by [our Telegram](https://t.me/onpopio). We're here to help!
