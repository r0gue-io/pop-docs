# Test

> 🚀 **Note:** For experimenting with ink! v6, make sure to check [getting-started-with-inkv6.](../welcome/migrating-to-inkv6.md)

To learn how to test your ink! smart contract, from unit tests to e2e testing workflows, go checkout the [ink! testing documentation](https://use.ink/docs/v6/contract-testing/overview).

To run your ink! smart contract's unit tests, make sure you're in the directory of your contract and run the following command:

```
pop test contract
```

To run end-to-end (e2e) tests, for which you need a blockchain running:

```bash
 pop test contract --e2e
```

If you want to run you e2e tests on your own local chain specify the directory path using:

```
 pop test contract --e2e --node ../my-chain
```

**Need help?**

Ask on [Polkadot Stack Exchange](https://polkadot.stackexchange.com/) (tag it [`pop`](https://substrate.stackexchange.com/tags/pop/info)) or drop by [our Telegram](https://t.me/onpopio). We're here to help!
