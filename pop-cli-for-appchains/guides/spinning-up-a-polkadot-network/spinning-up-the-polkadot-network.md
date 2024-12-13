---
description: How to spin up the Paseo Relay chain locally
---

# Spinning up Paseo

Lets create a configuration file to launch Paseo Local:

```bash
touch paseo-local.toml
```
```toml
[relaychain]
chain = "paseo-local"

[relaychain.genesis_overrides.sudo]
key = "5GrwvaEF5zXb26Fz9rcQpDWS57CtERHpNehXCPcNoHGKutQY" # Alice

[[relaychain.nodes]]
name = "alice"
rpc_port = 57731
validator = true

[[relaychain.nodes]]
name = "bob"
validator = true

[[relaychain.nodes]]
name = "charlie"
validator = true
```

As you can see, the sudo account (admin of the chain) is overridden with `Alice` account. This allows us to make changes
to Paseo Local if needed.

Run the network:
```
pop up parachain -f paseo-local.toml --verbose
```

> The `--verbose` flag provides us with extra information such as the location of the Paseo Local chain spec file.

<figure><img src="../../.gitbook/assets/Screenshot 2024-09-24 at 12.20.38 PM.png" alt=""><figcaption><p>pop up parachain -f paseo-local.toml --verbose</p></figcaption></figure>

Paseo Local should now be running on your machine and producing blocks!

## Network Endpoints

Based on the `paseo-local.toml` file, the following validator nodes are spun up:
- Alice: ws://localhost:57731
- Bob: ws://localhost:57735.
- Charlie: ws://localhost:57739.

The `rpc_port` for Alice has been specified, the ports for Bob & Charlie are dynamically assigned.

These endpoints come in handy when you want to interact with the chain (e.g. `pop call chain`).

## Configure Paseo Local

As of now, Paseo Local doesn't provide cores to validate parachain blocks on demand. We will have to make 2 calls to Paseo Local
using `Alice` as admin account.

First, configure Paseo Local to set coretime cores to `1`:
```bash
pop call chain --pallet Configuration --function set_coretime_cores --args "1" --url ws://localhost:57731/ --suri //Alice --sudo --skip-confirm
```

> Note: the specified rpc port `57731` is specified in the created `paseo-local.toml` file and is to interact with the validator `alice`.

Second, assign the core to the on demand pool:
```bash
pop call chain --url ws://localhost:57731 --call 0xff004a0400000a000000040100e100 --suri //Alice --skip-confirm
```

For more examples of network configurations:

* [https://github.com/r0gue-io/pop-cli/tree/main/tests/networks](https://github.com/r0gue-io/pop-cli/tree/main/tests/networks)

For more advanced options, such as specifying the Relay chain version, run the following command:

```
pop up parachain --help
```

### Learning Resources

* 🧑‍🏫 To learn about Polkadot in general, [Polkadot.network](https://polkadot.network/) website is a good starting point.
  * ⭕ Learn more about parachains [here](https://wiki.polkadot.network/docs/learn-parachains).
* 🧑‍🔧 For technical introduction, [here](https://github.com/paritytech/polkadot-sdk#-documentation) are the Polkadot SDK documentation resources.

**Technical Support**

* [Polkadot Stack Exchange](https://polkadot.stackexchange.com/)
  * Create a question and tag it with "[`pop`](https://substrate.stackexchange.com/tags/pop/info)"
  * Share the StackExchange question in our [Pop Support Telegram channel](https://t.me/pop\_support)
