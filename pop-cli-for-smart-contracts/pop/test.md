---
description: Test a smart contract
---

# test

```bash
pop test contract [OPTIONS]
```

To test an existing smart contract:

```bash
pop test contract -p ./my_contract
```



### E2E Testing

For end-to-end testing, you will need to have a Substrate node with `pallet contracts`. You do not need to run it in the background since the node is started for each test independently. To install the latest version:

```bash
cargo install contracts-node --git https://github.com/paritytech/substrate-contracts-node.git
```

If you want to run any other node with `pallet-contracts` you need to change `CONTRACTS_NODE` environment variable:

```bash
export CONTRACTS_NODE="YOUR_CONTRACTS_NODE_PATH"
```

Run e2e testing on an existing smart contract:

```bash
pop test contract  -p ./my_contract --features e2e-tests
```

For more details on E2E testing please see [here](https://use.ink/4.x/basics/contract-testing#end-to-end-e2e-tests).
