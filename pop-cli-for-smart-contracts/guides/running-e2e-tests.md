# Running E2E tests

> **⚠️ Note:** This guide supports ink! v5 by default. For experimenting with ink! v6, please refer to our [migration guide](./getting-started-with-inkv6.md).

Make sure you are in your contract's directory:

```shell
cd flipper
```

If you look at the contract source, you will notice it has end-to-end tests at the _end_ of the `lib.rs` file.

The ink! e2e tests are run against [substrate-contracts-node](https://github.com/paritytech/substrate-contracts-node). Pop CLI will download the latest [release](https://github.com/paritytech/substrate-contracts-node/releases) binary for your platform.

Run the e2e tests:

```bash
 pop test contract --e2e
```

```
┌   Pop CLI : Starting end-to-end tests
│
▲  ⚠️ The substrate-contracts-node binary is not found.
│  
◇  📦 Would you like to source it automatically now?
│  Yes 
│
◇  ✅ substrate-contracts-node successfully sourced. Cached at: /Users/bruno/Library/Caches/pop/substrate-contracts-node-v0.41.0
│
    Finished `test` profile [unoptimized + debuginfo] target(s) in 0.98s
     Running unittests lib.rs (target/debug/deps/my_contract-3c35b5992ffebf0d)

running 5 tests
test flipper::e2e_tests::e2e_test_deployed_contract ... ignored
test flipper::tests::it_works ... ok
test flipper::tests::default_works ... ok
   Compiling toml_datetime v0.6.5
   Compiling toml_edit v0.20.2
   ....
   Finished release [optimized] target(s) in 9.03s
   Running `target/ink/release/metadata-gen`
test flipper::e2e_tests::default_works ... ok
test flipper::e2e_tests::it_works ... ok

test result: ok. 4 passed; 0 failed; 1 ignored; 0 measured; 0 filtered out; finished in 47.56s

   Doc-tests flipper

running 0 tests

test result: ok. 0 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out; finished in 0.00s

└  End-to-end testing complete
```

Passed!

**Further Reading Material**

* [https://use.ink/basics/contract-testing/end-to-end-e2e-testing](https://use.ink/basics/contract-testing/end-to-end-e2e-testing)

**Technical Support**

* [Polkadot Stack Exchange](https://polkadot.stackexchange.com/)
  * Create a question and tag it with "[`pop`](https://substrate.stackexchange.com/tags/pop/info)"
  * Share the StackExchange question in our [Pop Support Telegram channel](https://t.me/pop\_support)
