# Build your chain

Use this when you want to build a chain binary or runtime (`pop build`); use `pop build spec` for chain specs and genesis artifacts.

Use `pop build` (alias: `pop b`) to build a parachain node or runtime. The command auto-detects the project type in this order:

- parachain runtime
- parachain (node/runtime workspace)
- generic Rust package

To build your chain using Pop CLI:

```shell
cd my-chain
pop build
```

```
┌   Pop CLI : Building a chain
│
   Compiling parachain-template-runtime v0.1.0 (/Users/pop/src/my-chain/runtime)
   Compiling parachain-template-node v0.1.0 (/Users/pop/src/my-chain/node)
    Finished release [optimized] target(s) in 1m 20s

└  Build Completed Successfully!
```

If you are outside the project's directory, you can specify the path

```shell
pop build --path ./my-chain
```

You can also pass the project directory positionally (the positional path wins if you also set `--path`).

```shell
pop build ../my-chain
```

## Build options

Most common flags:

| Flag | Description |
| --- | --- |
| `PATH` / `--path <path>` | Project directory (defaults to the current directory). |
| `-p, --package <name>` | Build a specific workspace package. |
| `-r, --release` | Build in release mode. Conflicts with `--profile`. |
| `--profile <debug|release|production>` | Build profile (default: `debug`). |
| `--features <list>` | Comma-separated feature list. |
| `--benchmark` | Adds the `runtime-benchmarks` feature. |
| `--try-runtime` | Adds the `try-runtime` feature. |
| `--only-runtime` | Build only the runtime. |
| `--deterministic` | Build the runtime deterministically (requires Docker/Podman). Implies `--only-runtime`. |
| `--tag <image>` | Use a specific srtool image tag (requires `--deterministic`). |

> [!NOTE]
> If your workspace has multiple runtime crates, Pop CLI prompts you to choose one.

## Examples

Build the parachain node with extra runtime features:

```
pop build --release --benchmark --try-runtime
```

Build only the runtime deterministically:

```
pop build --deterministic --tag v1.0.0
```

If you need a chain spec and genesis artifacts for onboarding, use `pop build spec` instead:

```
pop build spec --para-id 2000 --relay paseo --genesis-state --genesis-code
```

#### Learning Resources

* 🧑‍🏫 To learn about Polkadot in general, [Polkadot.network](https://polkadot.network/) website is a good starting point.
  * ⭕ Learn more about Polkadot chains [here](https://wiki.polkadot.network/docs/learn-parachains).
* 🧑‍🔧 For technical introduction, [here](https://github.com/paritytech/polkadot-sdk#-documentation) are the Polkadot SDK documentation resources.

**Technical Support**

* [Polkadot Stack Exchange](https://polkadot.stackexchange.com/)
  * Create a question and tag it with "[`pop`](https://substrate.stackexchange.com/tags/pop/info)"
  * Share the StackExchange question in our [Pop Support Telegram channel](https://t.me/pop_support)
