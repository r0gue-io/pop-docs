---
description: >-
  This guide is for manually onboarding a parachain to the Polkadot "Paseo"
  TestNet Locally
hidden: true
---

# Onboarding Locally

## Introduction

[Paseo](https://x.com/PaseoNetwork) is the community-run Polkadot Relay chain TestNet.

> Note: For contrast, Rococo is the [Parity](https://parity.io)-run Polkadot Relay chain TestNet which [will be deprecating](https://forum.polkadot.network/t/a-new-test-network-for-polkadot/4325) in favour of the [Paseo TestNet](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Fpaseo-rpc.dwellir.com#/explorer).

A good development workflow:&#x20;

1. Run your parachain **locally** using Pop CLI to onboard your parachain to Paseo TestNet automatically for development purposes:
   * [ ] [Run your parachain on Paseo](running-your-parachain.md)
2. When ready to test your parachain in a live environment with other parachains:
   * [ ] Use this guide to mimic the onboarding process for Paseo TestNet. When comfortable with this process locally, then use the next guide to onboard to Paseo Live TestNet.
   * [ ] [Onboard your parachain to Paseo Live TestNet](running-on-paseo-1.md)

Let's get started.

## Spin up Paseo locally

```bash
touch network.toml
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

> Interesting Fact: We have three validator nodes so that we can use Polkadot's Warp Sync to quickly sync with the network. The minimum requirement for Warp Sync are three validator nodes.

Run the network:

```
pop up parachain -f network.toml --verbose
```

> The `--verbose` flag will allow us to see extra information such as the location of the chain spec for the local Paseo network that we are running.

Paseo should now be running on your machine and producing blocks. We can now move towards setting up our parachain.

<figure><img src="../.gitbook/assets/Screenshot 2024-09-24 at 12.20.38 PM.png" alt=""><figcaption><p>pop up parachain -f network.toml --verbose</p></figcaption></figure>

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

Add the stash account to the [Polkadot Signer extension](https://polkadot.js.org/) so that you can see your account appear in the [PolkadotJs Apps](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Fpaseo-rpc.dwellir.com#/accounts) UI.

<figure><img src="../.gitbook/assets/Screenshot 2024-09-24 at 11.14.37 AM.png" alt="" width="375"><figcaption><p>Polkadot Signer Extension</p></figcaption></figure>

Make sure it appears in the PolkadotJs Apps UI:

<figure><img src="../.gitbook/assets/Screenshot 2024-09-24 at 11.17.59 AM.png" alt=""><figcaption><p>PolkadotJs Apps UI</p></figcaption></figure>

Notice that the balance is zero. Let's transfer some tokens from Alice's account to the stash account so that the stash account has funds to do transactions:

<figure><img src="../.gitbook/assets/Screenshot 2024-09-24 at 11.23.53 AM.png" alt=""><figcaption><p>Send Funds</p></figcaption></figure>

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
./target/release/parachain-template-node export-genesis-head --chain chain-spec-raw.json > para-2000-genesis-state
```

```
./target/release/parachain-template-node export-genesis-wasm --chain chain-spec-raw.json > para-2000.wasm
```

We are now ready to run our parachain's collator node to sync with Paseo and start producing blocks.

## Running the Collator

In order to run your parachain's collator node you will need the raw chain spec that our local Paseo network is using.&#x20;

This can be found in the output of when you ran the `pop up parachain -f network --verbose` command.:

<figure><img src="../.gitbook/assets/Screenshot 2024-09-24 at 12.20.38 PM (1).png" alt=""><figcaption></figcaption></figure>

Copy this chain spec into our `my-parachain` directory:

```bash
cd my-parachain
cp /var/folders/vl/txnq6gdj22s9rn296z0md27w0000gn/T/zombie-ddb5d2aa-704b-4658-af64-3cf9e3be5573/alice/cfg/paseo-local.json paseo-local-raw.json
```

> Note: Your Paseo chain spec path will be different from the above.

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
--chain paseo-local-raw.json \
--port 57733 \
--rpc-port 57731 \
--bootnodes /ip4/127.0.0.1/tcp/57733/p2p/12D3KooWQCkBm1BYtkHpocxCwMgR8yjitEeHGx8spzcDLGt2gkBm
```

> The second half of this command specifies the Relay chain node to connect to. In this case, Alice is the validator node that we want to connect to. Alice is the bootnode for the Paseo network that we are running locally. Alice's ports can be found in the output from the `pop up parachain -f network.toml --verbose` command that we previously ran.

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

We now need to onboard the parachain to Paseo.

Go to the Parachains tab.

<figure><img src="../.gitbook/assets/Screenshot 2024-09-24 at 11.37.32 AM.png" alt=""><figcaption><p>Parachains Tab</p></figcaption></figure>

Select "+ ParaId" and make sure to use the stash account:

<figure><img src="../.gitbook/assets/Screenshot 2024-09-24 at 11.38.09 AM.png" alt=""><figcaption><p>Reserve ParaId</p></figcaption></figure>

Next, select "+ ParaThread". Make sure to use the stash account, upload your wasm and genesis state:

<figure><img src="../.gitbook/assets/Screenshot 2024-09-24 at 11.38.50 AM.png" alt=""><figcaption><p>Register ParaThread</p></figcaption></figure>

It will take a moment for the ParaThread to onboard:

<figure><img src="../.gitbook/assets/Screenshot 2024-09-24 at 11.39.09 AM.png" alt=""><figcaption><p>ParaThread Onboarding</p></figcaption></figure>

Once onboarded, we now need to use sudo privileges (Alice account) on the Paseo Relay chain to "force" a lease for our parachain so that it can begin to use produce blocks.

Go to the "sudo" tab:

<figure><img src="../.gitbook/assets/Screenshot 2024-09-24 at 11.45.44 AM.png" alt=""><figcaption><p>Sudo tab</p></figcaption></figure>

Use sudo privileges on the Relay chain to force a lease for our parachain:

<figure><img src="../.gitbook/assets/Screenshot 2024-09-24 at 11.46.09 AM.png" alt=""><figcaption><p>Force a lease</p></figcaption></figure>

> In production, leases are assigned via the result of an on-chain auction.

Wait for the lease to take effect:

<figure><img src="../.gitbook/assets/Screenshot 2024-09-24 at 11.46.30 AM.png" alt=""><figcaption><p>Upgrading Parathread</p></figcaption></figure>

You will then see the parathread upgraded to a parachain:

<figure><img src="../.gitbook/assets/Screenshot 2024-09-24 at 11.50.18 AM.png" alt=""><figcaption><p>Parachain Tab</p></figcaption></figure>

After a few moments, you should see the parachain producing blocks.



Congrats! Your chain is now producing blocks!

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
