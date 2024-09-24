---
description: This guide is for running a parachain on the Polkadot "Paseo" TestNet Locally
hidden: true
---

# Running on Paseo Locally

## Introduction

[Paseo](https://x.com/PaseoNetwork) is the community-run Polkadot Relay chain TestNet.

> Note: For contrast, Rococo is the [Parity](https://parity.io)-run Polkadot Relay chain TestNet which [will be deprecating](https://forum.polkadot.network/t/a-new-test-network-for-polkadot/4325) in favour of the [Paseo TestNet](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Fpaseo-rpc.dwellir.com#/explorer).

A good development workflow:&#x20;

1. Run your parachain **locally** on Paseo TestNet for development purposes&#x20;
2. When ready to test in a live environment with other parachains:
   * [ ] Use this guide to onboard your parachain to a **local** [Paseo TestNet](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Fpaseo-rpc.dwellir.com#/explorer).
   * [ ] [Onboard your parachain to Paseo Live TestNet](running-on-paseo-1.md)

Let's get started.

## Spin up Paseo locally

```bash
touch paseo-local.toml
```

network.toml

```toml
[relaychain]
chain = "paseo-local"

[[relaychain.nodes]]
name = "alice"
validator = true

[[relaychain.nodes]]
name = "bob"
validator = true

[[relaychain.nodes]]
name = "charlie"
validator = true
```

Run the network:

```
pop up parachain -f network.toml --verbose
```

> The `--verbose` flag will allow us to see extra information such as the location of the chain spec for the local Paseo network that we are running.

Paseo should now be running on your machine and producing blocks. We can now move towards setting up our parachain.

## Setting up our parachain

For the sake of this exercise, let's create a new parachain:

```
pop new parachain
```

```
┌   Pop CLI : Generate a parachain
│
◇  Select a template provider: 
│  Pop 
│
◇  Select the type of parachain:
│  Standard 
│
⚙  Template License: Unlicense
│  
◇  Select a specific release:
│  Polkadot v1.14 
│
◇  Where should your project be created?
│  ./my-parachain
│
◇  What is the symbol of your parachain token?
│  UNIT
│
◇  How many token decimals?
│  12
│
◆  And the initial endowment for dev accounts?
│  1u64 << 60
└  
```

```
cd my-parachain
pop build
```

It will take some time for the parachain to build. In the meantime, we can start setting up our accounts.

## Setting Up Accounts

First, we will need to set up a stash account to do transactions on Paseo on behalf of our collator.

> A collator is the parachain node that will be running your parachain.

There are multiple ways to generate keys (accounts) on Polkadot, such as:

* [PolkadotJs Signer](https://polkadot.js.org/) or any other custodial&#x20;
* [PolkaVault](https://wiki.polkadot.network/docs/polkadot-vault)&#x20;
* [Subkey](https://paritytech.github.io/polkadot-sdk/master/subkey/index.html)

> Account creation should be done securely, such as using an air-gapped computer.

### Create a Stash Account Key

For the sake of this guide, we will use subkey:

```bash
docker pull parity/subkey:latest
```

> Once downloaded, `subkey` does not need an internet connection to work.&#x20;

Let's create the stash account key:

```bash
docker run -it parity/subkey:latest generate --scheme sr25519
```

> If you do not have docker installed, you can download the [polkadot-sdk](https://github.com/paritytech/polkadot-sdk) and run the following command instead:

```bash
git clone --depth 1 https://github.com/paritytech/polkadot-sdk
cd polkadot-sdk
pop build
./target/debug/polkadot key generate --scheme sr25519
```

You should get an output similar to:

```bash
Secret phrase:       innocent throw harsh wild example reflect sausage leopard lake bottom police enact
  Network ID:        substrate
  Secret seed:       0xee07e6d00ebed8816d3f3839caca779dbddb52e9847159feaf1858dec6adcc6e
  Public key (hex):  0xe0f20cba0c53da3ab427a5bd5b49b3214038a7b89fe4b1b7ea992d153fab495a
  Account ID:        0xe0f20cba0c53da3ab427a5bd5b49b3214038a7b89fe4b1b7ea992d153fab495a
  Public key (SS58): 5H9eVCvHfNMqNhMTL2FVJfy7fPgquCpvCktkMd98Dz9goRBy
  SS58 Address:      5H9eVCvHfNMqNhMTL2FVJfy7fPgquCpvCktkMd98Dz9goRBy
```

> This your key (Polkadot account). Save the secret phrase in a vault securely and never share it.

Now that we have a stash account, we need to fund this account with some tokens so that it has funds to perform transactions on behalf of the collator such as transactions related to onboarding your parachain.

Go to the [Polkadot Faucet](https://faucet.polkadot.io/) and fund your account:

<figure><img src="../.gitbook/assets/Screenshot 2024-09-06 at 4.04.37 PM.png" alt="" width="563"><figcaption><p>Polkadot / Paseo Faucet</p></figcaption></figure>

Cool. Make sure to add your account to the [Polkadot Signer extension](https://polkadot.js.org/) so that you can see your account appear in the [PolkadotJs Apps](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Fpaseo-rpc.dwellir.com#/accounts) UI.

> Alternatively, you can check the Paseo chain state under [system.account()](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Fpaseo-rpc.dwellir.com#/chainstate) to see your account balance.

### Create a Session Account Key

We now need to create one more account. In order for collators to produce blocks, they need to sign the block with a (session) key. We call this key the session key. This account is specifically created for block production.

Let's go ahead and create the session account key.

We can use `subkey` to generate the keys for us.

```
docker pull parity/subkey:latest
```

> Reminder that this should be done securely, such as using an air-gapped computer.

```
docker run -it parity/subkey:latest generate --scheme sr25519
```

```
Secret phrase:       innocent throw harsh wild example reflect sausage leopard lake bottom police enact
  Network ID:        substrate
  Secret seed:       0xee07e6d00ebed8816d3f3839caca779dbddb52e9847159feaf1858dec6adcc6e
  Public key (hex):  0xe0f20cba0c53da3ab427a5bd5b49b3214038a7b89fe4b1b7ea992d153fab495a
  Account ID:        0xe0f20cba0c53da3ab427a5bd5b49b3214038a7b89fe4b1b7ea992d153fab495a
  Public key (SS58): 5H9eVCvHfNMqNhMTL2FVJfy7fPgquCpvCktkMd98Dz9goRBy
  SS58 Address:      5H9eVCvHfNMqNhMTL2FVJfy7fPgquCpvCktkMd98Dz9goRBy
```

> Save the secret phrase in a secure manner.

## Creating the chain spec for the parachain

Let's create a chain spec for our parachain:

```
cd my-parachain
```

Build your parachain:

```
pop build
```

Create the chain spec:

```
pop build spec --release --id 2000 --type local --relay paseo-local --protocol-id my_parachain
```

This will output the following:

* Your parachain's chain specification file
  * The plain text and the raw version e.g. `chain-spec.json` and `chain-spec-raw.json`
* Your parachain's initial genesis state e.g. `para-2000-genesis-state`
* Your parachain's Wasm runtime e.g. `para-2000.wasm`

> For more advanced customization `pop build spec --help`

Open the `chain-spec.json` file in your editor.&#x20;

Make sure to edit your chain spec and:&#x20;

* **add your account and session keys**
* **specify the starting balance of specific accounts**
* **add the account that will be the sudo account for your parachain**

It should look similar to the below:

```json
{
  "name": "My Parachain",
  "id": "my_parachain",
  "chainType": "Local",
  "bootNodes": [],
  "telemetryEndpoints": null,
  "protocolId": "my_parachain",
  "properties": {
    "ss58Format": 42,
    "tokenDecimals": 12,
    "tokenSymbol": "UNIT"
  },
  "relay_chain": "paseo",
  "para_id": 2000,
  "codeSubstitutes": {},
  "genesis": {
    "runtimeGenesis": {
      "code": "...",
      "patch": {
        "balances": {
          "balances": [
            [
              "INSERT_SS58_STASH_ACCOUNT_KEY_COLLATOR_1",
              1152921504606846976
            ],
            [
              "INSERT_SS58_STASH_ACCOUNT_KEY_COLLATOR_2_OPTIONAL",
              1152921504606846976
            ],
          ]
        },
        "collatorSelection": {
          "candidacyBond": 16000000000,
          "invulnerables": [
            "INSERT_SS58_STASH_ACCOUNT_KEY_COLLATOR_1",
            "INSERT_SS58_STASH_ACCOUNT_KEY_COLLATOR_2_OPTIONAL"
          ]
        },
        "parachainInfo": {
          "parachainId": 2000
        },
        "polkadotXcm": {
          "safeXcmVersion": 4
        },
        "session": {
          "keys": [
            [
              "INSERT_SS58_STASH_ACCOUNT_KEY_COLLATOR_1",
              "INSERT_SS58_STASH_ACCOUNT_KEY_COLLATOR_1",
              {
                "aura": "INSERT_SS58_SESSION_KEY_COLLATOR_1"
              }
            ],
            [
              "INSERT_SS58_STASH_ACCOUNT_KEY_COLLATOR_2_OPTIONAL",
              "INSERT_SS58_STASH_ACCOUNT_KEY_COLLATOR_2_OPTIONAL",
              {
                "aura": "INSERT_SS58_SESSION_KEY_COLLATOR_2_OPTIONAL"
              }
            ]
          ]
        },
        "sudo": {
          "key": "INSERT_SS58_SUDO_ACCOUNT_KEY"
        }
      }
    }
  }
}
```

Since we have modified our chain spec, we will need to re-generate the raw chain spec:

```bash
./target/release/parachain-template-node build-spec --chain chain-spec.json --disable-default-bootnode --raw > chain-spec-raw.json
```

And re-generate the genesis state and wasm:

```
./target/release/parachain-template-node export-genesis-head --chain chain-spec-raw.json > para-2000-gensis-state
```

```
./target/release/parachain-template-node export-genesis-wasm --chain chain-spec-raw.json > para-2000.wasm
```

We are now ready to run our parachain's collator node to sync with Paseo and start producing blocks.

## Run the Collator

In order to run your parachain's collator node you will need the raw chain spec that our local Paseo network is using.&#x20;

This can be found in the output of when you ran the `pop up parachain -f network --verbose` command.:

<figure><img src="../.gitbook/assets/Screenshot 2024-09-18 at 3.56.34 PM.png" alt=""><figcaption><p><code>pop up parachain network.toml</code></p></figcaption></figure>

Copy this chain spec into our `awesome-network` directory:

```bash
cd awesome-network
cp /var/folders/vl/txnq6gdj22s9rn296z0md27w0000gn/T/zombie-ddb5d2aa-704b-4658-af64-3cf9e3be5573/alice/cfg/paseo-local.json paseo-local-raw.json
```

> Note: Your Paseo chain spec path will be different from the above.

We will also need to create a node-key for your collator:

```bash
cd awesome-network
```

```
mkdir -p data/chains/my_parachain/network
```

```
docker run -it parity/subkey:latest generate-node-key > ./data/chains/my_parachain/network/secret_ed25519
```

Run the collator with the following command:

```purebasic
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
--port 30343 \ # Paseo Validator Node Port
--rpc-port 9977 \ # Paseo Validator Node RPC Port
--bootnodes /ip4/127.0.0.1/tcp/51752/p2p/12D3KooWQCkBm1BYtkHpocxCwMgR8yjitEeHGx8spzcDLGt2gkBm
```

> Find the above Paseo validator ports in the output from the `pop up parachain -f network.toml --verbose` command.

We now need to insert the session key into our running collator:

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

It will take time for your collator to sync with the local Paseo Relay chain. Once it is synced your parachain will start producing blocks.

Congrats!&#x20;

## Next Steps

For next steps, you can try the following:

* Designate one collator as the bootnode for the parachain's network
  * Add a second collator

## Resources

#### Learning Resources

* 🧑‍🏫 To learn about Polkadot in general, [Polkadot.network](https://polkadot.network/) website is a good starting point.
  * ⭕ Learn more about parachains [here](https://wiki.polkadot.network/docs/learn-parachains).
* 🧑‍🔧 For technical introduction, [here](https://github.com/paritytech/polkadot-sdk#-documentation) are the Polkadot SDK documentation resources.

**Technical Support**

* [Polkadot Stack Exchange](https://polkadot.stackexchange.com/)
  * Create a question and tag it with "[`pop`](https://substrate.stackexchange.com/tags/pop/info)"
  * Share the StackExchange question in our [Pop Support Telegram channel](https://t.me/pop\_support)
