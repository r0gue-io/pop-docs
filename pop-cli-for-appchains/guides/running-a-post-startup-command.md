---
description: >-
  The following will guide developers on how to run a command after launch of
  your network.
---

# Running a post-startup command

Often there is a use case to run a command (or script) upon network initialization.

Say you want to fund accounts on your appchain or run a command to check account balances, Pop CLI allows you to do this via the `--cmd` option that is included in the `pop up network` command:

```
pop up network --help
```

To run a command post-initialization of the network, you can use the `--cmd` flag:

```bash
pop up network -f ./tests/networks/pop.toml --cmd path/to/command
```

Here is an example of a simple script to update account balances on the Polkadot Relay chain:

[https://github.com/brunopgalvao/set-balance](https://github.com/brunopgalvao/set-balance/blob/main/src/main.rs)

Clone and compile the script:

```bash
git clone https://github.com/brunopgalvao/set-balance
cd set-balance
cargo build
```

Create a simple zombienet network.toml file to spin up the Polkadot Relay chain:

```bash
touch network.toml
```

```toml
[relaychain]
chain = "paseo-local"

[[relaychain.nodes]]
name = "alice"
rpc_port = 8833
validator = true

[[relaychain.nodes]]
name = "bob"
validator = true
```

Spin up the Polkadot Relay chain with Pop CLI:

```
pop up network -f network.toml -r v1.8.0 --cmd ./target/debug/set-balance
```

```
┌   Pop CLI : Launch a local network
│
◓  Spinning up network & running command: ../set-balance/target/debug/set-balance                                                                                                             Connecting to the Relay chain...
Preparing to set Alice's balance...
New balance to be set for Alice: 3000000000000000000000
Creating SUDO call to set Alice's balance...
Submitting the transaction to set Alice's balance...
◓  Spinning up network & running command: ../set-balance/target/debug/set-balance                                                                                                             Alice's balance has been successfully set to: 3000000000000000000000
◇  🚀 Network launched successfully - ctrl-c to terminate
│  ⛓️ paseo-local
│       alice:
│         portal: https://polkadot.js.org/apps/?rpc=ws://127.0.0.1:8833#/explorer
│         logs: tail -f /var/folders/vl/txnq6gdj22s9rn296z0md27w0000gn/T/zombie-4299a032-01d0-4704-9c80-64f09b387aec/alice/alice.log
│       bob:
│         portal: https://polkadot.js.org/apps/?rpc=ws://127.0.0.1:53017#/explorer
│         logs: tail -f /var/folders/vl/txnq6gdj22s9rn296z0md27w0000gn/T/zombie-4299a032-01d0-4704-9c80-64f09b387aec/bob/bob.log
│
```

Pop CLI has spun up the Polkadot network and executed the post-startup script.

Alice's account has not been funded!

<figure><img src="../.gitbook/assets/Screenshot 2024-06-03 at 6.03.34 PM.png" alt="" width="375"><figcaption><p>Alice Dev Account</p></figcaption></figure>

Congrats!

**Need help?**

Ask on [Polkadot Stack Exchange](https://polkadot.stackexchange.com/) (tag it [`pop`](https://substrate.stackexchange.com/tags/pop/info)) or drop by [our Telegram](https://t.me/onpopio). We're here to help!
