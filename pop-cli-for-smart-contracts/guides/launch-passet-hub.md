# Launch Passet Hub Locally

Passet Hub is the temporary Asset Hub testnet for smart contract testing on Paseo. With Pop CLI, you can quickly spin up Passet Hub locally without needing to prepare network configuration files.

### Quick Start

Launch Paseo with Passet Hub:

```shell
pop up paseo -p passet-hub:9944
```

This automatically fetches the required binaries and chain-spec generators, so you can start testing your contracts in seconds.

The command will output the RPC endpoints for both Paseo and Passet Hub. Use the Passet Hub endpoint when deploying and interacting with your contracts.

### Learn More

* Learn more about Passet Hub (the temporary Asset Hub testnet) [here](https://forum.polkadot.network/t/testnets-paseo-officially-becomes-the-polkadot-testnet-temporary-passet-hub-chain-for-smart-contracts-testing/13209).

**Need help?**

Ask on [Polkadot Stack Exchange](https://polkadot.stackexchange.com/) (tag it [`pop`](https://substrate.stackexchange.com/tags/pop/info)) or drop by [our Telegram](https://t.me/onpopio). We're here to help!

