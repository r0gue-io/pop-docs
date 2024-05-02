---
description: >-
  In this tutorial developers will learn how to leverage Pop API to write a
  smart contract to create an NFT collection.
---

# Creating an NFT Collection

## Audience

Developers who want to learn how to:

* write a smart contract to create an NFT collection
* interact with the Pop API

## Learning Objectives

On completion of this tutorial, developers will:

* have knowledge of how to use Pop API in ink! smart contracts
* create an NFT collection
* check that the collection exists

## Prerequisites

Developers should have:

* completed [your first contract](your-first-contract.md) tutorial
* [installed Pop CLI](https://app.gitbook.com/s/CzgRNwlfNkPLJDU555lw/welcome/installing-pop-cli)

## Getting Started

Let's start by creating a new ink! smart contract.

```
pop new contract nfts

┌   Pop CLI : Generating new contract "nfts"!
│
◇  Smart contract created!
│
└  cd into "/Users/bruno/src/nfts" and enjoy hacking! 🚀
```

Let's make sure it builds.

```
cd nfts
pop build contract

┌   Pop CLI : Building a contract
│
 [==] Checking clippy linting rules
   Compiling proc-macro2 v1.0.81
   Compiling unicode-ident v1.0.12
   Compiling syn v1.0.109
   Compiling equivalent v1.0.1
   ....
   Compiling ink v5.0.0
   Compiling nfts v0.1.0 (/private/var/folders/vl/txnq6gdj22s9rn296z0md27w0000gn/T/cargo-contract_To9PFi)
   Compiling metadata-gen v0.1.0 (/private/var/folders/vl/txnq6gdj22s9rn296z0md27w0000gn/T/cargo-contract_To9PFi/.ink/metadata_gen)
    Finished release [optimized] target(s) in 24.87s
     Running `target/ink/release/metadata-gen`
 [==] Generating bundle
└  Build completed successfully!
◆  
│  Original wasm size: 35.5K, Optimized: 10.4K
│  
│  The contract was built in DEBUG mode.
│  
│  Your contract artifacts are ready. You can find them in:
│  /Users/bruno/src/my_nfts/target/ink
│  
│    - nfts.contract (code + metadata)
│    - nfts.wasm (the contract's code)
│    - nfts.json (the contract's metadata)
◆ 
```

Now we want to wipe out the `lib.rs` file and replace with our own contents.

```rust
#![cfg_attr(not(feature = "std"), no_std, no_main)]

use pop_api::nfts::*;

#[derive(Debug, Copy, Clone, PartialEq, Eq, scale::Encode, scale::Decode)]
#[cfg_attr(feature = "std", derive(scale_info::TypeInfo))]
pub enum ContractError {
    InvalidCollection,
    NftsError(Error),
}

impl From<Error> for ContractError {
    fn from(value: Error) -> Self {
        ContractError::NftsError(value)
    }
}

#[ink::contract]
mod nfts {
    use super::*;

    #[ink(storage)]
    #[derive(Default)]
    pub struct NFTs;

    impl NFTs {
        #[ink(constructor, payable)]
        pub fn new() -> Self {
            ink::env::debug_println!("NFTs::new");
            Default::default()
        }

        #[ink(message)]
        pub fn create_nft_collection(&mut self) -> Result<(), ContractError> {
            ink::env::debug_println!("NFTs::create_nft_collection: collection creation started.");
            let admin = Self::env().caller();
            let item_settings = ItemSettings(BitFlags::from(ItemSetting::Transferable));

            let mint_settings = MintSettings {
                mint_type: MintType::Issuer,
                price: Some(0),
                start_block: Some(0),
                end_block: Some(0),
                default_item_settings: item_settings,
            };

            let config = CollectionConfig {
                settings: CollectionSettings(BitFlags::from(CollectionSetting::TransferableItems)),
                max_supply: None,
                mint_settings,
            };
            pop_api::nfts::create(admin, config)?;
            ink::env::debug_println!(
                "NFTs::create_nft_collection: collection created successfully."
            );
            Ok(())
        }

        #[ink(message)]
        pub fn read_collection(&self, collection_id: u32) -> Result<(), ContractError> {
            ink::env::debug_println!("NFTs::read_collection: collection_id: {:?}", collection_id);
            let collection = pop_api::nfts::collection(collection_id)?
                .ok_or(ContractError::InvalidCollection)?;
            ink::env::debug_println!("NFTs::read_collection: collection: {:?}", collection);
            Ok(())
        }
    }
}
```

The code above uses Pop API so let's make some edits to our `Cargo.toml` file.

Add the `pop-api` dependency as well as the `scale` dependencies.

```diff
[dependencies]
ink = { version = "5.0.0", default-features = false }
+ pop-api = { git = "https://github.com/r0gue-io/pop-node.git", default-features = false }
+ scale = { package = "parity-scale-codec", version = "3", default-features = false, features = ["derive"] }
+ scale-info = { version = "2.6", default-features = false, features = ["derive"], optional = true }
```

Make sure to add the `std` feature for the dependencies.

```diff
[features]
default = ["std"]
std = [
    "ink/std",
+    "pop-api/std",
+    "scale/std",
+    "scale-info/std",
]
```

Now build.

```
pop build contract
```

> Full Code: [https://github.com/r0gue-io/pop-api-examples/tree/main/nfts](https://github.com/r0gue-io/pop-api-examples/tree/main/nfts)

### Using Pop API

So to recap, we are using Pop API, specifically the NFT portion of the API.

```rust
use pop_api::nfts::*;
```

### Creating an NFT collection

We have a function to create the NFT collection.

```rust
#[ink(message)]
pub fn create_nft_collection(&mut self) -> Result<(), ContractError> {
    ink::env::debug_println!("NFTs::create_nft_collection: collection creation started.");
    let admin = Self::env().caller();
    let item_settings = ItemSettings(BitFlags::from(ItemSetting::Transferable));

    let mint_settings = MintSettings {
        mint_type: MintType::Issuer,
        price: Some(0),
        start_block: Some(0),
        end_block: Some(0),
        default_item_settings: item_settings,
    };

    let config = CollectionConfig {
        settings: CollectionSettings(BitFlags::from(CollectionSetting::TransferableItems)),
        max_supply: None,
        mint_settings,
    };
    pop_api::nfts::create(admin, config)?;
    ink::env::debug_println!(
        "NFTs::create_nft_collection: collection created successfully."
    );
    Ok(())
}
```

This code will setup the configuration for creating an NFT collection and call `pop_api::nfts::create(admin, config)` passing in the `config` along with the `admin` of the NFT collection and create the NFT collection.

### Reading an NFT collection

There is also a function to read a specific NFT collection.

```rust
#[ink(message)]
pub fn read_collection(&self, collection_id: u32) -> Result<(), ContractError> {
    ink::env::debug_println!("NFTs::read_collection: collection_id: {:?}", collection_id);
    let collection = pop_api::nfts::collection(collection_id)?
        .ok_or(ContractError::InvalidCollection)?;
    ink::env::debug_println!("NFTs::read_collection: collection: {:?}", collection);
    Ok(())
}
```

This function is more of an assertion returning `Ok(())` if the collection exists.

### Testing the contract

Notice how there are `debug_println!` macros throughout the code. This will be handy for testing the contract.

To test the contract we will spin up a local Pop network, deploy our contract and call it.

Let's spin up our network. In order to do so we will need to define a `network.toml` file.

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
id = 9090
default_command = "pop-node"

[[parachains.collators]]
name = "pop"
args = ["-lruntime::contracts=debug"]
```

This `network.toml` file will spin up a Relay chain (`rococo-local`) with two validators (`alice` and `bob`) as will as the ink! smart contracts parachain (Pop Network) with one collator to run it.

So now let's use the `network.toml` configuration.

```
pop up parachain -f network.toml -p https://github.com/r0gue-io/pop-node
```

> This command may take some time if it requires you to download the Polkadot binaries

Once the `pop up` command is successful, it will spin up a network.

```
┌   Pop CLI : Deploy a parachain
│
◇  🚀 Network launched successfully - ctrl-c to terminate
│  ⛓️ rococo-local
│       alice:
│         portal: https://polkadot.js.org/apps/?rpc=ws://127.0.0.1:57256#/explorer
│         logs: tail -f /var/folders/vl/txnq6gdj22s9rn296z0md27w0000gn/T/zombie-58cc73fb-e985-4415-8f3d-b60c1c34ad82/alice/alice.log
│       bob:
│         portal: https://polkadot.js.org/apps/?rpc=ws://127.0.0.1:57260#/explorer
│         logs: tail -f /var/folders/vl/txnq6gdj22s9rn296z0md27w0000gn/T/zombie-58cc73fb-e985-4415-8f3d-b60c1c34ad82/bob/bob.log
│  ⛓️ local_testnet: 9090
│       pop:
│         portal: https://polkadot.js.org/apps/?rpc=ws://127.0.0.1:57264#/explorer
│         logs: tail -f /var/folders/vl/txnq6gdj22s9rn296z0md27w0000gn/T/zombie-58cc73fb-e985-4415-8f3d-b60c1c34ad82/pop/pop.log
│
```

Open the PolkadotJS Apps explorer for Pop Network in your browser.

```
open https://polkadot.js.org/apps/?rpc=ws://127.0.0.1:57264#/explorer
```

> Keep the port number handy (57264). You will need it again soon.

Open a separate terminal for watching the Pop logs.

```
tail -f /var/folders/vl/txnq6gdj22s9rn296z0md27w0000gn/T/zombie-58cc73fb-e985-4415-8f3d-b60c1c34ad82/pop/pop.log
```

This is where your `debug_println!` statements will appear.

But first we need to deploy the contract only then we can call it.

Now that we have Pop Network up and running, we can deploy our smart contract.

Open a separate terminal and run the following command.

> Make sure the replace with your Pop Network port number.

```
pop up contract --suri //Alice --url ws://127.0.0.1:57264 --gas 2000000000 --proof-size 1000000

┌   Pop CLI : Deploy a smart contract
│
◇  Contract deployed and instantiated: The Contract Address is "5E13Ah9rPZNHAwpwiJLd1EQ6eVsZEmT31Cxx8oDpgTKLR21h"
│
└  Deployment complete
```

Keep the Contract Address handy, you will need it soon.

You will notice PolkadotJS Apps shows that the contract has been deployed.

<figure><img src="../.gitbook/assets/Screenshot 2024-05-02 at 6.31.39 PM.png" alt=""><figcaption><p><a href="https://polkadot.js.org/apps/?rpc=ws://127.0.0.1:57264">https://polkadot.js.org/apps/?rpc=ws://127.0.0.1:57264</a></p></figcaption></figure>

Let's now go to the Contracts section in PolkadotJS under the Developer tab.

<figure><img src="../.gitbook/assets/Screenshot 2024-05-02 at 6.29.18 PM.png" alt=""><figcaption><p><a href="https://polkadot.js.org/apps/?rpc=ws://127.0.0.1:57264#/contracts">https://polkadot.js.org/apps/?rpc=ws://127.0.0.1:57264#/contracts</a></p></figcaption></figure>

The contract is already instantiated but let's also make sure it shows up in the UI.

Click "add an existing contract" and type in the contract information.

<figure><img src="../.gitbook/assets/Screenshot 2024-05-02 at 6.38.43 PM.png" alt=""><figcaption><p>add an existing contract</p></figcaption></figure>

Save and you should now see your NFTs contract in the UI.

<figure><img src="../.gitbook/assets/Screenshot 2024-05-02 at 6.40.00 PM.png" alt=""><figcaption><p>NFTs ink! smart contract</p></figcaption></figure>

This is useful if you want to use the UI to execute or read messages. However you can also use Pop CLI to do the same.



### Calling your NFTs ink! smart contract

Let's call our NFTs smart contract and create an NFT collection.

In order to call our contract we will need to fund the contract address with some tokens so that it has enough to execute the transactions.

Transfer some tokens from `alice` to the contract address.

<figure><img src="../.gitbook/assets/Screenshot 2024-05-02 at 6.54.11 PM.png" alt=""><figcaption><p>PolkadotJS Apps: Transfer</p></figcaption></figure>

<figure><img src="../.gitbook/assets/Screenshot 2024-05-02 at 6.55.24 PM.png" alt=""><figcaption></figcaption></figure>

Awesome the contract is now funded!

<figure><img src="../.gitbook/assets/Screenshot 2024-05-02 at 6.56.21 PM.png" alt=""><figcaption><p>NFTs contract</p></figcaption></figure>

Let's call the contract!

```
pop call contract --contract 5E13Ah9rPZNHAwpwiJLd1EQ6eVsZEmT31Cxx8oDpgTKLR21h --message create_nft_collection --suri //Alice --url ws://127.0.0.1:57264 --gas 200000000000 --proof-size 1000000 --execute

┌   Pop CLI : Calling a contract
│
◐  Calling the contract...                                                                                                                                                                ⚙        Events
│         Event Balances ➜ Withdraw
│           who: 5GrwvaEF5zXb26Fz9rcQpDWS57CtERHpNehXCPcNoHGKutQY
│           amount: 31.604858μUNIT
│         Event Balances ➜ Reserved
│           who: 5E13Ah9rPZNHAwpwiJLd1EQ6eVsZEmT31Cxx8oDpgTKLR21h
│           amount: 100mUNIT
│         Event Nfts ➜ Created
│           collection: 0
│           creator: 5E13Ah9rPZNHAwpwiJLd1EQ6eVsZEmT31Cxx8oDpgTKLR21h
│           owner: 5GrwvaEF5zXb26Fz9rcQpDWS57CtERHpNehXCPcNoHGKutQY
│         Event Nfts ➜ NextCollectionIdIncremented
│           next_id: Some(1)
│         Event Contracts ➜ Called
│           caller: Signed(5GrwvaEF5zXb26Fz9rcQpDWS57CtERHpNehXCPcNoHGKutQY)
│           contract: 5E13Ah9rPZNHAwpwiJLd1EQ6eVsZEmT31Cxx8oDpgTKLR21h
│         Event Balances ➜ Deposit
│           who: 5GrwvaEF5zXb26Fz9rcQpDWS57CtERHpNehXCPcNoHGKutQY
│           amount: 15.744445μUNIT
│         Event TransactionPayment ➜ TransactionFeePaid
│           who: 5GrwvaEF5zXb26Fz9rcQpDWS57CtERHpNehXCPcNoHGKutQY
│           actual_fee: 15.860413μUNIT
│           tip: 0UNIT
│         Event System ➜ ExtrinsicSuccess
│           dispatch_info: DispatchInfo { weight: Weight { ref_time: 3268630749, proof_size: 34072 }, class: Normal, pays_fee: Yes }
│  
└  Call completed successfully!
```

Wow! You can see the event trail that the contract left, including Pop Network's underlying NFTs pallet firing off an event saying that the collection got created.

But let's confirm.

Our NFTs contract has a `read_collection` function. Let's test it.

```
pop call contract --contract 5E13Ah9rPZNHAwpwiJLd1EQ6eVsZEmT31Cxx8oDpgTKLR21h --message read_collection --args 0 --suri //Alice --url ws://127.0.0.1:57264

┌   Pop CLI : Calling a contract
│
◐  Calling the contract...
⚙  Result: Ok(Ok())
│  
▲  Your call has not been executed.
│  
▲  To submit the transaction and execute the call on chain, add -x/--execute flag to the command.
│  
└  Call completed successfully!
```

> Stuck? Run `pop call contract --help`

For reading storage we do not need to execute an extrinsic so no need for the `--execute` flag. Instead it will connect with the RPC node and read the storage which is better because it will not cost any gas. Notice the `--args` specifying collection with ID of 0. And the result came back as `Ok(Ok())` meaning an NFT collection with ID of 0 exists.



### Conclusion

In this tutorial we learned how to create an NFTs ink! smart contract using Pop API. We then deployed the contract and called two of its functions.



### Next Steps

Try to implement a `mint` function using Pop API. Then re-build the contract (`pop build contract`), deploy the new contract, and test it by calling the contract.

You can find the solution for minting an NFT [here](https://github.com/r0gue-io/pop-node/blob/bc4154028a22be2d8cf9b5a4aab897a184ceb230/pop-api/examples/nfts/lib.rs#L60-L96).

