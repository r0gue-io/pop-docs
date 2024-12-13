---
description: How to spin up the Paseo Relay chain locally
---

# Spinning up Paseo

Use Pop CLI to spin up Paseo locally!

First, create the following `network.toml` file in your project's root directory.

```shell
touch network.toml
```

```toml
[relaychain]
chain = "paseo-local"

[[relaychain.nodes]]
name = "alice"
validator = true

[[relaychain.nodes]]
name = "bob"
validator = true
```

> This is specifying a network configuration using Paseo (which is the "test" version of the Polkadot Relay chain) along with two validator nodes to run the network: `alice` and `bob`.

Let's run it.

```shell
pop up parachain -f ./network.toml
```

It may take some time if it prompts you to download the binaries. Grab some coffee.

Eventually you will get output like so.

```
┌   Pop CLI : Deploy a parachain
│
◇  🚀 Network launched successfully - ctrl-c to terminate
│  ⛓️ paseo-local
│       alice:
│         portal: https://polkadot.js.org/apps/?rpc=ws://127.0.0.1:62715#/explorer
│         logs: tail -f /var/folders/vl/txnq6gdj22s9rn296z0md27w0000gn/T/zombie-2957639d-9818-43e5-8a1d-db85ad27fea2/alice/alice.log
│       bob:
│         portal: https://polkadot.js.org/apps/?rpc=ws://127.0.0.1:62719#/explorer
│         logs: tail -f /var/folders/vl/txnq6gdj22s9rn296z0md27w0000gn/T/zombie-2957639d-9818-43e5-8a1d-db85ad27fea2/bob/bob.log
```

Congrats! You now have a Paseo network running locally!

For more examples of network configurations:

* [https://github.com/r0gue-io/pop-cli/tree/main/tests/networks](https://github.com/r0gue-io/pop-cli/tree/main/tests/networks)

For more advanced options, such as specifying the Relay chain version, run the following command:

```
pop up parachain --help
```

#### Learning Resources

* 🧑‍🏫 To learn about Polkadot in general, [Polkadot.network](https://polkadot.network/) website is a good starting point.
  * ⭕ Learn more about parachains [here](https://wiki.polkadot.network/docs/learn-parachains).
* 🧑‍🔧 For technical introduction, [here](https://github.com/paritytech/polkadot-sdk#-documentation) are the Polkadot SDK documentation resources.

**Technical Support**

* [Polkadot Stack Exchange](https://polkadot.stackexchange.com/)
  * Create a question and tag it with "[`pop`](https://substrate.stackexchange.com/tags/pop/info)"
  * Share the StackExchange question in our [Pop Support Telegram channel](https://t.me/pop\_support)
