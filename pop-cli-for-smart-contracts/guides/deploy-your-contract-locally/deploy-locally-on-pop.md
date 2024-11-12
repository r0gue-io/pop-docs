---
description: >-
  This guide will teach you how to deploy your ink! smart contract to Pop
  locally
---

# Deploy your contract on Pop locally

### Deploy your contract locally <a href="#deploy-your-contract-locally" id="deploy-your-contract-locally"></a>

In order to deploy an ink! smart contract locally we need to spin up a local instance of [Pop](https://github.com/r0gue-io/pop-node).

Pop Network is a parachain meaning that it runs on the Polkadot Relay chain.

> To take a deep dive into how Parachains and the Relay chain work, go [here](https://docs.substrate.io/tutorials/build-a-parachain).

We will use Pop CLI to spin up a live network locally on our machine.

In a separate directory, let's create the configuration file for our local test network.

./network.toml

```
[relaychain]
chain = "paseo-local"

[[relaychain.nodes]]
name = "alice"
rpc_port = 8833
validator = true

[[relaychain.nodes]]
name = "bob"
validator = true

[[parachains]]
id = 4001
default_command = "pop-node"

[parachains.genesis_overrides.balances]
balances = [
    # Dev accounts
    ["5GrwvaEF5zXb26Fz9rcQpDWS57CtERHpNehXCPcNoHGKutQY", 10000000000000000],
    ["5FHneW46xGXgs5mUiveU4sbTyGBzmstUspZC92UhjJM694ty", 10000000000000000],
    ["5FLSigC9HGRKVhB9FiEo4Y3koPsNmBmLJbpXg2mp1hXcS59Y", 10000000000000000],
    ["5DAAnrj7VHTznn2AWBemMuyBwZWs6FNFjdyVXUeYum3PTXFy", 10000000000000000],
    ["5HGjWAeFDfFCWPsjFQdVV2Msvz2XtMktvgocEZcCj68kUMaw", 10000000000000000],
    ["5CiPPseXPECbkjWCa6MnjNokrgYjMqmKndv2rSnekmSK2DjL", 10000000000000000],
]

[[parachains.collators]]
name = "pop"
rpc_port = 9944
args = ["-lruntime::contracts=debug", "-lpopapi::extension=debug", "--enable-offchain-indexing=true"]

[[parachains]]
id = 1000
chain = "asset-hub-rococo-local"

[[parachains.collators]]
name = "asset-hub"
args = ["-lxcm=trace"]
rpc_port = 9977
```

We can now use the configuration file to spin up the network.

```
pop up parachain -f ./network.toml

┌   Pop CLI : Launch a local network
│
▲  ⚠️ The following binaries required to launch the network cannot be found locally:
│     > pop-node
│  
◇  📦 Would you like to source them automatically now? It may take some time...
│     > pop-node testnet-v0.4.2
│  Yes 
│
▲  ℹ️ The following binaries have newer versions available:
│     > polkadot v1.14.0 -> stable2409, polkadot-parachain v1.14.0 -> stable2409
│  
◇  📦 Would you like to source them automatically now? It may take some time...
│  Yes 
│
⚙  ℹ️ Binaries will be cached at /Users/bruno/Library/Caches/pop
│  
◇  📦 Sourcing binaries...
│  ✅  polkadot
│  ✅  pop-node
│  ✅  polkadot-parachain                                                                                                                
```

> The first time you run this command it will take some time. Grab some coffee.

Once complete, you will get some output:

```
....
◇  Sourcing complete.
│
◇  🚀 Network launched successfully - ctrl-c to terminate
│  ⛓️ paseo-local
│       alice:
│         portal: https://polkadot.js.org/apps/?rpc=ws://127.0.0.1:8833#/explorer
│         logs: tail -f /var/folders/vl/txnq6gdj22s9rn296z0md27w0000gn/T/zombie-2e522b81-b430-4897-b647-058fde52bbaa/alice/alice.log
│       bob:
│         portal: https://polkadot.js.org/apps/?rpc=ws://127.0.0.1:62339#/explorer
│         logs: tail -f /var/folders/vl/txnq6gdj22s9rn296z0md27w0000gn/T/zombie-2e522b81-b430-4897-b647-058fde52bbaa/bob/bob.log
│  ⛓️ pop-devnet: 4001
│       pop:
│         portal: https://polkadot.js.org/apps/?rpc=ws://127.0.0.1:9944#/explorer
│         logs: tail -f /var/folders/vl/txnq6gdj22s9rn296z0md27w0000gn/T/zombie-2e522b81-b430-4897-b647-058fde52bbaa/pop/pop.log
│  ⛓️ asset-hub-rococo-local: 1000
│       asset-hub:
│         portal: https://polkadot.js.org/apps/?rpc=ws://127.0.0.1:9977#/explorer
│         logs: tail -f /var/folders/vl/txnq6gdj22s9rn296z0md27w0000gn/T/zombie-2e522b81-b430-4897-b647-058fde52bbaa/asset-hub/asset-hub.log
```

You now have the following running locally on your machine:

> **paseo-local**
>
> * this is the Polkadot "Test" Relay chain
>   * two validator nodes (alice & bob) to run the paseo-local Relay chain
>
> **asset-hub-rococo-local**
>
> * this is the Asset Hub system parachain that manages assets in Polkadot
>   * one collator node is running for this system chain
>   * it is useful to have a second parachain like this one to test cross-chain capabilities
>
> **pop-devnet**
>
> * this is the Pop parachain
>   * one collator node is running for the Pop Network parachain

Confirm that the Pop parachain is producing blocks by opening the PolkadotJS link in your browser:

* https://polkadot.js.org/apps/?rpc=ws://127.0.0.1:9944#/explorer

> Replace the port number with the port number for the Pop Network parachain that is outputted in your terminal

<figure><img src="../../.gitbook/assets/Screenshot 2024-11-12 at 1.11.26 PM.png" alt=""><figcaption></figcaption></figure>

Cool. Now that we have Pop running we can now deploy our contract.

Keep the PolkadotJs App window open, specifically to see the recent events. After we deploy the contract, we will see the events displayed there.

You will need to know the RPC URL for Pop Network which was outputted in the terminal when you ran the `pop up` command.

```
pop up contract -p ./flipper --constructor new --args false --suri //Alice --url ws://127.0.0.1:9944

┌   Pop CLI : Deploy a smart contract
│
◐  Doing a dry run to estimate the gas...                                                                                                    
●  Gas limit Weight { ref_time: 264725731, proof_size: 16689 }
│  
◇  Contract deployed and instantiated: The Contract Address is "5CLPm1CeUvJhZ8GCDZCR7nWZ2m3XXe4X5MtAQK69zEjut36A"
│
└  Deployment complete
```

The contract has been deployed!

> Save the contract address as we will be used in the next step to interact with the contract.

You can also confirm that the contract was deployed in the recent events in Polkadot JS:

<figure><img src="https://pop-platform.gitbook.io/~gitbook/image?url=https%3A%2F%2Fhackmd.io%2F_uploads%2FrygPDVpJ0.png&#x26;width=300&#x26;dpr=2&#x26;quality=100&#x26;sign=a8ac462aa39b37e82c48e1f7203e8f6d1404c323050a90552e672a440830a5e0" alt="" width="563"><figcaption></figcaption></figure>

### Interacting with our contract <a href="#interacting-with-our-contract" id="interacting-with-our-contract"></a>

Now that the contract is live, we can start interacting with it.

Grab the contract address that was outputted from the previous step.

> If you lost the address, you can always pull up PolkadotJs Apps and check the chain state for the contract address.

```
pop call contract -p ./flipper --contract 5CLPm1CeUvJhZ8GCDZCR7nWZ2m3XXe4X5MtAQK69zEjut36A --message get --suri //Alice --url ws://127.0.0.1:4385

┌   Pop CLI : Calling a contract
│
◐  Calling the contract...                                                                                                                   
⚙  Result: Ok(false)
│  
▲  Your call has not been executed.
│  
▲  To submit the transaction and execute the call on chain, add -x/--execute flag to the command.
│  
└  Call completed successfully!
```

The result here is `false` meaning that `value` in the flipper storage is set to `false`.

Let's try _flipping_ it.

```
pop call contract -p ./flipper --contract 5CLPm1CeUvJhZ8GCDZCR7nWZ2m3XXe4X5MtAQK69zEjut36A --message flip --suri //Alice --url ws://127.0.0.1:9944 -x

┌   Pop CLI : Calling a contract
│
◐  Doing a dry run to estimate the gas...                                                                                                    
⚙  Gas limit Weight { ref_time: 382198493, proof_size: 18636 }
│  
◓  Calling the contract...                                                                                                                   
⚙        Events
│         Event Balances ➜ Withdraw
│           who: 5GrwvaEF5zXb26Fz9rcQpDWS57CtERHpNehXCPcNoHGKutQY
│           amount: 1.550397502mUNIT
│         Event Contracts ➜ Called
│           caller: Signed(5GrwvaEF5zXb26Fz9rcQpDWS57CtERHpNehXCPcNoHGKutQY)
│           contract: 5CLPm1CeUvJhZ8GCDZCR7nWZ2m3XXe4X5MtAQK69zEjut36A
│         Event TransactionPayment ➜ TransactionFeePaid
│           who: 5GrwvaEF5zXb26Fz9rcQpDWS57CtERHpNehXCPcNoHGKutQY
│           actual_fee: 1.550397502mUNIT
│           tip: 0UNIT
│         Event System ➜ ExtrinsicSuccess
│           dispatch_info: DispatchInfo { weight: Weight { ref_time: 1302708493, proof_size: 9064 }, class: Normal, pays_fee: Yes }
│  
└  Call completed successfully!
```

> Notice the `-x` flag at the end of the `pop call` command. This is needed because we are executing a transaction on the network and it will cost gas.

Did it work? Let's see if the value has been flipped.

```
pop call contract -p ./flipper --contract 5CLPm1CeUvJhZ8GCDZCR7nWZ2m3XXe4X5MtAQK69zEjut36A --message get --suri //Alice --url ws://127.0.0.1:9944

┌   Pop CLI : Calling a contract
│
◐  Calling the contract...                                                                                                                   
⚙  Result: Ok(true)
│  
▲  Your call has not been executed.
│  
▲  To submit the transaction and execute the call on chain, add -x/--execute flag to the command.
│  
└  Call completed successfully!
```

The `value` is now `true`.

Awesome we now know how to make calls to our smart contract.
