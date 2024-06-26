# Deploying your contract locally

Make sure your contract builds.

```sh
pop build contract
```

For all available options run: `pop build contract --help`

Now let's deploy the contract locally.

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
