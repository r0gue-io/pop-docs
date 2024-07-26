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

For ink! end-to-end testing, you will need to have a Substrate node with [`pallet contracts`](https://paritytech.github.io/polkadot-sdk/master/pallet\_contracts/index.html) for the ink! e2e tests to run against. Pop CLI will download the [substrate-contracts-node](https://github.com/paritytech/substrate-contracts-node) binary for this purpose:

```bash
pop test contract --e2e
```

Run e2e testing against a specific pallet-contracts node:

```bash
pop test contract  -p ./my_contract --e2e --node ./bin/my-contracts-node
```

For more details on E2E testing please see [here](https://use.ink/4.x/basics/contract-testing#end-to-end-e2e-tests).
