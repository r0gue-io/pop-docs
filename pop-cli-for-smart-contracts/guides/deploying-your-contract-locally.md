# Deploying your contract locally

Make sure your contract builds.

```sh
cd flipper
pop build
```

For all available options run: `pop build --help`

{% hint style="info" %}
For Pop CLI versions <`0.3.0` the `pop build` command is `pop build contract`
{% endhint %}

When deploying a smart contract you need to know how much gas is needed so you can pay accordingly. More info on this topic can be found here:

* [https://use.ink/basics/gas](https://use.ink/basics/gas)

To find an estimate of how much gas you will need, you can do a "dry-run" of the contract:

```
pop up contract --constructor new --args "false" --suri //Alice --dry-run
```

This will perform a dry-run via an RPC call to estimate the gas usage. It does not submit a transaction.

```
┌   Pop CLI : Deploy a smart contract
│
◇  Gas limit estimate: Weight { ref_time: 146346224, proof_size: 16689 }
```

You can now see the estimate and make sure your account is properly funded with that amount.

We can now deploy the contract locally, meaning that we will upload and instantiate the contract.

> It is also possible to **only** upload the contract and **not** instantiate by adding `--upload-only`

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

Your contract is now deployed! You can check in [PolkadotJs Apps](https://polkadot.js.org/apps/) as well.

<figure><img src="../.gitbook/assets/Screenshot 2024-05-09 at 7.31.17 PM.png" alt=""><figcaption><p>recent events</p></figcaption></figure>
