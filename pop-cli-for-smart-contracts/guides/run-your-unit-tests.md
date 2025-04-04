# Run your unit tests

> **⚠️ Note:** This guide supports ink! v5 by default. For experimenting with ink! v6, please refer to our [migration guide](./migrating-to-inkv6.md).

To run your ink! smart contract's unit tests, make sure you're in the directory of your contract and run the following command:

```
pop test contract
```

You should get output like the following:

```
┌   Pop CLI : Building a contract
│
 [==] Checking clippy linting rules
   Compiling proc-macro2 v1.0.79
   ....
   Compiling flipper v0.1.0 (/Users/pop/src/pop-cli/flipper)
    Finished test [unoptimized + debuginfo] target(s) in 8.06s
     Running unittests lib.rs (target/debug/deps/flipper-ff1505839f6873de)

running 2 tests
test flipper::tests::it_works ... ok
test flipper::tests::default_works ... ok

test result: ok. 2 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out; finished in 0.00s

   Doc-tests flipper

running 0 tests

test result: ok. 0 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out; finished in 0.00s

└  Unit testing complete
```

**Further Reading Material**

* [https://use.ink/basics/contract-testing/off-chain#unit-tests](https://use.ink/basics/contract-testing/off-chain#unit-tests)

**Technical Support**

* [Polkadot Stack Exchange](https://polkadot.stackexchange.com/)
  * Create a question and tag it with "[`pop`](https://substrate.stackexchange.com/tags/pop/info)"
  * Share the StackExchange question in our [Pop Support Telegram channel](https://t.me/pop\_support)
