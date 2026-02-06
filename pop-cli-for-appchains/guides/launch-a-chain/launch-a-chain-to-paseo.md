---
description: Deploy a chain to Paseo (local or live).
---

# Deploy a chain to Paseo

Use Paseo Local to test onboarding locally, then deploy to the live Paseo testnet.

## Prerequisites

- A chain project built with `pop build`.
- A chain spec and genesis artifacts.
- Collator binary built for your chain.
- Operational keys (stash, session, and chain manager).

## Choose Paseo Local or Live

- **Paseo Local**: [Launch Paseo locally](launch-paseo.md).
- **Paseo Live**: Use a public endpoint, for example the Polkadot.js Apps explorer: https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Frpc.ibp.network%2Fpaseo#/explorer

## Generate operational keys

Generate the stash, session, and chain manager keys: [Set up keys](keys.md).

## Fund the chain manager

### Paseo Local

```bash
pop call chain --pallet Balances --function transfer_allow_death \
  --url ws://localhost:57731/ \
  --suri //Alice \
  --args <CHAIN_MANAGER_SS58> 1000000000000000
```

### Paseo Live

Request PAS tokens from the Paseo faucet: https://faucet.polkadot.io/

## Prepare chain spec and genesis artifacts

Generate and edit your chain spec so it includes:

- Your collator stash and session keys.
- Initial balances for required accounts.
- The sudo account for the chain.

Use `pop build spec` to generate the chain spec and artifacts. For details, see [Build your chain spec](../build-spec.md).

If you edit `chain-spec.json`, regenerate the raw spec and genesis artifacts:

```bash
pop build spec --chain chain-spec.json --disable-default-bootnode --genesis-state --genesis-code
```

## Get the relay chain spec

### Paseo Local

When you launch Paseo Local with `pop up network`, Pop CLI prints the relay chain spec path. Copy it into your chain directory as `paseo-local-raw.json`.

### Paseo Live

Download the Paseo chain spec and save it as `paseo-raw.json`:

- https://github.com/paseo-network/runtimes/blob/main/chain-specs/paseo-local.raw.json

## Generate the node key

### Using Docker

```bash
cd my-chain
mkdir -p data/chains/my_chain/network
docker run -it parity/subkey:latest generate-node-key > ./data/chains/my_chain/network/secret_ed25519
```

### On your machine

```bash
path/to/polkadot-sdk/target/debug/substrate-node key generate-node-key \
  --file=secret_ed25519 \
  --chain=./chain-spec-raw.json

mv secret_ed25519 data/chains/my_chain/network
```

## Run the collator

```bash
./target/release/parachain-template-node \
  --collator \
  --force-authoring \
  --chain chain-spec-raw.json \
  --base-path ./data \
  --port 40333 \
  --rpc-port 8845 \
  -- \
  --sync warp \
  --chain paseo-local-raw.json \
  --port 57733 \
  --rpc-port 57731
```

The second half of this command configures the relay chain node to connect to.

Insert the session key so the collator can author blocks:

```bash
curl -H "Content-Type: application/json" \
  --data '{
    "jsonrpc":"2.0",
    "method":"author_insertKey",
    "params":[
      "aura",
      "INSERT_SECRET_SESSION_SEED_PHRASE",
      "INSERT_PUBLIC_SESSION_KEY_HEX_FORMAT"
    ],
    "id":1
  }' \
  http://localhost:8845
```

## Register the chain

Use `pop up` to reserve a para ID and register your chain:

```bash
pop up --genesis-state ./para-2000-genesis-state --genesis-code ./para-2000.wasm
```

If you prefer manual control, you can reserve and register with `pop call chain`.

## Acquire coretime

Your chain needs coretime to have its blocks validated. See [Acquire Coretime](coretime.md).

> Using a private key URI is fine for development accounts only. For production or higher security, use `--use-wallet` and follow [Securely sign transactions from CLI](../securely-sign-transactions-from-cli.md).

## Resources

#### Learning Resources

* [https://paritytech.github.io/devops-guide/guides/parachain_deployment.html](https://paritytech.github.io/devops-guide/guides/parachain_deployment.html)
* 🧑‍🏫 To learn about Polkadot in general, [Polkadot.network](https://polkadot.network/) website is a good starting point.
  * ⭕ Learn more about Polkadot chains [here](https://wiki.polkadot.network/docs/learn-parachains).
* 🧑‍🔧 For technical introduction, [here](https://github.com/paritytech/polkadot-sdk#-documentation) are the Polkadot SDK documentation resources.

**Need help?**

Ask on [Polkadot Stack Exchange](https://polkadot.stackexchange.com/) (tag it [`pop`](https://substrate.stackexchange.com/tags/pop/info)) or drop by [our Telegram](https://t.me/onpopio). We're here to help!
