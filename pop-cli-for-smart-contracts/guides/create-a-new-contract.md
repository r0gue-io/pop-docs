# Create a new contract

To view all available smart contract templates run:

```bash
pop new contract
```

To start a new contract, e.g. ERC20, run:

```bash
pop new contract my_erc20 --template erc20
```

Then open the generated `my_erc20` directory to start developing your contract!

## Adding a frontend

You can scaffold your contract with a frontend template using the `--with-frontend` flag. Pop CLI supports the following community frontend templates for contracts:

- [inkathon](https://github.com/scio-labs/inkathon) - Full-stack dApp boilerplate for ink! smart contracts
- [typink](https://github.com/dedotdev/typink) - Type-safe frontend framework for ink! contracts

### Interactive mode

The interactive prompt will ask you to select a frontend template:

```bash
pop new contract my_contract
```

### CLI mode

You can specify the frontend template directly:

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

### Running the frontend

After scaffolding your contract with a frontend, you can start the frontend development server from your generated chain folder with:

```bash
pop up frontend
```

This command starts the frontend dev server for your contract project.

**Need help?**

Ask on [Polkadot Stack Exchange](https://polkadot.stackexchange.com/) (tag it [`pop`](https://substrate.stackexchange.com/tags/pop/info)) or drop by [our Telegram](https://t.me/onpopio). We're here to help!
