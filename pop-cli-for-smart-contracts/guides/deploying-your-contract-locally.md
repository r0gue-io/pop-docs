# Deploying your contract locally

Make sure your contract builds.

```sh
pop build contract
```

For all available options run: `pop build contract --help`

When deploying a smart contract you need to know how much gas is needed so you can pay accordingly. More info on this topic can be found here:

* [https://use.ink/basics/gas](https://use.ink/basics/gas)

To find an estimate of how much gas you will need, you can do a "dry-run" of the contract:

```
pop up contract --constructor new --args "false" --suri //Alice --dry-run
```

This will perform a dry-run via an RPC call to estimate the gas usage. It does not submit a transaction.

```
pop up contract --constructor new --args "false" --suri //Alice --dry-run

....
Compiling flipper v0.1.0 (/private/var/folders/vl/txnq6gdj22s9rn296z0md27w0000gn/T/cargo-contract_Ebtb6E)
 Finished `release` profile [optimized] target(s) in 8.85s
 [==] Post processing code
 [==] Generating metadata
   Compiling metadata-gen v0.1.0 (/private/var/folders/vl/txnq6gdj22s9rn296z0md27w0000gn/T/cargo-contract_0xpg7p/.ink/metadata_gen)
    Finished `release` profile [optimized] target(s) in 4.29s
     Running `target/ink/release/metadata-gen`
 [==] Generating bundle
◆  
│  Original wasm size: 21.4K, Optimized: 1.7K
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
┌   Pop CLI : Deploy a smart contract
│
◐  Doing a dry run to estimate the gas...                                                              
●  Gas limit: Weight { ref_time: 144914687, proof_size: 16689 }
```

You can now see the estimate and make sure your account is properly funded with that amount.

We can now deploy the contract locally.

```
pop up contract --constructor new --args "false" --suri //Alice

◇  The chain "ws://localhost:9944/" is not live. Would you like pop to start a local node in the background for testing?
│  Yes 
│
2024-06-25 16:29:14.335  INFO main sc_cli::runner: Substrate Contracts Node    
2024-06-25 16:29:14.335  INFO main sc_cli::runner: ✌️  version 0.41.0-120504a771e    
2024-06-25 16:29:14.335  INFO main sc_cli::runner: ❤️  by anonymous, 2021-2024    
2024-06-25 16:29:14.335  INFO main sc_cli::runner: 📋 Chain specification: Development    
2024-06-25 16:29:14.335  INFO main sc_cli::runner: 🏷  Node name: dull-breakfast-7458    
2024-06-25 16:29:14.335  INFO main sc_cli::runner: 👤 Role: AUTHORITY    
2024-06-25 16:29:14.335  INFO main sc_cli::runner: 💾 Database: RocksDb at /var/folders/vl/txnq6gdj22s9rn296z0md27w0000gn/T/substrate9Sn7BB/chains/dev/db/full    
◆  Local node started successfully in the background.
│  
▲  NOTE: The contracts node is running in the background with process ID 26552. Please close it manually when done testing.
┌   Pop CLI : Deploy a smart contract
│
◐  Doing a dry run to estimate the gas...                                                                                                                                   ●  Gas limit Weight { ref_time: 140492887, proof_size: 16689 }
│  
◇  Contract deployed and instantiated: The Contract Address is "5EFaRZcQN3RcCRPWJchtgR7i2zgKBVK3Es26jFVtxcgS8Q9K"
│
└  Deployment complete
```

Your contract is now deployed! You can check in PolkadotJs Apps as well.

<figure><img src="../.gitbook/assets/Screenshot 2024-05-09 at 7.31.17 PM.png" alt=""><figcaption><p>recent events</p></figcaption></figure>
