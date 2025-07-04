---
description: The following guide shows how to acquire coretime.
---

# Coretime

In order to get a chain's block validated and finalised by the Relay chain it needs to acquire [coretime](https://wiki.polkadot.network/docs/learn-agile-coretime).

Acquire coretime using the following command:

```bash
pop call chain --url <relay_endpoint>
```
```bash
┌   Pop CLI : Call a chain
│
◇  What would you like to do?
│  Purchase on-demand coretime 
│
◇  Enter the value for the parameter: max_amount
│  10000000
│
◇  Enter the value for the parameter: para_id
│  2000
│
◇  Do you want to use your browser wallet to sign the extrinsic? (Selecting 'No' will prompt you to manually enter the secret key URI for signing, e.g., '//Alice')
│  No
│
◇  Signer of the extrinsic:
│  <CHAIN MANAGER ACCOUNT>
...
       Event OnDemand ➜ OnDemandOrderPlaced
         para_id: Id(2000)
         spot_price: 1mUNIT
         ordered_by: <CHAIN MANAGER ACCOUNT>
...
```

> Note: the `max_amount` (spot price willing to pay for a core) will vary depending on the Relay Network.

If the event `OnDemandOrderPlaced` is returned it means that your block will be validated and finalised!

> Note:
In the example above, you are prompted to provide a `<private-key>` to interact with the chain. However, this implies a potentially insecure way of handling private keys and should only be used for development accounts.
For production accounts and enhanced security, Pop CLI offers the `--use-wallet` option to securely sign transactions. Refer to the [Securely sign transactions from CLI guide](../securely-sign-transactions-from-cli.md) for detailed instructions.


## Resources

#### Learning Resources

* [https://paritytech.github.io/devops-guide/guides/parachain\_deployment.html](https://paritytech.github.io/devops-guide/guides/parachain\_deployment.html)
* 🧑‍🏫 To learn about Polkadot in general, [Polkadot.network](https://polkadot.network/) website is a good starting point.
    * ⭕ Learn more about parachains [here](https://wiki.polkadot.network/docs/learn-parachains).
* 🧑‍🔧 For technical introduction, [here](https://github.com/paritytech/polkadot-sdk#-documentation) are the Polkadot SDK documentation resources.

**Technical Support**

* [Polkadot Stack Exchange](https://polkadot.stackexchange.com/)
    * Create a question and tag it with "[`pop`](https://substrate.stackexchange.com/tags/pop/info)"
    * Share the StackExchange question in our [Pop Support Telegram channel](https://t.me/pop\_support)
