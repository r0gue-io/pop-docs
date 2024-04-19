# Deploy on Pop Testnet

Once you have battle-tested your smart contract using unit and e2e tests, you are ready to test your smart contract on a live network: the Pop Network TestNet.

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

![Screenshot 2024-04-09 at 7.13.17 PM](https://hackmd.io/\_uploads/HysR4czxA.png)

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

### Resources

* https://use.ink

### Feedback

> [Take a minute to give us feedback on this tutorial so we can improve it!](https://forms.gle/tABmLwtqYPUHArRQ9)
