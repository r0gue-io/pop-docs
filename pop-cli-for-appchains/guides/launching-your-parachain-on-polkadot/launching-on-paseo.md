---
description: This guide is for onboarding a parachain to the Polkadot "Paseo" Testnet
coverY: 0
layout:
  cover:
    visible: false
    size: hero
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Launching your parachain on Paseo Testnet

Let's start by creating the accounts needed for launching the parachain.

## Setting Up Accounts

First, we will need to set up a stash account to do transactions on Paseo to onboard our parachain.

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

Now that we have a stash account, we need to fund this account with some tokens so that it has funds to perform transactions on behalf of the collator such as transactions related to connecting your parachain to the Paseo network.

Add the stash account to the [Polkadot Signer extension](https://polkadot.js.org/) so that you can see your account appear in the [PolkadotJs Apps](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Fpaseo-rpc.dwellir.com#/accounts) UI.

<figure><img src="../../.gitbook/assets/Screenshot 2024-09-24 at 11.14.37 AM.png" alt="" width="375"><figcaption><p>Polkadot Signer Extension</p></figcaption></figure>

Go to the [Polkadot Faucet](https://faucet.polkadot.io/) and fund your account:

<figure><img src="../../.gitbook/assets/Screenshot 2024-09-06 at 4.04.37 PM.png" alt="" width="563"><figcaption><p>Polkadot / Paseo Faucet</p></figcaption></figure>

Cool. Our stash account is now funded on the Paseo Relay chain.

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

## Parachain Onboarding

With the account and session keys created, we can now ready our parachain for onboarding.

### Obtain the Parachain ID

To onboard our parachain we need to grab the next available parachain ID on Paseo. We can use [PolkaotJs Apps](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Fpaseo-rpc.dwellir.com#/chainstate) and check the chain state for the next available parachain ID:

<figure><img src="../../.gitbook/assets/Screenshot 2024-09-05 at 5.03.46 PM.png" alt=""><figcaption><p>ParaId</p></figcaption></figure>

> Make sure to note down the `paraId`. We will be using this in our next step

## Creating the chain spec for the parachain

Let's create a chain spec for our parachain:

```
cd my-parachain
```

Build your parachain for release:

```
pop build --release
```

Create the chain spec:

```
pop build spec --release --id 4024 --type live --relay paseo --protocol-id my_parachain
```

This will output the following:

* Your parachain's chain specification file
  * The plain text and the raw version e.g. `chain-spec.json` and `chain-spec-raw.json`
* Your parachain's initial genesis state e.g. `para-4024-genesis-state`
* Your parachain's Wasm runtime e.g. `para-4024.wasm`

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
  "chainType": "Live",
  "bootNodes": [],
  "telemetryEndpoints": null,
  "protocolId": "my_parachain",
  "properties": {
    "ss58Format": 42,
    "tokenDecimals": 12,
    "tokenSymbol": "UNIT"
  },
  "relay_chain": "paseo",
  "para_id": 4024,
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

\
Since we have modified our chain spec, we will need to re-generate the raw chain spec:

```bash
./target/release/parachain-template-node build-spec --chain chain-spec.json --disable-default-bootnode --raw > chain-spec-raw.json
```

And re-generate the genesis state and wasm:

```
./target/release/parachain-template-node export-genesis-head --chain chain-spec-raw.json > para-2000-genesis-state
```

```
./target/release/parachain-template-node export-genesis-wasm --chain chain-spec-raw.json > para-2000.wasm
```

We are now ready to run our parachain's collator node to sync with Paseo and start producing blocks.

## Requesting a Slot on Paseo

We are now ready to create an issue on Paseo's Github requesting to onboard our parachain:

* [https://github.com/paseo-network/support/issues](https://github.com/paseo-network/support/issues)

Once you have been confirmed and given a slot, you can start running your collators.

## Running the Collator

In order to run your parachain's collator node you will need the raw chain spec that our local Paseo network is using.&#x20;

You can download that here:&#x20;

* [https://github.com/paseo-network/runtimes/tags](https://github.com/paseo-network/runtimes/tags)

Make sure to select the tag version that Paseo is running, download the zip, and you will find the `paseo.raw.json` file inside the zip folder.

We will also need to create a node-key for your collator:

```bash
cd my-parachain
```

```
mkdir -p data/chains/my_parachain/network
```

```
docker run -it parity/subkey:latest generate-node-key > ./data/chains/my_parachain/network/secret_ed25519
```

> Alternatively you can use Polkadot SDK binary instead of a Docker image:
>
> ```
> path/to/polkadot-sdk/target/debug/substrate-node key generate-node-key --file=secret_ed25519 --chain=./chain-spec-raw.json
> ```
>
> <pre><code><strong>mv secret_ed25519 data/chains/my_parachain/network
> </strong></code></pre>
>
>

Run the collator with the following command:

> A note on hardware requirements:&#x20;
>
> * [https://paritytech.github.io/devops-guide/guides/parachain\_deployment.html#hardware](https://paritytech.github.io/devops-guide/guides/parachain\_deployment.html#hardware)

```
./target/release/parachain-template-node \
--collator \
--force-authoring \
--chain chain-spec-raw.json \
--base-path ./data \
--port 40333 \
--rpc-port 8845 \
-- \
--sync warp \
--chain paseo.raw.json \
--port 57733 \
--rpc-port 57731
```

> Choose ports that are not in use.&#x20;

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

It will take time for your collator to sync with the local Paseo Relay chain.

Once it is fully synced, block production will start.

Congrats!

## Next Steps

For next steps, you can try the following:

* Designate one collator as the bootnode for the parachain's network
  * Add a second collator

## Resources

#### Learning Resources

* [https://paritytech.github.io/devops-guide/guides/parachain\_deployment.html](https://paritytech.github.io/devops-guide/guides/parachain\_deployment.html)
* 🧑‍🏫 To learn about Polkadot in general, [Polkadot.network](https://polkadot.network/) website is a good starting point.
  * ⭕ Learn more about parachains [here](https://wiki.polkadot.network/docs/learn-parachains).
* 🧑‍🔧 For technical introduction, [here](https://github.com/paritytech/polkadot-sdk#-documentation) are the Polkadot SDK documentation resources.

**Technical Support**

* [Polkadot Stack Exchange](https://polkadot.stackexchange.com/)
  * Create a question and tag it with "[`pop`](https://substrate.stackexchange.com/tags/pop/info)"
  * Share the StackExchange question in our [Pop Support Telegram channel](https://t.me/pop\_support)
