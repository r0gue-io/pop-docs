---
description: >-
  In this tutorial developers will learn how to deploy an ink! smart contract to
  Pop Network.
---

# Your First Ink! Smart Contract

## Audience

Developers who want to learn how to:

* use the [Pop CLI](https://github.com/r0gue-io/pop-cli) tool to speed up development
* deploy smart contracts to [Pop Network](https://github.com/r0gue-io/pop-node)

## Learning Objectives

On completion of this tutorial, developers will be able to:

* create, build, test, and deploy ink! smart contracts
* deploy smart contracts to [Pop Network](https://github.com/r0gue-io/pop-node) locally and on [Paseo](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Frpc.ibp.network%2Fpaseo) (Polkadot's Test Network)
* interact with the deployed contract

## Prerequisites

Developers should have knowledge of the following:

* a firm understanding of the [Rust programming language](https://doc.rust-lang.org/book)
* a general concept of what smart contracts are
* general knowledge of Web3 and [blockchain](https://docs.substrate.io/learn/blockchain-basics)
* [install the Rust toolchain](https://docs.substrate.io/tutorials/smart-contracts/prepare-your-first-contract)

## Installing Pop CLI

Let's install the powerful [Pop CLI](https://github.com/r0gue-io/pop-cli) tool which will be used throughout this tutorial.

```shell
cargo install --locked --git https://github.com/r0gue-io/pop-cli
```

Make sure the [Pop CLI](https://github.com/r0gue-io/pop-cli) tool has been installed correctly by running the `pop` command in your terminal.

```shell
pop --help

An all-in-one tool for Polkadot development.

Usage: pop <COMMAND>

Commands:
  new    Generate a new parachain, pallet or smart contract
  build  Build a parachain or smart contract
  call   Call a smart contract
  up     Deploy a parachain or smart contract
  test   Test a smart contract
  help   Print this message or the help of the given subcommand(s)

Options:
  -h, --help     Print help
  -V, --version  Print version
```

## Getting Started

Questions:

1. What are ink! smart contracts?
2. What are the benefits of using the ink! programming language?
3. Why use ink! to develop smart contracts?
4. What makes ink! so nice?

Take 10 minutes to skim over the [ink! documentation](https://use.ink) and answer the above questions.

### ink! contracts are smart(er)

ink! allows you to write smart contracts using Rust. That means you get all the benefits of the Rust programming language:

* [https://use.ink/why-rust-for-smart-contracts](https://use.ink/why-rust-for-smart-contracts)

The ink! embedded domain-specific language (eDSL) retro-fits your Rust code with powerful features that enhance smart contract development. In doing so, ink! improves the developer ergonomics when it comes to using Rust for writing smart contracts.

Let's create a simple ink! smart contract:

```
pop new contract flipper

┌   Pop CLI : Generating new contract "flipper"!
│
◇  Smart contract created! Located in the following directory "/flipper"
│
└  cd into "flipper" and enjoy hacking! 🚀
```

By default, when we generate an ink! smart contract using the command above, we get a template (the "flipper" template). It serves as a good starting point for contract development.

Let's look at our first bit of ink! code.

```
cd flipper
```

You will see the following in this directory:

```
flipper
  └─ lib.rs
  └─ Cargo.toml
  └─ .gitignore
```

Inside `lib.rs` you will find all the contract logic.

Take a moment to get familiar with the ink! programming language:

* [https://use.ink/basics/contract-template](https://use.ink/basics/contract-template)

## Building the ink! smart contract

Let's build flipper! Make sure you are inside the flipper directory and run the following command:

```
pop build contract

┌   Pop CLI : Building a contract
│
 [==] Checking clippy linting rules
   Compiling proc-macro2 v1.0.79
   Compiling unicode-ident v1.0.12
   Compiling syn v1.0.109
   Compiling equivalent v1.0.1
   Compiling hashbrown v0.14.3
   Compiling winnow v0.5.40
   ....
   Finished release [optimized] target(s) in 21.17s
   Running `target/ink/release/metadata-gen`
 [==] Generating bundle
◆  
│  Original wasm size: 21.3K, Optimized: 1.6K
│  
│  The contract was built in RELEASE mode.
│  
│  Your contract artifacts are ready. You can find them in:
│  /Users/bruno/src/flipper/target/ink
│  
│    - flipper.contract (code + metadata)
│    - flipper.wasm (the contract's code)
│    - flipper.json (the contract's metadata)
│  
└  Build completed successfully!
```

Awesome, your contract is built!

With Pop CLI, you can also run your smart contract tests.

## Run your ink! smart contract unit tests

```
pop test contract

┌   Pop CLI : Building a contract
│
 [==] Checking clippy linting rules
   Compiling proc-macro2 v1.0.79
   ....
   Compiling flipper v0.1.0 (/Users/bruno/src/pop-cli/flipper)
    Finished test [unoptimized + debuginfo] target(s) in 8.06s
     Running unittests lib.rs (target/debug/deps/flipper-ff1505839f6873de)

running 2 tests
test flipper::tests::it_works ... ok
test flipper::tests::default_works ... ok

test result: ok. 2 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out; finished in 0.00s

   Doc-tests flipper

running 0 tests

test result: ok. 0 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out; finished in 0.00s

└  Unit testing complete
```

Okay, so you contract builds, your contract passes the tests. We can now deploy the contract.

## Deploy your contract locally

In order to deploy an ink! smart contract locally we need to spin up a local instance of [Pop Network](https://github.com/r0gue-io/pop-node).

Pop Network is a parachain meaning that it runs on the Polkadot Relay chain.

> To take a deep dive into how Parachains and the Relay chain work, go [here](https://docs.substrate.io/tutorials/build-a-parachain).

We will use Pop CLI to spin up a live network locally on our machine.

In a separate directory, let's create the configuration file for our local test network.

./network.toml

```toml
[relaychain]
chain = "rococo-local"

[[relaychain.nodes]]
name = "alice"
validator = true

[[relaychain.nodes]]
name = "bob"
validator = true

[[parachains]]
id = 1000
chain = "asset-hub-rococo-local"

[[parachains.collators]]
name = "asset-hub"

[[parachains]]
id = 9090
default_command = "pop-node"

[[parachains.collators]]
name = "pop"
```

We can now use the configuration file to spin up the network.

```
pop up parachain -f ./network.toml -p https://github.com/r0gue-io/pop-node

┌   Pop CLI : Deploy a parachain
│
▲  The following missing binaries are required: polkadot-parachain-v1.9.0
│  
◆  Would you like to source them automatically now?
│  ● Yes  / ○ No 
│
⚙  They will be cached at /Users/bruno/Library/Caches/pop
│  
◐  Sourcing polkadot-parachain-v1.9.0...                                                                                                                      

```

> The first time you run this command it will take some time. Grab some coffee.

Once complete, you will get some output:

```
....
◇  Sourcing complete.
│
◇  🚀 Network launched successfully - ctrl-c to terminate
│  ⛓️ rococo-local
│       alice:
│         portal: https://polkadot.js.org/apps/?rpc=ws://127.0.0.1:56533#/explorer
│         logs: tail -f /var/folders/vl/txnq6gdj22s9rn296z0md27w0000gn/T/zombie-48a3549c-5ab9-43de-93f5-9057211f3846/alice/alice.log
│       bob:
│         portal: https://polkadot.js.org/apps/?rpc=ws://127.0.0.1:56537#/explorer
│         logs: tail -f /var/folders/vl/txnq6gdj22s9rn296z0md27w0000gn/T/zombie-48a3549c-5ab9-43de-93f5-9057211f3846/bob/bob.log
│  ⛓️ local_testnet: 9090
│       pop:
│         portal: https://polkadot.js.org/apps/?rpc=ws://127.0.0.1:56545#/explorer
│         logs: tail -f /var/folders/vl/txnq6gdj22s9rn296z0md27w0000gn/T/zombie-48a3549c-5ab9-43de-93f5-9057211f3846/pop/pop.log
│  ⛓️ asset-hub-rococo-local: 1000
│       asset-hub:
│         portal: https://polkadot.js.org/apps/?rpc=ws://127.0.0.1:56541#/explorer
│         logs: tail -f /var/folders/vl/txnq6gdj22s9rn296z0md27w0000gn/T/zombie-48a3549c-5ab9-43de-93f5-9057211f3846/asset-hub/asset-hub.log
```

You now have the following running locally on your machine:

> **rococo-local**
>
> * this is the Polkadot "Test" Relay chain
>   * two validator nodes (alice & bob) to run the rococo-local Relay chain
>
> **local\_testnet**
>
> * this is the Pop Network parachain
>   * one collator node is running for the Pop Network parachain
>
> **asset-hub-rococo-local**
>
> * this is the Asset Hub system parachain that manages assets in Polkadot
>   * one collator node is running for this system chain
>   * it is useful to have a second parachain like this one to test cross-chain capabilities

Confirm that the Pop Network parachain is producing blocks by openning the PolkadotJS link in your browser:

* https://polkadot.js.org/apps/?rpc=ws://127.0.0.1:56545#/explorer

> Replace the port number with the port number for the Pop Network parachain that is outputted in your terminal

![](https://hackmd.io/\_uploads/S1yVLETkC.png)

Cool. Now that we have Pop Network running we can now deploy our contract.

Keep the PolkadotJs App window open, specifically to see the recent events. After we deploy the contract, we will see the events displayed there.

You will need to know the RPC URL for Pop Network which was outputted in the terminal when you ran the `pop up` command.

```
pop up contract -p ./flipper --constructor new --args "false" --suri //Alice --url ws://127.0.0.1:56545

┌   Pop CLI : Deploy a smart contract
│
◐  Doing a dry run to estimate the gas...                                                                                                    ●  Gas limit Weight { ref_time: 264725731, proof_size: 16689 }
│  
◇  Contract deployed and instantiated: The Contract Address is "5CLPm1CeUvJhZ8GCDZCR7nWZ2m3XXe4X5MtAQK69zEjut36A"
│
└  Deployment complete
```
> **⚠️ Note:** If you're using ink! v6, the contract address displayed here will appear as a 20-byte hash.

The contract has been deployed!

> Save the contract address as we will be used in the next step to interact with the contract.

You can also confirm that the contract was deployed in the recent events in Polkadot JS: ![Screenshot 2024-04-05 at 5.33.19 PM](https://hackmd.io/\_uploads/rygPDVpJ0.png)

## Interacting with our contract

Now that the contract is live, we can start interacting with it.

Grab the contract address that was outputted from the previous step.

> If you lost the address, you can always pull up PolkadotJs Apps and check the chain state for the contract address.

```
pop call contract -p ./flipper --contract 5CLPm1CeUvJhZ8GCDZCR7nWZ2m3XXe4X5MtAQK69zEjut36A --message get --suri //Alice --url ws://127.0.0.1:56545

┌   Pop CLI : Calling a contract
│
◐  Calling the contract...                                                                                                                   ⚙  Result: Ok(false)
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
pop call contract -p ./flipper --contract 5CLPm1CeUvJhZ8GCDZCR7nWZ2m3XXe4X5MtAQK69zEjut36A --message flip --suri //Alice --url ws://127.0.0.1:56545 -x

┌   Pop CLI : Calling a contract
│
◐  Doing a dry run to estimate the gas...                                                                                                    ⚙  Gas limit Weight { ref_time: 382198493, proof_size: 18636 }
│  
◓  Calling the contract...                                                                                                                   ⚙        Events
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
pop call contract -p ./flipper --contract 5CLPm1CeUvJhZ8GCDZCR7nWZ2m3XXe4X5MtAQK69zEjut36A --message get --suri //Alice --url ws://127.0.0.1:56545

┌   Pop CLI : Calling a contract
│
◐  Calling the contract...                                                                                                                   ⚙  Result: Ok(true)
│  
▲  Your call has not been executed.
│  
▲  To submit the transaction and execute the call on chain, add -x/--execute flag to the command.
│  
└  Call completed successfully!
```

The `value` is now `true`.

Awesome we now know how to make calls to our smart contract.

## Running end-to-end tests of your contract

Make sure you are in the flipper directory:

```
cd flipper
```

If you look at the flipper ink! smart contract, you will notice it has end-to-end tests at the _end_ of the `lib.rs` file.

The ink! e2e tests runs against a [substrate-contracts-node](https://github.com/paritytech/substrate-contracts-node).

Download and save the [substrate-contracts-node](https://github.com/paritytech/substrate-contracts-node).

Run the e2e tests:

```
export CONTRACTS_NODE="/Users/bruno/src/substrate-contracts-node/target/debug/substrate-contracts-node"
pop test contract --features e2e-tests

┌   Pop CLI : Starting end-to-end tests
│
    Finished test [unoptimized + debuginfo] target(s) in 0.83s
     Running unittests lib.rs (target/debug/deps/flipper-508d8cf8af185e86)

running 5 tests
test flipper::e2e_tests::e2e_test_deployed_contract ... ignored
test flipper::tests::it_works ... ok
test flipper::tests::default_works ... ok
   Compiling toml_datetime v0.6.5
   Compiling toml_edit v0.20.2
   ....
   Finished release [optimized] target(s) in 9.03s
   Running `target/ink/release/metadata-gen`
test flipper::e2e_tests::default_works ... ok
test flipper::e2e_tests::it_works ... ok

test result: ok. 4 passed; 0 failed; 1 ignored; 0 measured; 0 filtered out; finished in 47.56s

   Doc-tests flipper

running 0 tests

test result: ok. 0 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out; finished in 0.00s

└  End-to-end testing complete
```

Passed!

## Deploy on the Pop Network TestNet

Once you have battle-tested your smart contract using the armor of unit and e2e tests, you are ready to test your smart contract on a live network: the Pop Network TestNet.

Let's deploy.

The test network for Polkadot is [Paseo](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Fpaseo.rpc.amforc.com).

You can find Pop Network running on Paseo [here](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Frpc2.paseo.popnetwork.xyz).

Use the [Paseo faucet](https://faucet.polkadot.io/paseo) to fund Alice's Paseo account with some PAS tokens. PAS tokens are the equivalent of DOT on Polkadot.

Alice's account is: `5GrwvaEF5zXb26Fz9rcQpDWS57CtERHpNehXCPcNoHGKutQY`

Go to the [Paseo faucet](https://faucet.polkadot.io/paseo) and request some PAS tokens for Alice.

Since Pop Network uses the Relay chain's native token as its native token, we will need to get the PAS tokens that is in your account on Paseo into Pop Network so we can deploy the contract.

We will need to add Alice to our [PolkadotJs Wallet extension](https://polkadot.js.org/extension). We can do that by using Alice's secret seed: `0xe5be9a5092b81bca64be81d212e7f2f9eba183bb7a90954f7b76361f6edb5c0a`

![](https://hackmd.io/\_uploads/HymzV9GeA.png)

> Remember this is for development purposes. In production you would already have an existing wallet to use.

Now that we have Alice's account in our PolkadotJs Wallet Extension, we can use the following for teleporting PAS tokens to Pop Network:

* https://onboard.popnetwork.xyz

![](https://hackmd.io/\_uploads/HysR4czxA.png)

Cool! Now that you have some PAS tokens on Pop Network, we can deploy.

```
pop up contract --constructor new --args "false" --suri 0xe5be9a5092b81bca64be81d212e7f2f9eba183bb7a90954f7b76361f6edb5c0a --url wss://rpc1.paseo.popnetwork.xyz
```

```
┌   Pop CLI : Deploy a smart contract
│
◐  Doing a dry run to estimate the gas...                                                                                                    ●  Gas limit Weight { ref_time: 266641786, proof_size: 16689 }
│  
◇  Contract deployed and instantiated: The Contract Address is "5GE1BaqUsbh4ty1c6Ko1kkAp2AEZQCDhpvtKpJJ1Q3ex1xzC"
│
└  Deployment complete
```

You can now take the contract address and check the chain state to confirm that the contract exists

![](https://hackmd.io/\_uploads/H1j56cMl0.png)

Let's add the contract to our Contracts UI in PolkadotJS Apps. Click on "add an existing contract".

![](https://hackmd.io/\_uploads/HJ3h1sGg0.png)

Add the contract address along with the `flipper.contract` file found inside `flipper/target/ink/flipper.contract`

![Screenshot 2024-04-09 at 8.01.17 PM](https://hackmd.io/\_uploads/Bys-lozl0.png) Save. You can now see your newly uploaded contract.

![](https://hackmd.io/\_uploads/Bkh3lsfxA.png)

Done!

## Resources

* https://use.ink

## Feedback

> [Take a minute to give us feedback on this tutorial so we can improve it!](https://forms.gle/tABmLwtqYPUHArRQ9)
