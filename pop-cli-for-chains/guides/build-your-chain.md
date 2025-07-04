# Build your chain

To build your chain using Pop CLI

```shell
cd my-chain
pop build
```

{% hint style="info" %}
For Pop CLI versions <`0.3.0` the `pop build` command is `pop build chain`
{% endhint %}

```
┌   Pop CLI : Building a chain
│
   Compiling chain-template-runtime v0.1.0 (/Users/pop/src/my-chain/runtime)
   Compiling chain-template-node v0.1.0 (/Users/pop/src/my-chain/node)
    Finished release [optimized] target(s) in 1m 20s

└  Build Completed Successfully!
```

If you are outside the project's directory, you can specify the path

```shell
pop build -p ./my-chain
```

{% hint style="info" %}
For Pop CLI versions <`0.3.0` the `pop build` command is `pop build chain`
{% endhint %}

If you are building the chain with the intent to onboard the chain to a Polkadot Relay chain then you can run the following build command:

```
pop build -p ../my-chain --para_id 2000
```

This command will build your chain and generate the chain spec, WebAssembly runtime for the chain, and generate the chain genesis state needed for registering and onboarding a chain onto the Relay chain.

#### Learning Resources

* 🧑‍🏫 To learn about Polkadot in general, [Polkadot.network](https://polkadot.network/) website is a good starting point.
  * ⭕ Learn more about chains [here](https://wiki.polkadot.network/docs/learn-parachains).
* 🧑‍🔧 For technical introduction, [here](https://github.com/paritytech/polkadot-sdk#-documentation) are the Polkadot SDK documentation resources.

**Technical Support**

* [Polkadot Stack Exchange](https://polkadot.stackexchange.com/)
  * Create a question and tag it with "[`pop`](https://substrate.stackexchange.com/tags/pop/info)"
  * Share the StackExchange question in our [Pop Support Telegram channel](https://t.me/pop_support)
