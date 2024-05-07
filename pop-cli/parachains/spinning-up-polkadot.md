# Spinning up Polkadot

Perhaps the simplest workflow a developer would want is to spin up the Polkadot Relay chain.

With Pop CLI this can be done.

First, create the following network.toml file in your project's root directory.

```
touch network.toml
```

```toml
[relaychain]
chain = "rococo-local"

[[relaychain.nodes]]
name = "alice"
validator = true

[[relaychain.nodes]]
name = "bob"
validator = true
```

> This is specifying a network configuration for the Rococo Relay chain (which is the test version of the Polkadot Relay chain) along with two validator nodes to run the network: alice and bob

Let's run it.

```
pop up parachain -f ./network.toml
```

It may take some time if it prompts you to download the binaries. Grab some coffee.

Eventually you will get output like so.

```
┌   Pop CLI : Deploy a parachain
│
◇  🚀 Network launched successfully - ctrl-c to terminate
│  ⛓️ rococo-local
│       alice:
│         portal: https://polkadot.js.org/apps/?rpc=ws://127.0.0.1:62715#/explorer
│         logs: tail -f /var/folders/vl/txnq6gdj22s9rn296z0md27w0000gn/T/zombie-2957639d-9818-43e5-8a1d-db85ad27fea2/alice/alice.log
│       bob:
│         portal: https://polkadot.js.org/apps/?rpc=ws://127.0.0.1:62719#/explorer
│         logs: tail -f /var/folders/vl/txnq6gdj22s9rn296z0md27w0000gn/T/zombie-2957639d-9818-43e5-8a1d-db85ad27fea2/bob/bob.log
```

Congrats! You now have the Polkadot Relay chain running!
