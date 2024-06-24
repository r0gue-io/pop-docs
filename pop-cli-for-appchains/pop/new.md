---
description: Generate a new parachain or pallet.
---

# new

```bash
pop new <COMMAND>
```

Use Pop CLI to create a new parachain:

```bash
pop new parachain
```

The above will provide interactive guidance to create parachain.

If interactive guidance is not desired than one can proceed with:

```bash
# Create a minimal parachain
pop new parachain my-appchain
```

### Templates

Pop CLI supports several parachain templates, to use a specific one use the flag `--template`:

```bash
# Create an assets parachain
pop new parachain my-appchain pop --template assets
# Create a contracts parachain
pop new parachain my-appchain pop --template contracts
# Create a evm parachain
pop new parachain my-appchain pop --template evm
```

You can see a full list of provider templates be running `pop new parachain --help`

Some examples are:

```bash
# Get Parity's pallet-contracts enabled parachain template
pop new parachain my-appchain parity --template cpt
# Get Parity's evm compatible parachain template
pop new parachain my-appchain parity --template fpt
```

#### Customize

For Pop templates, you can also customize your parachain by providing config options for the token symbol (as it appears in the chain metadata), token decimals, and the initial endowment for developer accounts. Here's how:

```bash
# Create a minimal parachain with "DOT" as the token symbol, 6 token decimals, and 1 billion tokens per dev account
pop new parachain my-appchain --symbol DOT --decimals 6 --endowment 1_000_000_000
```

There's also the shorter version:

```bash
pop new parachain my-appchain -s DOT -d 6 -i 1_000_000_000
```



### Generate a new pallet

To generate a new pallet:

```bash
pop new pallet my-pallet
```

Additional options:

```bash
pop new pallet --help

Generate a new pallet

Usage: pop new pallet [OPTIONS] [NAME]

Arguments:
  [NAME]  Name of the pallet [default: pallet-template]

Options:
  -a, --authors <AUTHORS>          Name of authors [default: Anonymous]
  -d, --description <DESCRIPTION>  Pallet description [default: "Frame Pallet"]
  -p, --path <PATH>                Path to the pallet, [default: current directory]
```
