# Launch a known Chain

With Pop CLI you can quickly spin up a supported chain without needing to prepare network configuration files.

A `supported chain` currently refers to one of the system chains. Pop CLI automatically fetches the required binaries and chain-spec generators so you can launch in seconds. Support for additional chains will be added in the future.

### Example Usage

Spin up Paseo with Asset Hub:

```shell
pop up paseo -p asset-hub
```

Run Kusama on port `8833` with Asset Hub chain assigned to `9944`:

```shell
pop up kusama --port 8833 --parachain asset-hub:9944 -r stable2412-4
```

Launch Polkadot with two parachains, including a specific ParaId for Asset Hub:

```shell
pop up polkadot --port 8833 --parachain asset-hub#3395:9977 -r stable2412-4
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

> For more details on network configuration files, check the [Zombienet documentation](https://docs.polkadot.com/develop/toolkit/parachains/spawn-chains/zombienet/get-started/?utm_source=chatgpt.com#configure-zombienet)

Run the network:

```
pop up network ./paseo-local.toml --verbose
```

#### Learning Resources

* 🧑‍🏫 To learn about [System Chains](https://wiki.polkadot.com/learn/learn-system-chains/) website is a good starting point.
  * ⭕ Learn more about PassetHub (the temporary AssetHub testnet) [here](https://forum.polkadot.network/t/testnets-paseo-officially-becomes-the-polkadot-testnet-temporary-passet-hub-chain-for-smart-contracts-testing/13209).
* 🧑‍🔧 For technical documentation of Paseo Network, [here](https://github.com/paseo-network).

**Need help?**

Ask on [Polkadot Stack Exchange](https://polkadot.stackexchange.com/) (tag it [`pop`](https://substrate.stackexchange.com/tags/pop/info)) or drop by [our Telegram](https://t.me/onpopio). We're here to help!
