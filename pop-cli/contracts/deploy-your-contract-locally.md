# Deploy your contract locally

In order to deploy an ink! smart contract locally we need to spin up a local instance
of [Pop Network](https://github.com/r0gue-io/pop-node).

Pop Network is a parachain meaning that it runs on the Polkadot Relay Chain.

> To take a deep dive into how parachains and the Relay Chain work,
> go [here](https://docs.substrate.io/tutorials/build-a-parachain).

We will use Pop CLI to spin up a live network locally on our machine.

In a separate directory, let's create the configuration file for our local test network.

`./network.toml`

```toml
[relaychain]
chain = "rococo-local"

[[relaychain.nodes]]
name = "alice"
rpc_port = 8833
validator = true

[[relaychain.nodes]]
name = "bob"
validator = true

[[parachains]]
id = 4385
default_command = "pop-node"

[[parachains.collators]]
name = "pop"
rpc_port = 9944
```

We can now use the configuration file to spin up the network.

```shell
pop up parachain -f ./network.toml -p https://github.com/r0gue-io/pop-node                                                                                                                    
```

```
....
┌   Pop CLI : Deploy a parachain
│
▲  The following missing binaries are required: polkadot-parachain-v1.9.0
│  
◆  Would you like to source them automatically now?
│  ● Yes  / ○ No 
│
⚙  They will be cached at /Users/pop/Library/Caches/pop
│  
◐  Sourcing polkadot-parachain-v1.9.0...  
```

> Note: The first time you run this command it will take some time. Grab some coffee. We will be providing
> cross-platform, pre-built binaries in the future to streamline this process.

Once complete, you will get some output:

```
....
◇  Sourcing complete.
│
◇  🚀 Network launched successfully - ctrl-c to terminate
│  ⛓️ rococo-local
│       alice:
│         portal: https://polkadot.js.org/apps/?rpc=ws://127.0.0.1:8833#/explorer
│         logs: tail -f /var/folders/mr/gvb9gkhx58x2mxbpc6dw77ph0000gn/T/zombie-cf760027-2f3e-466d-a611-57ab7fa0a890/alice/alice.log
│       bob:
│         portal: https://polkadot.js.org/apps/?rpc=ws://127.0.0.1:56537#/explorer
│         logs: tail -f /var/folders/vl/txnq6gdj22s9rn296z0md27w0000gn/T/zombie-48a3549c-5ab9-43de-93f5-9057211f3846/bob/bob.log
│  ⛓️ pop-devnet: 4385
│       pop:
│         portal: https://polkadot.js.org/apps/?rpc=ws://127.0.0.1:9944#/explorer
│         logs: tail -f /var/folders/mr/gvb9gkhx58x2mxbpc6dw77ph0000gn/T/zombie-e361d127-6d4a-4ce9-be1d-c735dc34f7bb/pop/pop.log
```

You now have the following running locally on your machine:

> **rococo-local**
>
> * this is the "Polkadot" Relay chain
> * two validator nodes (alice & bob) to run the rococo-local Relay chain
>
> **pop\_devnet**
>
> * this is the Pop Network parachain
> * one collator node is running for the Pop Network parachain

Confirm that the Pop Network parachain is producing blocks by opening the following link in your browser:

* https://polkadot.js.org/apps/?rpc=ws://127.0.0.1:9944#/explorer

![](https://hackmd.io/\_uploads/S1yVLETkC.png)

Cool. Now that we have Pop Network running we can now deploy our contract. Keep the command running and the PolkadotJs
App window open, specifically to see the recent events. After we deploy the contract, we will see the events displayed
there.

In another terminal, and in the contract folder, make sure your contract is built:

```shell
pop build contract
```

Deploy the contract to Pop Network running on the local network:

```shell
pop up contract --constructor new --args "false" --suri //Alice
```

```
┌   Pop CLI : Deploy a smart contract
│
◐  Doing a dry run to estimate the gas...                                                                                                    ●  Gas limit Weight { ref_time: 264725731, proof_size: 16689 }
│  
◇  Contract deployed and instantiated: The Contract Address is "5CLPm1CeUvJhZ8GCDZCR7nWZ2m3XXe4X5MtAQK69zEjut36A"
│
└  Deployment complete
```

The contract has been deployed!

> Save the contract address as we will be used in the next step to interact with the contract.

You can also confirm that the contract was deployed in the recent events in Polkadot
JS: <img src="https://hackmd.io/_uploads/rygPDVpJ0.png" alt="Screenshot 2024-04-05 at 5.33.19 PM" data-size="original">
