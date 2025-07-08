# Deploy

> 🚀 **Note:** For experimenting with ink! v6, make sure to check [getting-started-with-inkv6.](../welcome/migrating-to-inkv6.md)

### Local Deployment (Default)

If no `--url` is provided, Pop CLI automatically launches a local [ink-node](https://github.com/use-ink/ink-node) in the background.

```bash
pop up -p ./path-to-contract \
  --constructor <constructor_name> \
  --args <arg_1> <arg_2> ... \
  --suri //Alice \
```

* `--path`: points to the contract directory
* `--constructor`: method name (default: `new`)
* `--args`: constructor arguments
* `--suri`: secret key URI (default: `//Alice`)
* No `--url`: spins up `ws://localhost:9944` automatically

> If at anytime you need to stop the node you can do the following:
>
> 1. Find the process ID by running the following command: `lsof -i :9944`
> 2. Kill the process by running the `kill` command and passing in the ID of the process:
>    * `kill -9 3537`

When you have successfully deployed your contract you will get the following output:

```
pop up -p ./flipper --constructor new --args false --suri //Alice --url ws://127.0.0.1:9944

┌   Pop CLI : Deploy a smart contract
│
◐  Doing a dry run to estimate the gas...                                                                                                    
●  Gas limit Weight { ref_time: 264725731, proof_size: 16689 }
│  
◇  Contract deployed and instantiated: The Contract Address is "5CLPm1CeUvJhZ8GCDZCR7nWZ2m3XXe4X5MtAQK69zEjut36A"
│
└  Deployment complete
```

Save the `Contract Address` which you will need to interact with the contract.

### Deploy to Custom or Public Network

To deploy on a specific network, supply a `--url`:

```bash
pop up -path ./my_contract \
  --url ws://<network-endpoint> \
  --constructor <name> \
  --args <arg_1> <arg_2> \
  --suri <your-SURI-or-use-wallet> 
```

Alternatively, use `--use-wallet` to [sign via browser wallet](securely-sign-transactions-from-cli.md) (PolkadotJS, Talisman, SubWallet) instead of exposing private keys.

### Gas

When deploying a smart contract you need to know how much [gas](https://use.ink/basics/gas) is needed so you can pay accordingly. To find an estimate of how much gas you will need, you can do a "dry-run" of the contract:

```
pop up --constructor new --args false --suri //Alice --dry-run
```

This will perform a dry-run via an RPC call to estimate the gas usage. It does not submit a transaction.

```
┌   Pop CLI : Deploy a smart contract
│
◇  Gas limit estimate: Weight { ref_time: 146346224, proof_size: 16689 }
```

You can now see the estimate and make sure your account is properly funded with that amount.

> It is also possible to **only** upload the contract and **not** instantiate by adding `--upload-only`. More on this check out the[ ink! docs](https://use.ink/docs/v6/getting-started/deploy-your-contract).

**Need help?**

Ask on [Polkadot Stack Exchange](https://polkadot.stackexchange.com/) (tag it [`pop`](https://substrate.stackexchange.com/tags/pop/info)) or drop by [our Telegram](https://t.me/onpopio). We're here to help!
