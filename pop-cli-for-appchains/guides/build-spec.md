---
description: Generate a plain chain specification and optional artifacts for your chain.
---

# Build a Chain Spec

The chain specification ("chain spec") captures the initial state and configuration of your chain. Nodes use it to start
a network or to join and sync with an existing one. With Pop CLI you can generate a plain chain spec interactively or
non‑interactively, and optionally produce extra artifacts such as genesis state and wasm code, and even inject a
deterministically built runtime.

## Interactive walkthrough

Run the command without arguments to be guided through all choices:

```bash
pop build spec
```

This will prompt you for key values like output file name, parachain id, relay chain, chain type, protocol id,
properties, whether to generate genesis files, and whether to build and inject a deterministic runtime.

<figure><img src="../.gitbook/assets/build_spec.gif" alt="pop build spec"><figcaption></figcaption></figure>

> Tip: You can press Tab to accept defaults and type to filter list selections.

### What you’ll typically provide

- Output file name/path for the plain spec (default: ./chain-spec.json)
- Parachain ID (default: 2000)
- Chain type (Development / Local / Live)
- Relay chain (paseo, westend, kusama, polkadot and their local variants)
- Protocol ID and optional properties (token symbol/decimals/SS58)
- Whether to also generate genesis state and genesis code files
- Optional: Build the runtime deterministically and inject it into the spec

## Non‑interactive usage

Prefer to skip prompts? Provide flags up front.

```bash
# Minimal example: write a plain chain spec
pop build spec -o ./chain-spec.json

# Provide basics explicitly
pop build spec -o ./chain-spec.json \
  --id 2000 \
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

Notes about flags:

- You can now pass --runtime without --deterministic to pre-select the runtime directory for the command. This alone
  will not trigger a deterministic build; you must add --deterministic to enable srtool.
- The --package flag is available to explicitly set the runtime package name when doing a deterministic build; if
  omitted, Pop CLI will infer it from the runtime directory.

{% hint style="info" %}
Omni-node-based chains: If your chain uses the community polkadot-omni-node host (ships only a runtime), you can still
use `pop build spec` the same way. Deterministic builds are recommended; Pop can also auto-source the
`polkadot-omni-node` binary when needed in related workflows.
{% endhint %}

## Command reference

```
$ pop build spec --help
Build a chain specification and its genesis artifacts

Usage: pop build spec [OPTIONS]

Options:
      --path <PATH>                          Directory path for your project [default: current directory] [default: ./]
  -o, --output <OUTPUT_FILE>                 File name for the resulting spec. If a path is given, the necessary directories will be created
      --profile <PROFILE>                    Build profile for the binary to generate the chain specification
  -i, --id <ID>                              Parachain ID to be used when generating the chain spec files
  -b, --default-bootnode <DEFAULT_BOOTNODE>  Whether to keep localhost as a bootnode [possible values: true, false]
  -t, --type <CHAIN_TYPE>                    Type of the chain [possible values: development, local, live]
      --features <FEATURES>                  Comma-separated list of features to build the node or runtime with [default: ]
      --skip-build                           Whether to skip the build step or not. If artifacts are not found, the build will be performed regardless
  -c, --chain <CHAIN>                        Provide the chain specification to use (e.g. dev, local, custom or a path to an existing file)
  -r, --relay <RELAY>                        Relay chain this parachain will connect to [possible values: paseo, paseo-local, westend, westend-local, kusama,
                                             kusama-local, polkadot, polkadot-local]
  -P, --protocol-id <PROTOCOL_ID>            Protocol-id to use in the specification
  -p, --properties <PROPERTIES>              The chain properties to use in the specification. For example, "tokenSymbol=UNIT,decimals=12"
  -S, --genesis-state <GENESIS_STATE>        Whether the genesis state file should be generated [possible values: true, false]
  -C, --genesis-code <GENESIS_CODE>          Whether the genesis code file should be generated [possible values: true, false]
  -d, --deterministic <DETERMINISTIC>        Whether to build the runtime deterministically. This requires a containerization solution (Docker/Podman) [possible
                                             values: true, false]
      --runtime <runtime>                    Define the directory path where the runtime is located
      --package <PACKAGE>                    Specify the runtime package name. If not specified, it will be automatically determined based on `runtime`
  -h, --help                                 Print help
```

## Outputs

Depending on the flags used you’ll get:

- A plain chain spec JSON.
- A raw chain spec JSON.
- Optionally, a genesis state file.
- Optionally, a genesis wasm code file.

## Next steps

- Open your chain-spec.json and edit fields like accounts, balances, sudo, and session keys as needed.
- Use the spec to launch locally with `pop up` or onboard to a relay chain (see Launch a Chain guides).

#### Learning Resources

- 🧑‍🏫 Background on Polkadot/Parachains: wiki.polkadot.network
- 🧑‍🔧 srtool: https://github.com/paritytech/srtool

**Need help?**

Ask on Polkadot Stack Exchange (tag it `pop`) or drop by our Telegram: https://t.me/onpopio. We're here to help!
