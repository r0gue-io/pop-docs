# Create a new chain

Use the interactive prompt to scaffold a chain:

```bash
pop new chain
```

Available templates:

- Pop
  - Standard
  - Assets
  - Contracts
- OpenZeppelin
  - Generic Runtime Template
  - EVM Template
- Parity
  - Polkadot SDK's Parachain Template

> Note: Some upstream template names and binaries still say "parachain", this is a Polkadot Chain.

## Adding a frontend

You can scaffold your chain with a frontend template using the `--with-frontend` flag. Pop CLI supports the following community frontend template for chains:

- [create-dot-app](https://github.com/polkadot-developers/create-dot-app) - Full-stack dApp boilerplate for Polkadot chains

### Interactive mode

The interactive prompt will ask you if you want to include a frontend template:

```bash
pop new chain
```

### CLI mode

You can specify the frontend template directly:

```bash
# With default frontend template selection
pop new chain my-chain --with-frontend
```

### Frontend dependencies

Pop CLI will automatically check for required dependencies and prompt you to install them if not present:

- Node.js (version 20 or later)

You can also install frontend dependencies separately using:

```bash
pop install -y --frontend
```

### Running the frontend

After scaffolding your chain with a frontend, you can start the frontend development server from your generated chain folder with:

```bash
pop up frontend
```

This command starts the frontend dev server for your chain project.

**Need help?**

Ask on [Polkadot Stack Exchange](https://polkadot.stackexchange.com/) (tag it [`pop`](https://substrate.stackexchange.com/tags/pop/info)) or drop by [our Telegram](https://t.me/onpopio). We're here to help!

