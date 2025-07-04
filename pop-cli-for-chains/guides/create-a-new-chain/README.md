# Create a new chain

### Interactive Guidance (Recommended)

Create a new chain **with** Pop CLI's interactive guidance by simply entering:

```shell
pop new chain
```

You will be prompted to select a template and depending on the options chosen, be prompted with additional customization options.

<figure><img src="../../.gitbook/assets/001.gif" alt="pop new chain"><figcaption><p>pop new chain</p></figcaption></figure>

### Manual (non-interactive)

Create a new standard chain **without** Pop CLI's interactive guidance by specifying a name for your chain:

```shell
pop new chain my-chain
```

```
┌   Pop CLI : Generating "my-chain" using Standard from Pop!
│
◇  Generation complete
│
⚙  Version: polkadot-v1.9.0
│  
└  cd into "my-chain" and enjoy hacking! 🚀
```

You can specify different flags to configure your chain. To see a full list of configuration options:

```shell
pop new chain --help
```

```
Generate a new chain

Usage: pop new chain [OPTIONS] [NAME] [PROVIDER]

Arguments:
  [NAME]      Name of the project. If empty assistance in the process will be provided.
  [PROVIDER]  Template provider. [default: pop] [possible values: pop, openzeppelin, parity]

Options:
  -t, --template <TEMPLATE>            Template to use. [possible values: standard, assets, contracts, evm, polkadot-generic-runtime-template, cpt, fpt]
  -r, --release-tag <RELEASE_TAG>      Release tag to use for template. If empty, latest release will be used.
  -s, --symbol <SYMBOL>                Token Symbol [default: UNIT]
  -d, --decimals <DECIMALS>            Token Decimals [default: 12]
  -i, --endowment <INITIAL_ENDOWMENT>  Token Endowment for dev accounts [default: "1u64 << 60"]
  -v, --verify                         Fetches the latest license, release, and commit SHA data from GitHub.
  -h, --help                           Print help
```



#### Learning Resources

* 🧑‍🏫 To learn about Polkadot in general, [Polkadot.network](https://polkadot.network/) website is a good starting point.
  * ⭕ Learn more about parachains [here](https://wiki.polkadot.network/docs/learn-parachains).
* 🧑‍🔧 For technical introduction, [here](https://github.com/paritytech/polkadot-sdk#-documentation) are the Polkadot SDK documentation resources.

**Technical Support**

* [Polkadot Stack Exchange](https://polkadot.stackexchange.com/)
  * Create a question and tag it with "[`pop`](https://substrate.stackexchange.com/tags/pop/info)"
  * Share the StackExchange question in our [Pop Support Telegram channel](https://t.me/pop\_support)
