---
description: >-
  In this tutorial developers will learn how to leverage Pop API to write a
  smart contract to mint an NFT.
---

# Minting an NFT

## Audience

Developers who want to learn how to:

* write a smart contract to mint an NFT

## Learning Objectives

On completion of this tutorial, developers will:

* have knowledge of how to use Pop API in ink! smart contracts
* create an NFT collection
* mint an NFT

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
```

