---
description: The following guide shows how to test runtime upgrades.
---

# Test runtime upgrades

A key feature of Substrate is its support for forkless upgrades. Testing the blockchain runtime upgrade process is essential to ensure a seamless network transition without disruptions.

To simulate and validate the process of upgrading a blockchain's runtime, the `on-runtime-upgrade` executes the [`OnRuntimeUpgrade`](https://paritytech.github.io/polkadot-sdk/master/frame_support/traits/trait.OnRuntimeUpgrade.html) hooks of a runtime against the state of a live blockchain or a snapshot.

Hence, there are two subcommands `live` and `snap` to specify the source of the runtime state.

```bash
Usage: pop test on-runtime-upgrade [OPTIONS] [COMMAND]

Commands:
  live  A live chain
  snap  A state snapshot
  help  Print this message or the help of the given subcommand(s)
```

### JSON mode

Use global `--json` for structured output:

```bash
pop --json test on-runtime-upgrade --runtime ./target/release/my_runtime.wasm --checks all live --uri wss://rpc1.paseo.popnetwork.xyz
```

In JSON mode, interactive prompts are disabled. Provide all required runtime/source/check flags explicitly.

## Test migrations

By running the command `pop test on-runtime-upgrade`, you can test the [Runtime Upgrades](https://docs.polkadot.com/develop/parachains/maintenance/runtime-upgrades/) and [Storage Migrations](https://docs.polkadot.com/develop/parachains/maintenance/storage-migrations/) in a simulated environment.

```bash
┌   Pop CLI : Testing migrations
│
◆  Do you want to specify which runtime to run the migration on?
│  If not provided, use the code of the remote node, or a snapshot.
│  ● Yes  / ○ No
└
```

Before running the migration, you will be prompted to confirm if you want to specify which runtime to run the migration on:

* If you choose to specify, you will be prompted to select the runtime to run the migration on. The feature requires your runtime to be [built with `--try-runtime` feature](/broken/pages/iluN68guT8MppWAl8pBK).

> Pop CLI will automatically locate the runtime binary based on the provided `--profile`. Pop CLI will automatically build the runtime if not found.

* If not, the migration will be run against the code that's currently running on the remote node or the one stored inside the snapshot file you provide.

> Snapshot can be created with `pop test create-snapshot`.

### Test migrations against a live chain

```bash
◆  Select source of runtime state:
│  ● Live (Run the migrations on top of live state.)
│  ○ Snapshot
|
◇  Enter the live chain of your node:
│  wss://rpc1.paseo.popnetwork.xyz
│
◆  Enter the block hash (optional):
│  0x1234567890abcdef1234567890abcdef
└
```

You'll be asked to enter the URI of a live node and optionally provide a block hash. If a [block hash](https://paritytech.github.io/polkadot-sdk/master/sp_runtime/traits/trait.HashOutput.html) is given, the state will be executed at the specified block hash on the provided network.

To run the migrations on top of live state manually:

```bash
pop test on-runtime-upgrade \
    --runtime=<path_to_runtime_binary> \
    live \
    --uri=wss://rpc1.paseo.popnetwork.xyz \
    --at=0x1234567890abcdef1234567890abcdef
```

_**Note**_: The specified runtime and the remote node's runtime must have the same name and version. If not, the migration will fail. You can add the flag `--disable-spec-version-check` and `--disable-spec-name-check` to bypass the checks. Pop may also prompt you to disable these checks if it detects a mismatch.

```bash
pop test on-runtime-upgrade \
    --runtime=<path_to_runtime_binary> \
    --disable-spec-version-check \
    --disable-spec-name-check \
    live \
    --uri=wss://rpc1.paseo.popnetwork.xyz \
    --at=0x1234567890abcdef1234567890abcdef
```

After that, you can select the upgrade checks to perform:

```bash
◆  Select upgrade checks to perform:
│  ○ none
│  ● all (Run the `try_state`, `pre_upgrade` and `post_upgrade` checks)
│  ○ try-state
│  ○ pre-and-post
└
```

### Test migrations with a snapshot file

A second approach to test migrations is with a snapshot file. First, you need to create a snapshot file of a live network. You can do this by running the following command:

```bash
pop test create-snapshot
```

The interactive interface to prompts for the live URI and the path of the snapshot file:

```bash
┌   Pop CLI : Creating a snapshot file
│
▲  NOTE: `create-snapshot` only works with the remote node. No runtime required.
│
◇  Enter the URI of the remote node:
│  wss://rpc1.paseo.popnetwork.xyz
│
◇  Enter the path to write the snapshot to (optional):
│  If not provided `<spec-name>-<spec-version>@<block-hash>.snap` will be used.
│  example.snap
```

To skip the interactive prompt, use the `--uri` and `--path` flags:

```bash
pop test create-snapshot --uri wss://rpc1.paseo.popnetwork.xyz ./example.snap
```

If the path of snapshot file is not provided, the default name following a format `<spec-name>-<spec-version>@<block-hash>.snap` will be used. Note that the remote node must be built with `--try-runtime` feature enabled.

Assume there is a snapshot file created with the name `example.snap`:

```bash
◇  Enter path to your snapshot file?
│  Snapshot file can be generated using `pop test create-snapshot` command.
│  ./example.snap
```

To run migrations with a snapshot manually:

```bash
pop test on-runtime-upgrade \
    --runtime=<path_to_runtime_binary> \
    --blocktime=6000 \
    --checks=all \
    snap \
    --path=./example.snap
```

#### Learning Resources

* 🧑‍🏫 To learn about Polkadot in general, [Polkadot.network](https://polkadot.network/) website is a good starting point.
* 🧑‍🔧 For technical introduction of the `try-runtime`, [here](https://paritytech.github.io/try-runtime-cli/try_runtime/).
* Learn more about [Runtime Upgrades](https://docs.polkadot.com/develop/parachains/maintenance/runtime-upgrades/) and [Storage Migrations](https://docs.polkadot.com/develop/parachains/maintenance/storage-migrations).

**Need help?**

Ask on [Polkadot Stack Exchange](https://polkadot.stackexchange.com/) (tag it [`pop`](https://substrate.stackexchange.com/tags/pop/info)) or drop by [our Telegram](https://t.me/onpopio). We're here to help!
