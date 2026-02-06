# Test

To learn how to test your ink! smart contract, from unit tests to e2e testing workflows, go checkout the [ink! testing documentation](https://use.ink/docs/v6/contract-testing/overview).

To run your ink! smart contract's unit tests, make sure you're in the directory of your contract and run the following command:

```bash
pop test
```

If you pass a single positional value and it isn't a directory, Pop treats it as a test filter and runs in the current directory:

```bash
pop test my_test_name
```

Pop checks for an `ink` dependency. If it finds one, it runs contract tests. If it doesn't, Pop runs `cargo test` for non-contract projects and then checks whether the project is a chain.

To run end-to-end (e2e) tests, for which you need a blockchain running:

```bash
 pop test --e2e
```

If you want to run you e2e tests on your own local chain specify the directory path using:

```
 pop test --e2e --node ../my-chain
```

**Need help?**

Ask on [Polkadot Stack Exchange](https://polkadot.stackexchange.com/) (tag it [`pop`](https://substrate.stackexchange.com/tags/pop/info)) or drop by [our Telegram](https://t.me/onpopio). We're here to help!
