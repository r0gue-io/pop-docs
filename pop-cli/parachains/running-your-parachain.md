# Running your parachain

To run your parachain, you will need to spin up a Polkadot Network with your Parachain configuration.

The `pop up parachain` command can help with this.

```
pop up parachain --help

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

Say we want to spin up the Polkadot Relay chain with the Pop Network node.

First we need to define a zombienet network configuration file. You can do this in the root of your project.

```
cd my-parachain
touch network.toml
```

network.toml

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
id = 9090
default_command = "pop-node"

[[parachains.collators]]
name = "pop"
args = ["-lruntime::contracts=debug"]
```

> This network configuration will launch a "rococo-local" instance of the Polkadot Relay chain with two validator nodes to run the network: alice and bob. It will also run the pop-node (Pop Network) with one collator node named "pop" along with a debug flag so we can inspect any `debug_println!` statements that are used on our ink! smart contracts in the collator node's logs.

Cool. Let's spin this up.

```
pop up parachain -f ./network.toml -p https://github.com/r0gue-io/pop-node
```

If this is the first time you are running the `pop up` command, it will prompt you to download the Polkadot binaries. This will take some time, grab some coffee. Notice that we also have to provide the URL for where to download the parachain binary, in this case: pop-node.

Once all the binaries are loaded.  You should have output similar to this.

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
│  ⛓️ local_testnet: 9090
│       pop:
│         portal: https://polkadot.js.org/apps/?rpc=ws://127.0.0.1:62559#/explorer
│         logs: tail -f /var/folders/vl/txnq6gdj22s9rn296z0md27w0000gn/T/zombie-c0eb16fc-5d11-4792-aced-493ef972d056/pop/pop.log
│
```

Congrats! You have now spun up a network with Pop Network running!
