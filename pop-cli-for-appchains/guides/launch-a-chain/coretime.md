---
description: Acquire coretime for a chain on a relay network.
---

# Acquire coretime

Your chain needs [coretime](https://wiki.polkadot.network/docs/learn-agile-coretime) to have its blocks validated and finalized by the relay chain.

## Purchase on-demand coretime

```bash
pop call chain --url <relay_endpoint>
```

Use the on-demand coretime flow and provide:

- `max_amount`: the maximum spot price you will pay for a core.
- `para_id`: your parachain ID.

When the transaction succeeds, the relay chain emits an `OnDemandOrderPlaced` event.

> The `max_amount` you need depends on current relay chain pricing.

> Using a private key URI is fine for development accounts only. For production or higher security, use `--use-wallet` and follow [Securely sign transactions from CLI](../securely-sign-transactions-from-cli.md).

## Resources

#### Learning Resources

* [https://paritytech.github.io/devops-guide/guides/parachain_deployment.html](https://paritytech.github.io/devops-guide/guides/parachain_deployment.html)
* 🧑‍🏫 To learn about Polkadot in general, [Polkadot.network](https://polkadot.network/) website is a good starting point.
  * ⭕ Learn more about Polkadot chains [here](https://wiki.polkadot.network/docs/learn-parachains).
* 🧑‍🔧 For technical introduction, [here](https://github.com/paritytech/polkadot-sdk#-documentation) are the Polkadot SDK documentation resources.

**Need help?**

Ask on [Polkadot Stack Exchange](https://polkadot.stackexchange.com/) (tag it [`pop`](https://substrate.stackexchange.com/tags/pop/info)) or drop by [our Telegram](https://t.me/onpopio). We're here to help!
