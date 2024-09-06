---
description: The guide is for running a parachain on the Polkadot "Paseo" TestNet
hidden: true
---

# Running on Paseo

[Paseo](https://x.com/PaseoNetwork) is the community-run Polkadot Relay chain TestNet.

> Note: For contrast, Rococo is the [Parity](https://parity.io)-run Polkadot Relay chain TestNet which [will be deprecating](https://forum.polkadot.network/t/a-new-test-network-for-polkadot/4325) in favour of the [Paseo TestNet](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Fpaseo-rpc.dwellir.com#/explorer).

A good development workflow:&#x20;

1. Run your parachain **locally** on Paseo TestNet for development purposes&#x20;
2. When ready to test in a live environment with other parachains, deploy on the **live** [Paseo TestNet](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Fpaseo-rpc.dwellir.com#/explorer).

Let's get started.

First, we will need to set up an account to do transactions on Paseo on behalf of your collator.

There are multiple ways to generate keys (accounts) on Polkadot, such as:

* [PolkadotJs Signer](https://polkadot.js.org/) or any other custodial&#x20;
* [PolkaVault](https://wiki.polkadot.network/docs/polkadot-vault)&#x20;
* [Subkey](https://paritytech.github.io/polkadot-sdk/master/subkey/index.html)

> Account creation should be done securely, such as using an air-gapped computer.

For the sake of this guide, we will use subkey:

```bash
docker pull parity/subkey:latest
```

> `subkey` does not need an internet connection to work.

Let's create the account key:

```bash
docker run -it parity/subkey:latest generate --scheme sr25519
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

> Save the secret phrase in a vault securely

Now that we have an account, we need to fund this account with some tokens so that it has funds to perform transactions on behalf of the collator.

Go to the [Polkadot Faucet](https://faucet.polkadot.io/) and fund your account:

<figure><img src="../.gitbook/assets/Screenshot 2024-09-06 at 4.04.37 PM.png" alt="" width="563"><figcaption><p>Polkadot / Paseo Faucet</p></figcaption></figure>

Cool. Make sure to add your account to a [Polkadot Signer extension](https://polkadot.js.org/) so that you can see your account appear in the [PolkadotJs Apps](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Fpaseo-rpc.dwellir.com#/accounts) UI.

> Alternatively, you can check the Paseo chain state under [system.account()](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Fpaseo-rpc.dwellir.com#/chainstate) to see your account balance.

You may have have to wait a couple minutes to see the funds appear in [your account on Paseo.](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Fpaseo-rpc.dwellir.com#/accounts)

We now have an account to manage funds for transactions related to managing our collator.

We will need to create one more account. In order for collators to produce blocks, they need to sign the block with an (account) key. We call this account key the session key.

Let's go ahead and create the session key:



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

With the account and session keys created we can now ready our parachain for onboarding.

To do this we need to grab the next available parachain ID. We can use [PolkaotJs Apps](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Fpaseo-rpc.dwellir.com#/chainstate) and check the chain state for the next available parachain ID:

<figure><img src="../.gitbook/assets/Screenshot 2024-09-05 at 5.03.46 PM.png" alt=""><figcaption><p>ParaId</p></figcaption></figure>

> Make sure to note down the ParaId, we will be using this in our next step

Now that we have our paraId we can create our parachain's specification:

```bash
cd my-parachain
```

```
pop build spec --release --id 4024 --type live --relay paseo --protocol-id my-parachain
```

This will output our chain spec.

Make sure to edit your chain spec and add your account and session keys:

```json
{
  "name": "INSERT_NAME",
  "id": "INSERT_ID",
  "chainType": "Local",
  "bootNodes": [],
  "telemetryEndpoints": null,
  "protocolId": "INSERT_PROTOCOL_ID",
  "properties": {
    "ss58Format": 42,
    "tokenDecimals": 12,
    "tokenSymbol": "UNIT"
  },
  "relay_chain": "paseo",
  "para_id": 1,
  "codeSubstitutes": {},
  "genesis": {
    "runtimeGenesis": {
      "code": "...",
      "patch": {
        "balances": {},
        "collatorSelection": {
          "candidacyBond": 16000000000,
          "invulnerables": [
            "INSERT_ACCOUNT_ID_COLLATOR_1",
            "INSERT_ACCOUNT_ID_COLLATOR_2"
          ]
        },
        "parachainInfo": {
          "parachainId": 1
        },
        "polkadotXcm": {
          "safeXcmVersion": 4
        },
        "session": {
          "keys": [
            [
              "INSERT_ACCOUNT_ID_COLLATOR_1",
              "INSERT_ACCOUNT_ID_COLLATOR_1",
              {
                "aura": "INSERT_SESSION_KEY_COLLATOR_1"
              }
            ],
            [
              "INSERT_ACCOUNT_ID_COLLATOR_2",
              "INSERT_ACCOUNT_ID_COLLATOR_2",
              {
                "aura": "INSERT_SESSION_KEY_COLLATOR_2"
              }
            ]
          ]
        },
        "sudo": {
          "key": "INSERT_SUDO_ACCOUNT"
        }
      }
    }
  }
}
```

We are now ready to create an issue on Paseo's Github requesting to onboard our parachain:

* [https://github.com/paseo-network/support/issues](https://github.com/paseo-network/support/issues)

Once you have been confirmed and given a slot, you can start running your collators.



In order to run your collator you will need the latest version of the Paseo chain spec. You can download that here:&#x20;

* [https://github.com/paseo-network/runtimes/tags](https://github.com/paseo-network/runtimes/tags)

Make sure to select the latest tag version, download the zip, and you will find the `paseo.raw.json` file inside the zip folder.

Run the collator with the following command:

```purebasic
./target/release/parachain-template-node \
--collator \
--force-authoring \
--chain raw-parachain-chainspec.json \
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
    "INSERT_SECRET_PHRASE",
    "INSERT_PUBLIC_KEY_HEX_FORMAT"
  ],
  "id":1
}' \
http://localhost:8845
```

#### Learning Resources

* 🧑‍🏫 To learn about Polkadot in general, [Polkadot.network](https://polkadot.network/) website is a good starting point.
  * ⭕ Learn more about parachains [here](https://wiki.polkadot.network/docs/learn-parachains).
* 🧑‍🔧 For technical introduction, [here](https://github.com/paritytech/polkadot-sdk#-documentation) are the Polkadot SDK documentation resources.

**Technical Support**

* [Polkadot Stack Exchange](https://polkadot.stackexchange.com/)
  * Create a question and tag it with "[`pop`](https://substrate.stackexchange.com/tags/pop/info)"
  * Share the StackExchange question in our [Pop Support Telegram channel](https://t.me/pop\_support)
