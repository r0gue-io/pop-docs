---
description: This guide is for running a parachain on the Polkadot "Paseo" TestNet
hidden: true
---

# Onboarding on Paseo

## Introduction

[Paseo](https://x.com/PaseoNetwork) is the community-run Polkadot Relay chain TestNet.

> Note: For contrast, Rococo is the [Parity](https://parity.io)-run Polkadot Relay chain TestNet which [will be deprecating](https://forum.polkadot.network/t/a-new-test-network-for-polkadot/4325) in favour of the [Paseo TestNet](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Fpaseo-rpc.dwellir.com#/explorer).

A good development workflow:&#x20;

1. Run your parachain **locally** on Paseo TestNet for development purposes&#x20;
2. When ready to test in a live environment with other parachains:
   1. Use this guide to onboard your parachain onto the **live** [Paseo TestNet](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Fpaseo-rpc.dwellir.com#/explorer).

Let's get started.

## Setting Up Polkadot Accounts

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

## Parachain Onboarding

With the account and session keys created, we can now ready our parachain for onboarding.

### Obtain the Parachain ID

To onboard our parachain we need to grab the next available parachain ID on Paseo. We can use [PolkaotJs Apps](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Fpaseo-rpc.dwellir.com#/chainstate) and check the chain state for the next available parachain ID:

<figure><img src="../.gitbook/assets/Screenshot 2024-09-05 at 5.03.46 PM.png" alt=""><figcaption><p>ParaId</p></figcaption></figure>

> Make sure to note down the `paraId`. We will be using this in our next step

### Create the Parachain's Chain Spec

Now that we have our `paraId` we can create our parachain's specification:

```bash
cd my-parachain
```

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

* add your account and session keys
* specify the starting balance of specific accounts
* add the account that will be the sudo account for your parachain

```json
{
  "name": "Awesome Network",
  "id": "awesome_network",
  "chainType": "Live",
  "bootNodes": [],
  "telemetryEndpoints": null,
  "protocolId": "awesome_network",
  "properties": {
    "ss58Format": 42,
    "tokenDecimals": 12,
    "tokenSymbol": "AWE"
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
              "INSERT_SS58_ACCOUNT_KEY_COLLATOR_1",
              1152921504606846976
            ],
            [
              "INSERT_SS58_ACCOUNT_KEY_COLLATOR_2_OPTIONAL",
              1152921504606846976
            ],
          ]
        },
        "collatorSelection": {
          "candidacyBond": 16000000000,
          "invulnerables": [
            "INSERT_SS58_ACCOUNT_KEY_COLLATOR_1",
            "INSERT_SS58_ACCOUNT_KEY_COLLATOR_2_OPTIONAL"
          ]
        },
        "parachainInfo": {
          "parachainId": 4024
        },
        "polkadotXcm": {
          "safeXcmVersion": 4
        },
        "session": {
          "keys": [
            [
              "INSERT_SS58_ACCOUNT_KEY_COLLATOR_1",
              "INSERT_SS58_ACCOUNT_KEY_COLLATOR_1",
              {
                "aura": "INSERT_SS58_SESSION_KEY_COLLATOR_1"
              }
            ],
            [
              "INSERT_SS58_ACCOUNT_KEY_COLLATOR_2_OPTIONAL",
              "INSERT_SS58_ACCOUNT_KEY_COLLATOR_2_OPTIONAL",
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



We are now ready to create an issue on Paseo's Github requesting to onboard our parachain:

* [https://github.com/paseo-network/support/issues](https://github.com/paseo-network/support/issues)

Once you have been confirmed and given a slot, you can start running your collators.

## Run the Collator

In order to run your parachain's collator node you will need the latest version of the Paseo chain spec.&#x20;

You can download that here:&#x20;

* [https://github.com/paseo-network/runtimes/tags](https://github.com/paseo-network/runtimes/tags)

Make sure to select the tag version that Paseo is running, download the zip, and you will find the `paseo.raw.json` file inside the zip folder.

We will also need to create a node-key for your collator:

```bash
mdkir data/chains/awesome_network/network
cd data/chains/awesome_network/network
docker run -it parity/subkey:latest generate-node-key --file=secret_ed25519 --chain=./chain-spec-raw.json
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
--execution wasm \
--chain paseo.raw.json \
--port 30343 \
--rpc-port 9977
```

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

It will take time for your collator to sync with the Paseo Relay chain. Once it is synced your parachain will start producing blocks.

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
