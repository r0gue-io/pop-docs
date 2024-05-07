# Create an EVM parachain

To create an EVM parachain with Pop CLI

```
pop new parachain my-evm-chain pop -t evm
```

Additional configuration flags are available.

```
pop new parachain --help

Generate a new parachain

Usage: pop new parachain [OPTIONS] [NAME] [PROVIDER]

Arguments:
  [NAME]      Name of the project. If empty assistance in the process will be provided.
  [PROVIDER]  Template provider. [default: pop] [possible values: pop, parity]

Options:
  -t, --template <TEMPLATE>            Template to use. [possible values: base, assets, cpt, fpt]
  -s, --symbol <SYMBOL>                Token Symbol [default: UNIT]
  -d, --decimals <DECIMALS>            Token Decimals [default: 12]
  -i, --endowment <INITIAL_ENDOWMENT>  Token Endowment for dev accounts [default: "1u64 << 60"]
  -p, --path <PATH>                    Path for the parachain project, [default: current directory]
  -h, --help                           Print help
```
