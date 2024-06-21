# Running your parachain

To run your parachain, you will need to spin up a local network with your parachain configuration.

The `pop up parachain` command can help with this.

```shell
pop up parachain --help
```

```
Deploy a parachain to a local network

Usage: pop up parachain [OPTIONS] --file <FILE>

Options:
  -f, --file <FILE>
          The Zombienet configuration file to be used
  -r, --relay-chain <RELAY_CHAIN>
          The version of Polkadot to be used for the relay chain, as per the release tag (e.g. "v1.7.0")
  -s, --system-parachain <SYSTEM_PARACHAIN>
          The version of Polkadot to be used for a system parachain, as per the release tag (e.g. "v1.7.0")
  -p, --parachain <PARACHAIN>
          The url of the git repository of a parachain to be used, with branch/release tag specified as #fragment (e.g. 'https://github.com/org/repository#tag'). A specific binary name can also be optionally specified via query string parameter (e.g. 'https://github.com/org/repository?binaryname#tag'), defaulting to the name of the repository when not specified
  -v, --verbose
          Whether the output should be verbose
  -h, --help
          Print help
```

Say we want to spin up a local network for your parachain. First we need to define a zombienet network configuration file. You can do this in the root of your project.

```
cd my-parachain
touch network.toml
```

Add the following configuration, adapting as necessary.

```toml
[relaychain]
chain = "rococo-local"

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

> This network configuration will launch a relay chain using a `rococo-local` instance of Polkadot with two validator nodes to run the network: `alice` and `bob`. It will also run `parachain-template-node` with one collator node named `collator-01`.

Cool. Let's spin this up, ensuring that your parachain binary has been built using `pop build parachain`.

```shell
pop up parachain -f ./network.toml
```

If this is the first time you are running the `pop up` command, it will prompt you to source the required Polkadot binaries. This will take some time, grab some coffee.

Once all the binaries are sourced, you should have output similar to this.

```
┌   Pop CLI : Deploy a parachain
│
◇  🚀 Network launched successfully - ctrl-c to terminate
│  ⛓️ rococo-local
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
