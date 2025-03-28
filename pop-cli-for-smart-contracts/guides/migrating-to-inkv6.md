---
description: This guide will show you how to start experimenting with ink! v6
---

# Migrating from ink! v5 (Wasm) to ink! v6 (PolkaVM / RISC-V)

Pop CLI version `v0.7.0` introduces experimental support for ink! v6 smart contracts running on PolkaVM (RISC-V) via `pallet-revive`. This guide helps you transition from ink! v5 (WebAssembly) and start experimenting with ink! v6.

### What's new in ink! v6?
- **ink! v5**: Compiled smart contracts to WebAssembly (Wasm) binaries executed by Substrate's `pallet-contracts`.
- **ink! v6**: Compiles smart contracts to RISC-V binaries executed by `pallet-revive` using PolkaVM.

### Installing Pop CLI for ink! v6 support

Pop CLI supports ink! v6 through the `polkavm-contracts` feature flag. Install it with:

```
cargo install --force --locked pop-cli --no-default-features -F polkavm-contracts,parachain
```

### Getting started with ink! v6

The workflow for developing smart contracts remains similar to ink! v5. To get started, see our guide:
[Create a new contract](./create-a-new-contract.md).


#### Migrating existing ink! smart contracts
If you already have an ink! smart contract, update your dependencies as follows:
Update your `Cargo.toml`

```
[dependencies]
ink = { version = "6.0.0-alpha", default-features = false }

[dev-dependencies]
ink_e2e = { version = "6.0.0-alpha" }
```

For a complete example, check the [ink! flipper example](https://github.com/use-ink/ink-examples/tree/v6.x/flipper).

#### Account Mapping
When interacting with `pallet_revive` for the first time, you must map your account. Mapping an account registers your account ID to interact with contracts.

Pop CLI will prompt you automatically:

<figure><img src=".gitbook/assets/map_account_prompt.png" alt="Map Account Pop CLI"><figcaption></figcaption></figure>

Alternatively, you can Polkadot JS UI:
<figure><img src=".gitbook/assets/map_account_polkadot_ui.png" alt="Map Account Polkadot UI"><figcaption></figcaption></figure>

## Resources

#### Learning Resources

* [Migrate from ink! v5 → v6](https://use.ink/6.x/faq/migrating-from-ink-5-to-6) in the ink! docs.
    * ⭕ Detailed [ink! v6 Changelog](https://github.com/use-ink/ink/blob/master/CHANGELOG.md#version-600).
* [pallet_revive](https://paritytech.github.io/polkadot-sdk/master/pallet_revive/index.html) technical documentation.


**Technical Support**

* [Polkadot Stack Exchange](https://polkadot.stackexchange.com/)
    * Create a question and tag it with "[`pop`](https://substrate.stackexchange.com/tags/pop/info)"
    * Share the StackExchange question in our [Pop Support Telegram channel](https://t.me/pop\_support)