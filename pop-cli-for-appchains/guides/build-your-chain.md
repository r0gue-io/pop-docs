# Build your chain

Use `pop build` (alias: `pop b`) to build a chain node or runtime. If you need a chain spec or genesis artifacts, use `pop build spec`.

To build your chain using Pop CLI

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
pop build -p ./my-chain
```

You can also pass the project directory positionally:

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

If you are building the chain with the intent to onboard to a Polkadot Relay chain then you can run the following build command:

```
pop build -p ../my-chain --para_id 2000
```

This command will build your chain and generate the chain spec, the WebAssembly runtime, and generate the chain genesis state needed for registering and onboarding onto the Relay chain.

If you need to generate chain specs and genesis artifacts directly, use `pop build spec`:

```shell
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
