# Launch a Chain in Development

To run your parachain, you will need to spin up a local network with your parachain configuration.

The `pop up` command can help with this.

```shell
pop up network --help
```

> [!TIP]
> For Pop CLI versions <`0.7.0` the `pop up network` command is `pop up parachain`

Say we want to spin up a local network for your parachain. First we need to define a [zombienet](https://github.com/paritytech/zombienet) network configuration file. You can do this in the root of your project.

```
cd my-appchain
touch network.toml
```

Add the following configuration, adapting as necessary.

> You can use `paseo-local` for your Relay chain. Paseo is the community-led Polkadot Test Relay chain.

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

> This network configuration will launch a relay chain using a `paseo-local` instance of Polkadot with two validator nodes to run the network: `alice` and `bob`. It will also run `parachain-template-node` with one collator node named `collator-01`.

Cool. Let's spin this up, ensuring that your parachain binary has been built using `pop build parachain`.

```shell
pop up ./network.toml
```

If this is the first time you are running the `pop up` command, it will prompt you to source the required Polkadot binaries. This will take some time, grab some coffee.

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

**Need help?**

Ask on [Polkadot Stack Exchange](https://polkadot.stackexchange.com/) (tag it [`pop`](https://substrate.stackexchange.com/tags/pop/info)) or drop by [our Telegram](https://t.me/onpopio). We're here to help!
