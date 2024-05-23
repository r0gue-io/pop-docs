# Deploying your contract

In order to run your contract, we will need a running Substrate node that has [pallet-contracts](https://paritytech.github.io/polkadot-sdk/master/pallet\_contracts/index.html) running.

We can use [substrate-contracts-node](https://github.com/paritytech/substrate-contracts-node) for this.

```sh
cargo install contracts-node
```

> The contracts-node is a Substrate blockchain with pallet contracts installed

Now let's run the node. Type the following in your terminal.

```sh
substrate-contracts-node
```

Make sure the contracts-node is running.

```
https://polkadot.js.org/apps/?rpc=ws://127.0.0.1:9944#/explorer
```

Make sure your contract builds.

```sh
pop build contract
```

Now let's deploy to contract to the contracts-node.

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
