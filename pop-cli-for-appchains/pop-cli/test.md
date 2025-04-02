---
description: Test a runtime
---

# test

The Pop CLI feature is built on top of the [`try-runtime-cli`](https://github.com/paritytech/try-runtime-cli?tab=readme-ov-file), a programmatic testing framework designed for Substrate. It allows you to test runtime upgrades and state transitions.

- `on-runtime-upgrade`: Tests migrations.
- `execute-block`: Executes a specified block against some state.
- `fast-forward`: Executes a runtime upgrade (optional), then mines a number of blocks while performing try-state checks.
- `create-snapshot`: Creates a chain state snapshot.

## test on-runtime-upgrade

To test [runtime upgrades](https://docs.polkadot.com/develop/parachains/maintenance/runtime-upgrades/), you can use the `on-runtime-upgrade` subcommand. This command allows you to test migrations by executing the [`OnRuntimeUpgrade`](https://paritytech.github.io/polkadot-sdk/master/frame_support/traits/trait.OnRuntimeUpgrade.html) hooks of a runtime against the state of a live blockchain or a snapshot.

```bash
pop test on-runtime-upgrade live
```

**Note:**

If you choose to run migrations on a local runtime binary, you will be prompted to provide a valid runtime binary path. [Ensure the binary is built with the `try-runtime` feature](./build.md).

> Pop CLI will automatically locate the runtime binary based on the provided `--profile`. If the binary is not found, Pop CLI will build it automatically.

If a runtime is not specified, the migration will be executed on the code currently running on the remote node of a live blockchain or the one stored in the provided snapshot file.

### test on-runtime-upgrade live

To run migrations against a live blockchain, use the `on-runtime-upgrade live` subcommand:

```bash
pop test on-runtime-upgrade live
```

You'll be prompted to provide an RPC URI of a live blockchain. A valid RPC URI must start with `ws://` or `wss://`. You can also specify an optional block hash to define the state execution point.

To manually provide the URI and block hash, use:

```bash
pop test on-runtime-upgrade \
    --runtime=<path_to_runtime_binary> \
    live \
    --uri=wss://rpc1.paseo.popnetwork.xyz \
    --at=0x1234567890abcdef1234567890abcdef1234567890abcdef1234567890abcdef
```

### test on-runtime-upgrade snap

To run migrations against a snapshot, use the `on-runtime-upgrade snap` subcommand:

```bash
pop test on-runtime-upgrade \
    --runtime=<path_to_runtime_binary> \
    snap \
    --path=./example.snap
```

## test create-snapshot

This command saves a copy of the blockchain's current state to a file (a snapshot). You can use this snapshot to run tests quickly instead of downloading the state from a live blockchain each time.

```bash
pop test create-snapshot
```

You'll be prompted to enter the RPC URI (the network address of the live blockchain) and the file path where the snapshot should be stored.

To provide these parameters manually:

```bash
pop test create-snapshot \
    --uri=wss://rpc1.paseo.popnetwork.xyz \
    --path=./example.snap
```

**Note:** A live blockchain node must be [built with the `--try-runtime` feature](./build.md).

## test execute-block

This command executes a given number of blocks. Currently, `execute-block` only supports executing blocks against a live state. You can also specify a block hash to execute from.

```bash
pop test execute-block
```

You'll be prompted to select state tests to execute, with the following options:

- **None**: No state tests will be executed.
- **All**: Executes all available state tests.
- **Round Robin**: Runs a fixed number of state tests in a round-robin manner.
- **Only Pallets**: Executes only state tests related to specific pallets.

Available pallets on the live network will be listed for selection:

```bash
◆  Select pallets (select with SPACE):
│
│  ◻ Assets
│  ◻ Aura
│  ◻ AuraExt
│  ◻ Authorship
│  ◻ Balances
│  ◻ CollatorSelection
│  ◻ Contracts
│  ◻ Council
└
```

To skip the interactive interface and provide arguments manually:

```bash
pop test execute-block \
    --runtime=<path_to_runtime_binary> \
    --try-state=Aura,Assets,Authorship \
    -y \
    --uri=wss://rpc1.paseo.popnetwork.xyz
```

For round-robin state tests, use:

```bash
pop test execute-block \
    --runtime=<path_to_runtime_binary> \
    --try-state=rr-10 \
    -y \
    --uri=wss://rpc1.paseo.popnetwork.xyz
```

## test fast-forward

Executes a runtime upgrade (optional), then mines a number of blocks while performing try-state checks.

```bash
pop test fast-forward
```

You'll be prompted to specify the number of blocks to mine and whether to run pending migrations before fast-forwarding.

```bash
◇  How many empty blocks should be processed?
│  10
│
◆  Do you want to run pending migrations before fast-forwarding?
│  ● Yes  / ○ No
└
```

Like `on-runtime-upgrade`, `fast-forward` supports `live` and `snap` subcommands to define the source of the runtime state.

To skip the interactive interface and specify arguments manually:

```bash
pop test fast-forward \
    --n-blocks 10 \
    --run-migrations \
    live \
    --uri=wss://rpc1.paseo.popnetwork.xyz
```

**Note:**

If you run migrations on a local runtime binary, you will be prompted to provide a valid runtime binary path. [Ensure the binary is built with the `try-runtime` feature](./build.md).

> Pop CLI will automatically locate the runtime binary based on the provided `--profile`. If the binary is not found, it will be built automatically.

### Learning Resources

- Learn more about [Runtime Upgrades](https://docs.polkadot.com/develop/parachains/maintenance/runtime-upgrades/) and [Storage Migrations](https://docs.polkadot.com/develop/parachains/maintenance/storage-migrations/).
