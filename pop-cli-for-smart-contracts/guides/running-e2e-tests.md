# Running E2E tests

Make sure you are in your contract's directory:

```shell
cd flipper
```

If you look at the contract source, you will notice it has end-to-end tests at the _end_ of the `lib.rs` file.

The ink! e2e tests are run against [substrate-contracts-node](https://github.com/paritytech/substrate-contracts-node).
You will need to download the latest [release](https://github.com/paritytech/substrate-contracts-node/releases)
binary for your platform.

Run the e2e tests, adapting the `CONTRACTS_NODE` environment variable to the location of the `substrate-contracts-node`
binary:

```bash
export CONTRACTS_NODE="/Users/pop/bin/substrate-contracts-node" 
pop test contract --features e2e-tests
```

```
┌   Pop CLI : Starting end-to-end tests
│
    Finished test [unoptimized + debuginfo] target(s) in 0.83s
     Running unittests lib.rs (target/debug/deps/flipper-508d8cf8af185e86)

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
