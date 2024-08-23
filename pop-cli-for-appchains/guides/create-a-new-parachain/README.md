# Create a new parachain

### Interactive Guidance (Recommended)

Create a new parachain **with** Pop CLI's interactive guidance by simply entering:

```shell
pop new parachain
```

You will be prompted to select a template and depending on the options chosen, be prompted with additional customization options.

### Manual (non-interactive)

Create a new standard parachain **without** Pop CLI's interactive guidance by specifying a name for your parachain:

```shell
pop new parachain my-chain
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

You can specify different flags to configure your parachain. To see a full list of configuration options:

```shell
pop new parachain --help
```

```
Generate a new parachain

Usage: pop new parachain [OPTIONS] [NAME] [PROVIDER]

Arguments:
  <NAME>      Name of the project
  [PROVIDER]  Template provider. [default: pop] [possible values: pop, parity]

Options:
  -t, --template <TEMPLATE>            Template to use. [possible values: base, assets, cpt, fpt]
  -s, --symbol <SYMBOL>                Token Symbol [default: UNIT]
  -d, --decimals <DECIMALS>            Token Decimals [default: 12]
  -i, --endowment <INITIAL_ENDOWMENT>  Token Endowment for dev accounts [default: "1u64 << 60"]
  -h, --help                           Print help
```



#### Learning Resources

* 🧑‍🏫 To learn about Polkadot in general, [Polkadot.network](https://polkadot.network/) website is a good starting point.
  * ⭕ Learn more about parachains [here](https://wiki.polkadot.network/docs/learn-parachains).
* 🧑‍🔧 For technical introduction, [here](https://github.com/paritytech/polkadot-sdk#-documentation) are the Polkadot SDK documentation resources.
