# Create a new parachain

### Interactive Guidance

To create a new Parachain **with** Pop CLI's interactive guidance and follow the screen prompts

```
pop new parachain
```

### Manual (non-interactive)

To create a new Parachain **without** Pop CLI's interactive guidance

```
pop new parachain my-chain

┌   Pop CLI : Generating "my-chain" using Base Parachain Template!
│
◇  Generation complete
│
⚙  Version: polkadot-v1.9.0
│  
└  cd into "my-chain" and enjoy hacking! 🚀
```

You can specify different flags to configure your parachain. To see a full list of configuration options:

```
pop new parachain --help

Generate a new parachain

Usage: pop new parachain [OPTIONS] <NAME> [TEMPLATE]

Arguments:
  <NAME>      Name of the project
  [TEMPLATE]  Template to use; Options are 'cpt', 'fpt'. Leave empty for default parachain template [default: base]

Options:
  -s, --symbol <SYMBOL>                Token Symbol [default: UNIT]
  -d, --decimals <DECIMALS>            Token Decimals [default: 12]
  -i, --endowment <INITIAL_ENDOWMENT>  Token Endowment for dev accounts [default: "1u64 << 60"]
  -p, --path <PATH>                    Path for the parachain project, [default: current directory]
  -h, --help                           Print help
```
