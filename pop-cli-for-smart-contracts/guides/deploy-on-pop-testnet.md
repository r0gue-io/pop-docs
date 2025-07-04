---
description: Learn how to deploy an ink! smart contract on Pop
---

# Deploy on Pop

Once you have battle-tested your smart contract using unit and e2e tests, you are ready to test your smart contract on the [Pop Testnet](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Frpc2.paseo.popnetwork.xyz).

Before you deploy, make sure you have bridged some tokens from the Polkadot Relay chain to your Pop account. To learn how to do this, go here:

* [Get tokens on Pop (Testnet)](./bridge-tokens-to-pop-network.md)

### Deploy

Now that your account is funded on Pop, you can deploy your contract.

Go to [contracts.onpop.io](https://contracts.onpop.io) and deploy your contract.

<figure><img src="../.gitbook/assets/Screenshot 2024-11-12 at 11.21.58 AM.png" alt=""><figcaption></figcaption></figure>

You can now interact with your contract.

<figure><img src="../.gitbook/assets/Screenshot 2024-11-12 at 11.47.56 AM.png" alt=""><figcaption></figcaption></figure>



## Use PolkadotJs Apps (Optional)

Another UI option to interact with your ink! smart contract is to use PolkadotJS Apps.

Once your contract is deployed, copy the contract address and check the [chain state](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Frpc2.paseo.popnetwork.xyz#/chainstate) to confirm that the contract exists.&#x20;

![](https://hackmd.io/\_uploads/H1j56cMl0.png)

You can add the contract to the [Contracts](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Frpc2.paseo.popnetwork.xyz#/contracts) UI in PolkadotJS Apps. Click on "+ Add an existing contract".

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
