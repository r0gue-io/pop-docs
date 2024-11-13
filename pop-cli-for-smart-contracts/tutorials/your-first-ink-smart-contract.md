---
description: Learn how to create and deploy your first ink! smart contract
---

# Your first ink! smart contract

<figure><img src="../.gitbook/assets/ink!-logo.png" alt=""><figcaption></figcaption></figure>

### Audience <a href="#audience" id="audience"></a>

Developers who want to learn how to:

* use the [Pop CLI](https://github.com/r0gue-io/pop-cli) tool to speed up development
* deploy smart contracts on [Pop](https://github.com/r0gue-io/pop-node)

### Learning Objectives <a href="#learning-objectives" id="learning-objectives"></a>

On completion of this tutorial, developers will be able to:

* create, build, test, and deploy ink! smart contracts
* deploy smart contracts to [Pop](https://github.com/r0gue-io/pop-node) on [Paseo](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Frpc.ibp.network%2Fpaseo) (Polkadot's Test Network)
* interact with the deployed contract

### Installing Pop CLI <a href="#installing-pop-cli" id="installing-pop-cli"></a>

Let's install the powerful [Pop CLI](https://github.com/r0gue-io/pop-cli) tool which will be used throughout this tutorial.

```
cargo install --force --locked pop-cli
```

Make sure the [Pop CLI](https://github.com/r0gue-io/pop-cli) tool has been installed correctly by running the `pop` command in your terminal.

```bash
pop --help
```

### Getting Started <a href="#getting-started" id="getting-started"></a>

TL;DR;

* [Why ink!? talk](https://www.youtube.com/live/js2SpZf72t4?si=dKwbYtO0CyH18zbt\&t=153) by [@brunopgalvao](https://x.com/brunopgalvao)

Questions:

1. What are ink! smart contracts?
2. What are the benefits of using the ink! programming language?
3. Why use ink! to develop smart contracts?
4. What makes ink! so nice?

Take 10 minutes to skim over the [ink! documentation](https://use.ink/) and answer the above questions.

#### ink! contracts are smart(er) <a href="#ink-contracts-are-smart-er" id="ink-contracts-are-smart-er"></a>

ink! allows you to write smart contracts using Rust. That means you get all the benefits of the Rust programming language:

The ink! embedded domain-specific language (eDSL) retro-fits your Rust code with powerful features that enhance smart contract development. In doing so, ink! improves the developer ergonomics when it comes to using Rust for writing smart contracts.

Let's create a simple ink! smart contract:

```
pop new contract flipper

┌   Pop CLI : Generating new contract "flipper"!
│
◇  Smart contract created! Located in the following directory "/flipper"
│
└  cd into "flipper" and enjoy hacking! 🚀
```

By default, when we generate an ink! smart contract using the command above, we get a template (the "flipper" template). It serves as a good starting point for contract development.

Pop CLI supports several templates. If you would like to know what templates are available you can run the following command to see a list: `pop new contract --help`

Let's look at our first bit of ink! code.

You will see the following in this directory:

```
flipper
  └─ lib.rs
  └─ Cargo.toml
  └─ .gitignore
```

Inside `lib.rs` you will find all the contract logic.

Take a moment to get familiar with the ink! programming language:

### Building the ink! smart contract <a href="#building-the-ink-smart-contract" id="building-the-ink-smart-contract"></a>

Let's build flipper! Make sure you are inside the flipper directory and run the following command:

```
pop build

┌   Pop CLI : Building a contract
│
 [==] Checking clippy linting rules
   Compiling proc-macro2 v1.0.79
   Compiling unicode-ident v1.0.12
   Compiling syn v1.0.109
   Compiling equivalent v1.0.1
   Compiling hashbrown v0.14.3
   Compiling winnow v0.5.40
   ....
   Finished release [optimized] target(s) in 21.17s
   Running `target/ink/release/metadata-gen`
 [==] Generating bundle
◆  
│  Original wasm size: 21.3K, Optimized: 1.6K
│  
│  The contract was built in RELEASE mode.
│  
│  Your contract artifacts are ready. You can find them in:
│  /Users/bruno/src/flipper/target/ink
│  
│    - flipper.contract (code + metadata)
│    - flipper.wasm (the contract's code)
│    - flipper.json (the contract's metadata)
│  
└  Build completed successfully!
```

Awesome, your contract is built!

With Pop CLI, you can also run your smart contract tests.

### Run your ink! smart contract unit tests <a href="#run-your-ink-smart-contract-unit-tests" id="run-your-ink-smart-contract-unit-tests"></a>

```
pop test contract

┌   Pop CLI : Building a contract
│
 [==] Checking clippy linting rules
   Compiling proc-macro2 v1.0.79
   ....
   Compiling flipper v0.1.0 (/Users/bruno/src/pop-cli/flipper)
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

Okay, so you contract builds, your contract passes the tests. We can now deploy the contract.

### Deploy your contract <a href="#deploy-your-contract-locally" id="deploy-your-contract-locally"></a>

Once you have battle-tested your smart contract using unit and e2e tests, you are ready to test your smart contract on the [Pop Testnet](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Frpc2.paseo.popnetwork.xyz).

Before you deploy, make sure you have bridged some tokens from the Polkadot Relay chain to your Pop account. To learn how to do this, go here:

* [https://learn.onpop.io/contracts/guides/get-tokens-on-pop](https://learn.onpop.io/contracts/guides/get-tokens-on-pop)

#### Deploy <a href="#deploy" id="deploy"></a>

Now that your account is funded on Pop, you can deploy your contract.

1. Go to: [contracts.onpop.io](https://contracts.onpop.io/)
2. Select "Add New Contract"
3. Select "Upload New Contract Code"

<figure><img src="../.gitbook/assets/Screenshot 2024-11-13 at 6.24.40 PM.png" alt=""><figcaption></figcaption></figure>

Next, give the contract a name and upload the `flipper/target/ink/flipper.contract` file which contains the contract's Wasm code as well as the contract's metadata.

<figure><img src="../.gitbook/assets/Screenshot 2024-11-13 at 6.25.32 PM.png" alt=""><figcaption></figcaption></figure>

You can now interact with your contract!

<figure><img src="../.gitbook/assets/Screenshot 2024-11-13 at 6.24.00 PM.png" alt=""><figcaption></figcaption></figure>

Congrats! Your contract is deployed!

#### Resources

* [https://use.ink](https://use.ink)

**Technical Support**

* [Polkadot Stack Exchange](https://polkadot.stackexchange.com/)
  * Create a question and tag it with "[`pop`](https://substrate.stackexchange.com/tags/pop/info)"
  * Share the StackExchange question in our [Pop Support Telegram channel](https://t.me/pop\_support)

### Feedback <a href="#feedback" id="feedback"></a>

> [Take a minute to give us feedback on this tutorial so we can improve it!](https://github.com/r0gue-io/pop-cli/discussions/categories/general)
