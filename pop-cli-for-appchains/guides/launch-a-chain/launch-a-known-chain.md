# Launch a Known Chain

With Pop CLI you can quickly spin up supported parachains without needing to prepare network configuration files.

A `supported parachain` currently refers to one of the system parachains or `pop`. Pop CLI automatically fetches the required binaries and chain-spec generators so you can launch in seconds. Support for additional parachains may be added in the future.

### Example Usage

Spin up Paseo with the Asset Hub and Passet Hub parachains:

```shell
pop up paseo -p asset-hub,passet-hub
```

Run Kusama on port `8833` with Asset Hub parachain assigned to `9944`:

```shell
pop up kusama --port 8833 --parachain asset-hub:9944 -r stable2412-4
```

Launch Polkadot with two parachains, including a specific ParaId for Pop:

```shell
pop up polkadot --port 8833 --parachain asset-hub:9977,pop#3395:9944 -r stable2412-4
```

### Custom Network Configurations
You can still provide a full network configuration file if needed. The `pop up network` command now accepts a positional path argument, eliminating the need for `--file`/`-f`:

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

As you can see, the sudo account (admin of the chain) is overridden with `Alice` account. This allows us to make changes to Paseo Local if needed.

Run the network:

```
pop up network ./paseo-local.toml --verbose
```

#### Learning Resources

* 🧑‍🏫 To learn about [System Chains](https://wiki.polkadot.com/learn/learn-system-chains/) website is a good starting point.
  * ⭕ Learn more about PassetHub [here](https://forum.polkadot.network/t/testnets-paseo-officially-becomes-the-polkadot-testnet-temporary-passet-hub-chain-for-smart-contracts-testing/13209).
* 🧑‍🔧 For technical documentation of Paseo Network, [here](https://github.com/paseo-network).

**Need help?**

Ask on [Polkadot Stack Exchange](https://polkadot.stackexchange.com/) (tag it [`pop`](https://substrate.stackexchange.com/tags/pop/info)) or drop by [our Telegram](https://t.me/onpopio). We're here to help!
