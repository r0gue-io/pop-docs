# Create an assets parachain

With Pop CLI you can create an assets parachain using a template

```
pop new parachain my-chain pop -t assets
```

where `pop` is the template provider and `-t` is the template, in this case, the "assets" template from the "pop" set/provider of templates.

Keep in mind there are additional configuration flags you can provide.

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
