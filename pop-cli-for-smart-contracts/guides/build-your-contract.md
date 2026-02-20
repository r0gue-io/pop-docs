# Build

Before we compile your smart contract, make sure to take a look at what an ink! contract consists of in the [documentation](https://use.ink/docs/v6/getting-started/building-your-contract).&#x20;

To build your ink! smart contract, make sure you are inside your ink! smart contract directory and run the following command:

```shell
pop build
```

or

```
pop build --release
```

## Build options

Most common flags:

| Flag | Description |
| --- | --- |
| `PATH` / `--path <path>` | Project directory (defaults to the current directory). |
| `-r, --release` | Build in release mode. Conflicts with `--profile`. |
| `--profile <debug|release|production>` | Build profile (default: `debug`). |
| `--features <list>` | Comma-separated feature list. |
| `--metadata <spec>` | Choose the contract metadata spec (run `pop build --help` for supported values). |
| `--verifiable` | Build a verifiable contract (deterministic release build). Conflicts with `--release` and `--profile`. |
| `--image <image>` | Use a custom image for verifiable builds (requires `--verifiable`). |

### JSON output

Use global `--json` for structured output in scripts:

```shell
pop --json build --path ./my_contract --profile release
```

> [!NOTE]
> Verifiable builds require Docker to be running.

**Need help?**

Ask on [Polkadot Stack Exchange](https://polkadot.stackexchange.com/) (tag it [`pop`](https://substrate.stackexchange.com/tags/pop/info)) or drop by [our Telegram](https://t.me/onpopio). We're here to help!
