---
description: Generate a plain chain specification and optional artifacts for your chain.
---

# Build your chain spec

Use this when you need a chain specification or genesis artifacts (`pop build spec`); use `pop build` for the chain binary/runtime.

The chain specification ("chain spec") captures the initial state and configuration of your chain. Nodes use it to start
a network or to join and sync with an existing one. With Pop CLI you can generate a plain chain spec interactively or
non‑interactively, and optionally produce extra artifacts such as genesis state and wasm code, and even inject a
deterministically built runtime.

## Interactive walkthrough

Run the command without arguments to be guided through all choices:

```bash
pop build spec
```

This will prompt you for key values like output file name, chain ID, relay chain, chain type, build profile, protocol
ID, properties, whether to generate genesis files, and whether to build and inject a deterministic runtime.


> Tip: You can press Tab to accept defaults and type to filter list selections.

### What you’ll typically provide

- Output file name/path for the plain spec (default: `./chain-spec.json`)
- Chain ID (default: `2000`) or relay mode
- Relay chain (paseo, westend, kusama, polkadot and local variants)
- Chain type (`Development`, `Local`, `Live`)
- Build profile (`debug`, `release`, `production`)
- Protocol ID and optional properties (token symbol/decimals/SS58)
- Whether to keep localhost as a bootnode (non-development chains)
- Whether to generate genesis state and genesis code files
- Optional: Build the runtime deterministically and inject it into the spec
- Runtime directory and package name (only when deterministic builds are enabled)
- Chain spec input or runtime preset (if you did not pass `--chain`)

## Non‑interactive usage

Prefer to skip prompts? Provide flags up front.

```bash
# Minimal example: write a plain chain spec
pop build spec -o ./chain-spec.json

# Provide basics explicitly
pop build spec -o ./chain-spec.json \
  --para-id 2000 \
  --type Local \
  --relay paseo \
  --protocol-id my-protocol \
  --properties "tokenSymbol=UNIT,decimals=12"

# Include build profile, features, and skip building binaries (if already built)
pop build spec -o ./chain-spec.json \
  --profile release \
  --features foo,bar \
  --skip-build

# Also generate genesis state and wasm code files alongside the spec
pop build spec -o ./chain-spec.json \
  --genesis-state \
  --genesis-code
```

### Deterministic runtime build and injection (optional)

Pop CLI can build your runtime deterministically using srtool and inject the resulting wasm code into the chain spec
generation flow.

```bash
# Minimal deterministic build example
pop build spec --deterministic --runtime ./runtime/mainnet

# Optionally specify the runtime package name used by srtool
# note: --package is only applicable when --deterministic is set
pop build spec --deterministic --runtime ./runtime/mainnet --package parachain-template-runtime
```

## Flags

Most common flags:

| Flag | Description |
| --- | --- |
| `-p, --path <path>` | Project directory (default: `./`). |
| `-o, --output <path>` | Output path for the plain spec. If you pass a directory or a path without `.json`, Pop CLI writes `chain-spec.json` in that directory. |
| `--profile <debug|release|production>` | Build profile for the node/runtime used to generate specs. |
| `-i, --para-id <id>` | Parachain ID (ignored when `--is-relay` is set). |
| `-b, --default-bootnode <true|false>` | Keep localhost as a bootnode. |
| `-t, --type <Development|Local|Live>` | Chain type. |
| `--features <list>` | Comma-separated features for the node/runtime build. |
| `--skip-build` | Skip the build step if artifacts exist. |
| `-c, --chain <value>` | Chain spec input (dev, local, custom, or a path to an existing spec). |
| `-R, --is-relay` | Generate a relay chain spec (conflicts with `--para-id` and `--relay`). |
| `-r, --relay <paseo|westend|kusama|polkadot|...>` | Relay chain for chain specs. |
| `-n, --name <name>` | Chain name to embed in the spec. |
| `--id <id>` | Chain ID to embed in the spec. |
| `-P, --protocol-id <id>` | Protocol ID to embed in the spec. |
| `--properties <kv>` | Chain properties string, for example `tokenSymbol=UNIT,decimals=12`. |
| `-S, --genesis-state <true|false>` | Generate genesis state. |
| `-C, --genesis-code <true|false>` | Generate genesis code (Wasm). |
| `-d, --deterministic <true|false>` | Build the runtime deterministically (requires Docker/Podman). |
| `--tag <image>` | srtool image tag for deterministic builds (requires `--deterministic`). |
| `--runtime <path>` | Runtime directory to use for builds and presets. |
| `--package <name>` | Runtime package name (requires `--deterministic`). |
| `--raw` | Generate a raw chain spec. |

## Behavior notes

- If `--chain` points to an existing spec file, Pop CLI uses that file as the output path and asks if you want to apply changes.
- `--skip-build` is ignored when node/runtime artifacts are missing. Pop CLI warns and builds anyway.
- `--deterministic` only runs when you enable it explicitly or choose it in the prompt. For `production` builds, the prompt defaults to deterministic. It asks for the runtime directory and package when needed.
- `--raw` is generated automatically when you request genesis state or genesis code.
- When you pass `--runtime`, Pop CLI treats the runtime directory as the build target and prompts you to pick a runtime preset if you did not pass `--chain`.

> [!TIP]
> Omni-node-based chains: If your chain uses the community `polkadot-omni-node` host (ships only a runtime), you can still
> use `pop build spec` the same way. Deterministic builds are recommended; Pop can also auto-source the
> `polkadot-omni-node` binary when needed in related workflows.

## Outputs

Depending on the flags used you’ll get:

- A plain chain spec JSON.
- Optionally, a raw chain spec JSON.
- Optionally, a genesis state file.
- Optionally, a genesis wasm code file.


#### Learning Resources

- 🧑‍🏫 Background on Polkadot chains: wiki.polkadot.network
- 🧑‍🔧 srtool: https://github.com/paritytech/srtool

**Need help?**

Ask on Polkadot Stack Exchange (tag it `pop`) or drop by our Telegram: https://t.me/onpopio. We're here to help!
