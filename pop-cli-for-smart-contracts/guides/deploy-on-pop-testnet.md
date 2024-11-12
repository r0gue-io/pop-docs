---
description: Learn how to deploy an ink! smart contract to Pop
---

# Deploy on Pop

Once you have battle-tested your smart contract using unit and e2e tests, you are ready to test your smart contract on a live test network: the [Pop Testnet](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Frpc2.paseo.popnetwork.xyz).

Let's deploy.

### Onboarding

The community Relay test network for Polkadot is [Paseo](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Fpaseo.rpc.amforc.com).&#x20;

[Pop Testnet](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Frpc2.paseo.popnetwork.xyz) is connected to Paseo.

In order to deploy a smart contract on the Pop Testnet, we will need to fund your account with some native tokens. Remember, Pop Network uses DOT as its native token, which is the Relay network's native token. In this case, we are on Paseo, so the native token for the Paseo Relay network is PAS.&#x20;

We will need to:

1. Fund our account on Paseo with some PAS tokens
2. Teleport those PAS tokens from the Paseo account to the Pop Testnet account



**Fund your Paseo account**

Use the [Paseo faucet](https://faucet.polkadot.io/) to fund your Paseo account with some PAS tokens.&#x20;

PAS tokens are the equivalent of DOT on Polkadot.

**Go to the** [**Paseo faucet**](https://faucet.polkadot.io/) **and request some PAS tokens for your account.**



**Teleport from Paseo Relay Network to Pop Testnet**

As Pop Network uses the Relay chain's native token as its native token, we will need to teleport some of the PAS tokens from your account on Paseo to Pop Network so that we can then deploy the contract.

We can use [onboard.popnetwork.xyz](https://onboard.popnetwork.xyz) for teleporting PAS tokens to Pop Network. Make sure to authorize your wallet to connect to the site.

![Screenshot 2024-04-09 at 7.13.17 PM](https://hackmd.io/\_uploads/HysR4czxA.png)

Cool! Now that you have some PAS tokens on the [Pop Testnet](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Frpc2.paseo.popnetwork.xyz), we can deploy our smart contract.

### Deploy

Now that your account is funded on Pop, you can deploy your contract.

Go to [contracts.onpop.io](https://contracts.onpop.io) and deploy your contract.

<figure><img src="../.gitbook/assets/Screenshot 2024-11-12 at 11.21.58 AM.png" alt=""><figcaption></figcaption></figure>

You can now take the contract address and check the [chain state](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Frpc2.paseo.popnetwork.xyz#/chainstate) to confirm that the contract exists.&#x20;

![](https://hackmd.io/\_uploads/H1j56cMl0.png)

Let's add the contract to the [Contracts](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Frpc2.paseo.popnetwork.xyz#/contracts) UI in PolkadotJS Apps. Click on "+ Add an existing contract".

![](https://hackmd.io/\_uploads/HJ3h1sGg0.png)

Add the contract address along with the `flipper.contract` file found inside `flipper/target/ink/flipper.contract`

![Screenshot 2024-04-09 at 8.01.17 PM](https://hackmd.io/\_uploads/Bys-lozl0.png)

You can now see your newly uploaded contract:

![](https://hackmd.io/\_uploads/Bkh3lsfxA.png)

Done!



#### Resources

* [https://use.ink](https://use.ink)

**Technical Support**

* [Polkadot Stack Exchange](https://polkadot.stackexchange.com/)
  * Create a question and tag it with "[`pop`](https://substrate.stackexchange.com/tags/pop/info)"
  * Share the StackExchange question in our [Pop Support Telegram channel](https://t.me/pop\_support)
