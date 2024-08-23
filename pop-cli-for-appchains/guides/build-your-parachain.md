# Build your parachain

To build your parachain using Pop CLI

```shell
cd my-appchain
pop build
```

{% hint style="info" %}
For Pop CLI versions <`0.3.0` the `pop build` command is `pop build parachain`
{% endhint %}

```
┌   Pop CLI : Building a parachain
│
   Compiling parachain-template-runtime v0.1.0 (/Users/pop/src/my-appchain/runtime)
   Compiling parachain-template-node v0.1.0 (/Users/pop/src/my-appchain/node)
    Finished release [optimized] target(s) in 1m 20s

└  Build Completed Successfully!
```

If you are outside the project's directory, you can specify the path

```shell
pop build -p ./my-appchain
```

{% hint style="info" %}
For Pop CLI versions <`0.3.0` the `pop build` command is `pop build parachain`
{% endhint %}

If you are building the parachain with the intent to onboard the parachain to a Polkadot Relay chain then you can run the following build command:

```
pop build -p ../my-appchain --para_id 2000
```

This command will build your parachain and generate the chain spec, WebAssembly runtime for the parachain, and generate the parachain genesis state needed for registering and onboarding a parachain onto the Relay chain.

#### Learning Resources

* 🧑‍🏫 To learn about Polkadot in general, [Polkadot.network](https://polkadot.network/) website is a good starting point.
  * ⭕ Learn more about parachains [here](https://wiki.polkadot.network/docs/learn-parachains).
* 🧑‍🔧 For technical introduction, [here](https://github.com/paritytech/polkadot-sdk#-documentation) are the Polkadot SDK documentation resources.
