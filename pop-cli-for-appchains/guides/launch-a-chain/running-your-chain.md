# Launch a local development network

Use `pop up network` with a Zombienet configuration file to run your chain locally.

## Prerequisites

- A chain project built with `pop build`.
- Pop CLI installed.

## Create a network config

In your chain project directory:

```shell
cd my-chain
touch network.toml
```

Example `network.toml`:

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

## Launch the network

```shell
pop up network ./network.toml
```

Pop CLI sources required binaries when needed and starts the relay chain and your parachain locally.

> Under the hood, Pop CLI uses Zombienet. For advanced configuration, see the Zombienet docs: https://github.com/paritytech/zombienet

#### Learning Resources

* 🧑‍🏫 To learn about Polkadot in general, [Polkadot.network](https://polkadot.network/) website is a good starting point.
  * ⭕ Learn more about Polkadot chains [here](https://wiki.polkadot.network/docs/learn-parachains).
* 🧑‍🔧 For technical introduction, [here](https://github.com/paritytech/polkadot-sdk#-documentation) are the Polkadot SDK documentation resources.

**Need help?**

Ask on [Polkadot Stack Exchange](https://polkadot.stackexchange.com/) (tag it [`pop`](https://substrate.stackexchange.com/tags/pop/info)) or drop by [our Telegram](https://t.me/onpopio). We're here to help!
