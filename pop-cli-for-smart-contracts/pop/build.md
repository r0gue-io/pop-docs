---
description: Build a contract
---

# build

```bash
pop build contract [OPTIONS]
```

Build an existing smart contract:

```bash
pop build contract -p ./my_contract
```

By default, the contract is compiled with `debug` functionality included.

This enables the contract to output debug messages but increases the contract size and the amount of gas used.

For production builds, use the --release flag: `--release`:

```bash
pop build contract -p ./my_contract --release
```

