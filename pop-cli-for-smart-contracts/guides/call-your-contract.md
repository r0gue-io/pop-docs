# Call Your Contract

The `pop call contract` command enables interaction with deployed ink! smart contracts.

### What Can You Do?

The `pop call contract` command supports three types of operations:

#### 1. Execute Messages

Submit transactions that modify contract state. These operations require signing, consume gas, and wait for on-chain finalization.

#### 2. Query Messages

Read contract state without making changes. These read-only calls require no signing, incur no gas costs, and return values instantly.

#### 3. Read Storage

Access contract storage fields directly using the storage layout. No signing required and no gas costs.

### Interactive Guidance (Recommended)

Interact with your contract **using** Pop CLI's interactive guidance by simply entering:

```shell
pop call contract
```

First, you will be prompted to select or enter your contract project path. You can type to filter the list and quickly find your desired contract. After selecting your contract, you will be prompted to choose a chain from the list or enter a custom RPC endpoint. If you want to connect to a custom RPC endpoint, select the **"Custom"** option, which allows you to manually type the chain URL.

After selecting your chain, you will be prompted to specify the deployed contract address, then select a message or storage item to call:

- **Execute a message** (function that modifies state, e.g. flip, transfer)
- **Query a message** (read-only function that returns state)
- **Read storage** (direct storage access)

After making your selection, you'll be guided through providing any required arguments and (for messages that modify state) the account to sign the transaction.

### Manual (non-interactive)

If you prefer not to use interactive prompts, you can call your contract by specifying all the required arguments directly:

#### Executing a Message

You can execute a message by specifying the contract path (or the metadata file), contract address, message name, and any arguments.

```shell
pop call contract --path ./flipper --contract 0x48550a4bb374727186c55365b7c9c0a1a31bdafe --message flip --suri //Alice --execute --url ws://localhost:9944/
```

```shell
pop call contract --path ./my_token --contract 0x48550a4bb374727186c55365b7c9c0a1a31bdafe --message transfer --args "0x1234..." "1000" --execute --url ws://localhost:9944/
```

#### Querying Messages

You can query messages by specifying the contract path, contract address, and message name.
Query messages return the current value immediately without requiring transaction signing.

```shell
pop call contract --path ./flipper --contract 0x48550a4bb374727186c55365b7c9c0a1a31bdafe --message get --url ws://localhost:9944/
```

#### Reading Storage

Access contract storage fields directly using the storage layout.
Storage queries return the current value immediately without requiring transaction signing.

```shell
pop call contract --path ./flipper --contract 0x48550a4bb374727186c55365b7c9c0a1a31bdafe --message value --url ws://localhost:9944/
```

### Additional Options

#### Gas Estimation

Use `--dry-run` to estimate gas consumption before execution without submitting the transaction:

```shell
pop call contract --path ./flipper --contract 0x48550a4bb374727186c55365b7c9c0a1a31bdafe --message flip --dry-run --url ws://localhost:9944/
```

#### Using Wallet for Signing

You can use a browser extension wallet to sign transactions instead of providing a secret URI:

```shell
pop call contract --path ./flipper --contract 0x48550a4bb374727186c55365b7c9c0a1a31bdafe --message flip --use-wallet --url ws://localhost:9944/
```

Or use the shorthand `-w`:

```shell
pop call contract --path ./flipper --contract 0x48550a4bb374727186c55365b7c9c0a1a31bdafe --message flip -w --url ws://localhost:9944/
```

#### Developer Mode

Use `--dev` for rapid testing during development. This skips gas prompts and confirmation dialogs:

```shell
pop call contract --dev --contract 0x48550a4bb374727186c55365b7c9c0a1a31bdafe --message flip --execute
```

### Note on Addresses

If you're using ink! v6, contract addresses are displayed as 20-byte H160 hashes (for example, `0x48550a4bb374727186c55365b7c9c0a1a31bdafe`).


**Need help?**

Ask on [Polkadot Stack Exchange](https://polkadot.stackexchange.com/) (tag it [`pop`](https://substrate.stackexchange.com/tags/pop/info)) or drop by [our Telegram](https://t.me/onpopio). We're here to help!
