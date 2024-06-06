# Deploying your contract locally

In order to run your contract, we will need a running Substrate node that has [pallet-contracts](https://paritytech.github.io/polkadot-sdk/master/pallet\_contracts/index.html) running.

We can use [substrate-contracts-node](https://github.com/paritytech/substrate-contracts-node) - a Substrate blockchain with pallet contracts installed.

Let's spin up the contracts-node.

```bash
pop up contracts-node

┌   Pop CLI : Launch a contracts node
│
    Updating crates.io index
  Downloaded contracts-node v0.41.0
  Downloaded 1 crate (92.9 KB) in 0.15s
  Installing contracts-node v0.41.0
  ....
  Finished `release` profile [optimized] target(s) in 16m 02s
  Installing /Users/bruno/Library/Caches/pop/bin/substrate-contracts-node
  Installed package `contracts-node v0.41.0` (executable `substrate-contracts-node`)
warning: be sure to add `/Users/bruno/Library/Caches/pop/bin` to your PATH to be able to run the installed binaries
2024-06-05 22:36:05.908  INFO main sc_cli::runner: Substrate Contracts Node    
2024-06-05 22:36:05.908  INFO main sc_cli::runner: ✌️  version 0.41.0-unknown    
2024-06-05 22:36:05.908  INFO main sc_cli::runner: ❤️  by anonymous, 2021-2024    
2024-06-05 22:36:05.908  INFO main sc_cli::runner: 📋 Chain specification: Development    
2024-06-05 22:36:05.908  INFO main sc_cli::runner: 🏷  Node name: comfortable-sort-2315    
2024-06-05 22:36:05.908  INFO main sc_cli::runner: 👤 Role: AUTHORITY    
2024-06-05 22:36:05.908  INFO main sc_cli::runner: 💾 Database: RocksDb at /var/folders/vl/txnq6gdj22s9rn296z0md27w0000gn/T/substratefIj1lH/chains/dev/db/full    
2024-06-05 22:36:06.715  INFO main sc_rpc_server: Running JSON-RPC server: addr=127.0.0.1:9944, allowed origins=["*"]    
```

The contracts-node is now running on your machine.

Let's make sure by opening your browser and connecting to the contracts-node.

```
https://polkadot.js.org/apps/?rpc=ws://127.0.0.1:9944#/explorer
```

Make sure your contract builds.

```sh
pop build contract
```

Now let's deploy the contract to the contracts-node.

```sh
pop up contract --constructor new --args "false" --suri //Alice

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
