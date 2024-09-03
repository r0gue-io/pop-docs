---
description: The guide is for running a parachain on a local Paseo TestNet
---

# Running on Paseo locally

[Paseo](https://x.com/PaseoNetwork) is the community-run Polkadot Relay chain TestNet.

> Note: For contrast, Rococo is the [Parity](https://parity.io)-run Polkadot Relay chain TestNet which [will be deprecating](https://forum.polkadot.network/t/a-new-test-network-for-polkadot/4325) in favour of the [Paseo TestNet](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Fpaseo-rpc.dwellir.com#/explorer).

A good development workflow:&#x20;

1. Run your parachain **locally** on Paseo TestNet for development purposes&#x20;
2. When ready to test in a live environment with other parachains, deploy on the **live** [Paseo TestNet](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Fpaseo-rpc.dwellir.com#/explorer).

Let's get started.

To run your parachain on Paseo locally, you will need to spin up a local network with your parachain configuration.

The `pop up parachain` command can help with this.

```shell
pop up parachain --help
```

First we need to define a [zombienet](https://github.com/paritytech/zombienet) network configuration file. You can do this in the root of your project.

```
cd my-appchain
touch network.toml
```

Add the following configuration, adapting as necessary.

```toml
[relaychain]
chain = "paseo-local"

[[relaychain.nodes]]
name = "alice"
validator = true

[[relaychain.nodes]]
name = "bob"
validator = true

[[parachains]]
id = 2000
default_command = "./target/release/parachain-template-node"

[[parachains.collators]]
name = "collator-01"
```

> This network configuration will launch a relay chain using a `paseo-local` instance of the Polkadot Relay chain with two validator nodes to run the network: `alice` and `bob`. It will also run `parachain-template-node` with one collator node named `collator-01`.
>
> Note: The `default_command` is using `release` and not `debug.` If you are developing, you may want to switch the path to use `debug.`

Cool. Let's spin this up!

```shell
pop up parachain -f ./network.toml
```

If this is the first time you are running the `pop up` command, you will be prompted to source the required Polkadot binaries. This will take some time, grab some coffee.

Once all the binaries are sourced, you should have output similar to this.

```
┌   Pop CLI : Deploy a parachain
│
◇  🚀 Network launched successfully - ctrl-c to terminate
│  ⛓️ paseo-local
│       alice:
│         portal: https://polkadot.js.org/apps/?rpc=ws://127.0.0.1:62551#/explorer
│         logs: tail -f /var/folders/vl/txnq6gdj22s9rn296z0md27w0000gn/T/zombie-c0eb16fc-5d11-4792-aced-493ef972d056/alice/alice.log
│       bob:
│         portal: https://polkadot.js.org/apps/?rpc=ws://127.0.0.1:62555#/explorer
│         logs: tail -f /var/folders/vl/txnq6gdj22s9rn296z0md27w0000gn/T/zombie-c0eb16fc-5d11-4792-aced-493ef972d056/bob/bob.log
│  ⛓️ local_testnet: 2000
│       collator-01:
│         portal: https://polkadot.js.org/apps/?rpc=ws://127.0.0.1:62559#/explorer
│         logs: tail -f /var/folders/vl/txnq6gdj22s9rn296z0md27w0000gn/T/zombie-c0eb16fc-5d11-4792-aced-493ef972d056/collator-01/collator-01.log
│
```

Congrats! You have now spun up a network with your parachain running!

> Under-the-hood, Pop CLI uses zombienet to spin up the network.\
> For more advanced network configurations and options consult the [zombienet repo](https://github.com/paritytech/zombienet)

#### Learning Resources

* 🧑‍🏫 To learn about Polkadot in general, [Polkadot.network](https://polkadot.network/) website is a good starting point.
  * ⭕ Learn more about parachains [here](https://wiki.polkadot.network/docs/learn-parachains).
* 🧑‍🔧 For technical introduction, [here](https://github.com/paritytech/polkadot-sdk#-documentation) are the Polkadot SDK documentation resources.

**Technical Support**

* [Polkadot Stack Exchange](https://polkadot.stackexchange.com/)
  * Create a question and tag it with "[`pop`](https://substrate.stackexchange.com/tags/pop/info)"
  * Share the StackExchange question in our [Pop Support Telegram channel](https://t.me/pop\_support)
