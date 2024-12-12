---
description: >-
  This guide is for manually onboarding a parachain to the Polkadot "Paseo"
  Testnet Locally
---

# Launch a Chain on a Local Test Network

## Introduction

[Paseo](https://x.com/PaseoNetwork) is the community-run Polkadot Relay chain Testnet.

A typical development workflow for launching your chain on Polkadot:&#x20;

* Run your chain **locally** using Pop CLI.&#x20;
  * Under the hood, Pop CLI launches your chain to Paseo Local Testnet automatically for development purposes:
    * [Run your parachain on Paseo](running-your-parachain.md)
* When ready to test your chain in a live environment with other chains:
  * Use this guide to mimic the manual onboarding process for Paseo Testnet locally.&#x20;
  * When comfortable with manually onboarding locally, then use the next guide to onboard to Paseo Live Testnet.
    * [Launching your parachain on Paseo Testnet](launching-your-parachain-on-polkadot/launching-on-paseo.md)
* Finally, when thoroughly tested on Paseo, launch on Polkadot
  * The process here is similar to launching on Paseo.

***

Let's get started.

## Launch Paseo Local

Lets create a configuration file for Paseo first:
```bash
touch network.toml
```
```toml
[relaychain]
chain = "paseo-local"

[relaychain.genesis_overrides.sudo]
key = "5GrwvaEF5zXb26Fz9rcQpDWS57CtERHpNehXCPcNoHGKutQY" # Alice

[[relaychain.nodes]]
name = "alice"
rpc_port = 57731
validator = true

[[relaychain.nodes]]
name = "bob"
validator = true

[[relaychain.nodes]]
name = "charlie"
validator = true
```

As you can see, the sudo account (admin of the chain) is overridden with `Alice` account. This allows us to make changes
to Paseo if needed.

Run the network:
```
pop up parachain -f network.toml --verbose
```

> The `--verbose` flag provides us with extra information such as the location of the Paseo chain spec file. This will become important later so keep this terminal tab as is.

<figure><img src="../.gitbook/assets/Screenshot 2024-09-24 at 12.20.38 PM.png" alt=""><figcaption><p>pop up parachain -f network.toml --verbose</p></figcaption></figure>

Paseo should now be running on your machine and producing blocks.


### Configure Paseo

As of now, Paseo local doesn't provide cores to validate parachain blocks on demand. We will have to make 2 calls to Paseo
using `Alice` as admin account.

First, configure Paseo to set coretime cores to `1`:
```bash
pop call chain --pallet Configuration --function set_coretime_cores --args "1" --url ws://localhost:57731/ --suri //Alice --sudo --skip-confirm
```

> Note: the specified rpc port `57731` is specified in the created `network.toml` file and is to interact with the validator `alice`.

Second, assign the core to the on demand pool:
```bash
pop call chain --url ws://localhost:57731 --call 0xff004a0400000a000000040100e100 --suri //Alice --skip-confirm
```

## Setting Up Accounts

First, we will need to set up a stash account to do transactions on Paseo on behalf of our collator that we will launch later in this guide.

> A collator is the parachain node that will be running for your parachain.

There are multiple ways to generate keys (accounts) on Polkadot, such as:

* [PolkadotJs Signer](https://polkadot.js.org/) or any other custodial&#x20;
* [PolkaVault](https://wiki.polkadot.network/docs/polkadot-vault)&#x20;
* [Subkey](https://paritytech.github.io/polkadot-sdk/master/subkey/index.html)

> Account creation should be done securely, such as using an air-gapped computer.

### Create a Stash Account Key

For the sake of this guide, we will use subkey:

#### Using Docker

```bash
docker pull parity/subkey:latest
```

> Once downloaded, `subkey` does not need an internet connection to work.&#x20;

Let's create the stash account key:

```bash
docker run -it parity/subkey:latest generate --scheme sr25519
```

#### On your Machine

You can download the [polkadot-sdk](https://github.com/paritytech/polkadot-sdk) and run the following command instead:

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

### Fund Stash Account

Now that we have a stash account, using docker or on your local machine, we need to fund this account with some tokens so that it has funds to perform transactions on behalf of the collator.

```bash
pop call chain --pallet Balances --function transfer_allow_death --url ws://localhost:57731/ --suri //Alice
```
```bash
┌   Pop CLI : Call a chain
│
◇  Select the value for the parameter: dest
│  Id 
│
◇  Enter the value for the parameter: Id
│  <STASH ACCOUNT SS58 ADDRESS>
│
◇  Enter the value for the parameter: value
│  1000000000000000
│
...
```

Cool. Our stash account is now funded on the Paseo Relay chain.

### Create a Session Account

We now need to create one more account (optionally if you are just testing and you can use the stash account). In order for collators to produce blocks, they need to sign the block with a (session) key. This account is specifically created for block production.

We can use `subkey` again to generate the keys for us.

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

We now have all the accounts we need and we can start prepare our chain!

## Setting up the Chain

For the sake of this exercise, let's create a new chain project:
```
pop new parachain my-chain
```
> The folder includes a `network.toml` file which can be ignored. This is to launch a network with the chain already onboarded.


### Creating the chain spec

The chain specification holds all the information the node requires to start or sync with the chain's network.

Let's generate a chain spec:

<figure><img src="../.gitbook/assets/buildspec.gif" alt="pop build spec"><figcaption><p>pop build spec</p></figcaption></figure>

```
cd my-chain
pop build spec --profile release --id 2000 --type local --relay paseo-local --protocol-id my_chain --chain local --genesis-state --genesis-code
```
```bash
┌   Pop CLI : Generate your chain spec
│
◇  Name or path for the plain chain spec file:
│  ./chain-spec.json
│
◇  Would you like to use local host as a bootnode ?
│  No 
│
┌   Pop CLI : Building your chain spec
│
◇  Chain specification built successfully.
│
◆  Generated files:
│  ● Plain text chain specification file generated at: ./chain-spec.json
│  ● Raw chain specification file generated at: ./chain-spec-raw.json
│  ● WebAssembly runtime file exported at: ./para-2000.wasm
│  ● Genesis State file exported at: ./para-2000-genesis-state
│  
└  Need help? Learn more at https://learn.onpop.io
```

> For more advanced customization `pop build spec --help`

Open the `chain-spec.json` file in your editor.&#x20;

Make sure to edit your chain spec and:&#x20;

* **add your account and session keys**
* **specify the starting balance of specific accounts**
* **add the account that will be the sudo account for your chain**

It should look similar to the below:

```json
{
  "name": "My Chain",
  "id": "my_chain",
  "chainType": "Local",
  "bootNodes": [],
  "telemetryEndpoints": null,
  "protocolId": "my_chain",
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

Since we have modified our chain spec, we will need to re-generate the raw chain spec, genesis state and wasm:

```bash
pop build spec --chain chain-spec.json --disable-default-bootnode --genesis-state --genesis-code  
```
```bash
┌   Pop CLI : Generate your chain spec
│
◇  An existing chain spec file is provided. Do you want to make additional changes to it?
│  No 
```

> Pop CLI allows you to provide the path to an existing chain spec file to edit or regenerate the artifacts.

We are now ready to run to sync with Paseo and start producing blocks!

## Launch the Chain

In order to run your parachain's collator you will need the raw chain spec that our local Paseo network is using.&#x20;

This can be found in the output when you ran the `pop up parachain -f network --verbose` command:

<figure><img src="../.gitbook/assets/Screenshot 2024-09-24 at 12.20.38 PM (1).png" alt=""><figcaption></figcaption></figure>

Copy this chain spec into our `my-chain` directory:

```bash
cd my-chain
cp /var/folders/vl/txnq6gdj22s9rn296z0md27w0000gn/T/zombie-ddb5d2aa-704b-4658-af64-3cf9e3be5573/alice/cfg/paseo-local.json paseo-local-raw.json
```

> Note: Your Paseo chain spec path will be different from the above.

### Generate Node Key

We will also need to create a node-key for your collator:

```bash
cd my-chain
mkdir -p data/chains/my_chain/network
docker run -it parity/subkey:latest generate-node-key > ./data/chains/my_chain/network/secret_ed25519
```

> Alternatively you can use Polkadot SDK binary instead of a Docker image:
> ```
> path/to/polkadot-sdk/target/debug/substrate-node key generate-node-key --file=secret_ed25519 --chain=./chain-spec-raw.json
> ```
>
> <pre><code><strong>mv secret_ed25519 data/chains/my_chain/network
> </strong></code></pre>

### Run Collator

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
--rpc-port 57731
```

> The second half of this command specifies the Relay chain node to connect to.

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

Ensure the chain is synced with Paseo:
```bash
2024-12-10 09:06:05 [Relaychain] Warp sync is complete, continuing with state sync.    
2024-12-10 09:06:06 [Relaychain] State sync is complete, continuing with block sync.    
2024-12-10 09:06:06 [Relaychain] 🏆 Imported #47 (0xb04f…12a4 → 0x6adf…6582)    
2024-12-10 09:06:06 [Relaychain] 🏆 Imported #48 (0x6adf…6582 → 0xd036…e502) 
```

We now need to onboard the chain to Paseo.

### Reserve Para ID

We can reserve a para ID for the chain using pop cli:

```bash
pop call chain --url ws://localhost:57731
```

 
```bash
┌   Pop CLI : Call a chain
│
◇  What would you like to do?
│  Reserve a parachain ID 
│
◇  Signer of the extrinsic:
│  <STASH ACCOUNT>
│  
...
       Event Balances ➜ Reserved
         who: <STASH ACCOUNT>
         amount: 100UNIT
       Event Registrar ➜ Reserved
         para_id: Id(2000)
         who: <STASH ACCOUNT>
...         
```

In the events we can see the `para_id` that is assigned to the chain. Make sure this is the para ID specified in the chain spec file (and thus the chain artifacts).

### Register Para ID with Genesis State & Code

Now we register the para ID with the generated genesis state (`para-2000-genesis-state`) and genesis code (`para-2000.wasm`).

```bash
│
◇  Do you want to perform another call?
│  Yes
│
◇  What would you like to do?
│  Register a parachain ID with genesis state and code
│
◇  Enter the value for the parameter: id
│  2000
│
◇  The value for `genesis_head` might be too large to enter. You may enter the path to a file instead.
│  para-2000-genesis-state
│
◇  The value for `validation_code` might be too large to enter. You may enter the path to a file instead.
│  para-2000.wasm
│
◇  Signer of the extrinsic:
│  <STASH ACCOUNT>
│
...
       Event Balances ➜ Withdraw
         who: <STASH ACCOUNT>
         amount: 90.71989507390UNIT
       Event Balances ➜ Reserved
         who: <STASH ACCOUNT>
         amount: 3.145826kUNIT
       Event Paras ➜ PvfCheckStarted
         0: ValidationCodeHash(0x1821617486094e18595084b580fe9324a084adedbf80ec61c9d7b75736ab5f5b)
         1: Id(2000)
       Event Registrar ➜ Registered
         para_id: Id(2000)
         manager: <STASH ACCOUNT>
...
```

Your chain is now registered on Paseo!

### Buy an On Demand Core

Now we need to buy a core to have Paseo validate a block.

```bash
│
◇  Do you want to perform another call?
│  Yes 
│
◇  What would you like to do?
│  Purchase on-demand coretime 
│
◇  Enter the value for the parameter: max_amount
│  10000000
│
◇  Enter the value for the parameter: para_id
│  2000
│
◇  Signer of the extrinsic:
│  <STASH ACCOUNT>
...
       Event OnDemand ➜ OnDemandOrderPlaced
         para_id: Id(2000)
         spot_price: 1mUNIT
         ordered_by: <STASH ACCOUNT>
...
```
         
Congrats! If your parachain produced another block it means that your first block is now validated by Paseo!

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
