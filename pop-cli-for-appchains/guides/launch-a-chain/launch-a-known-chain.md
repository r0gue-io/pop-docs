# Launch a known chain locally

Use `pop up <relay>` to launch a supported relay chain and system chains without writing a network config.

Supported relay chains: `paseo`, `kusama`, `polkadot`, `westend`.

## Examples

Spin up Paseo with Asset Hub:

```shell
pop up paseo -p asset-hub
```

Run Kusama on port `8833` with Asset Hub on `9944`:

```shell
pop up kusama --port 8833 --parachain asset-hub:9944 -r stable2412-4
```

Launch Polkadot with Asset Hub and a specific para ID:

```shell
pop up polkadot --port 8833 --parachain asset-hub#3395:9977 -r stable2412-4
```

#### Learning Resources

* 🧑‍🏫 To learn about [System Chains](https://wiki.polkadot.com/learn/learn-system-chains/) website is a good starting point.
  * ⭕ Learn more about PassetHub (the temporary AssetHub testnet) [here](https://forum.polkadot.network/t/testnets-paseo-officially-becomes-the-polkadot-testnet-temporary-passet-hub-chain-for-smart-contracts-testing/13209).
* 🧑‍🔧 For technical documentation of Paseo Network, [here](https://github.com/paseo-network).

**Need help?**

Ask on [Polkadot Stack Exchange](https://polkadot.stackexchange.com/) (tag it [`pop`](https://substrate.stackexchange.com/tags/pop/info)) or drop by [our Telegram](https://t.me/onpopio). We're here to help!
