# Call

Now that your contract is deployed, we can interact with it.

### Interactive Guidance (Recommended)

Interact with your deployed contract **with** Pop CLI's interactive guidance by simply entering:

```shell
pop call
```

Grab the contract address that was outputted from the [previous step](broken-reference).

> If you lost the address, you can always pull up [PolkadotJs Apps](https://polkadot.js.org/apps/) and check the chain state for the contract address.

You will be prompted to select the path to your contract, choose which contract message to call, and provide the arguments for the message.

### Manual (non-interactive)

If you prefer not to use the interactive prompts, you can call your contract by specifying all the required arguments directly.

```shell
pop call contract -p ./flipper --contract 5CLPm1CeUvJhZ8GCDZCR7nWZ2m3XXe4X5MtAQK69zEjut36A --message get --suri //Alice
```

> **⚠️ Note:** If you're using ink! v6, the contract address displayed here will appear as a 20-byte hash.

```
┌   Pop CLI : Calling a contract
│
◐  Calling the contract...                                                                                                                   
⚙  Result: Ok(false)
│  
▲  Your call has not been executed.
│  
▲  To submit the transaction and execute the call on chain, add -x/--execute flag to the command.
│  
└  Call completed successfully!
```

The result here is `false` meaning that `value` in the contract storage is set to `false`.

Let's try _flipping_ it. Remember to update the contract address accordingly.

```shell
pop call contract -p ./flipper --contract 5CLPm1CeUvJhZ8GCDZCR7nWZ2m3XXe4X5MtAQK69zEjut36A --message flip --suri //Alice -x
```

> **Gas Estimate**
>
> Notice the `-x` flag at the end of the `pop call` command. This is needed because we are executing a transaction on the network, and it will cost gas.
>
> Typically it is useful for you to estimate how much gas the call will cost. Fortunately you can do this with the `--dry-run` flag:
>
> ```
> pop call contract -p ./flipper --contract 5CLPm1CeUvJhZ8GCDZCR7nWZ2m3XXe4X5MtAQK69zEjut36A --message flip --suri //Alice -x --dry-run
> ```

```
┌   Pop CLI : Calling a contract
│
◐  Doing a dry run to estimate the gas...                                                                                                    
⚙  Gas limit Weight { ref_time: 382198493, proof_size: 18636 }
│  
◓  Calling the contract...                                                                                                                   
⚙        Events
│         Event Balances ➜ Withdraw
│           who: 5GrwvaEF5zXb26Fz9rcQpDWS57CtERHpNehXCPcNoHGKutQY
│           amount: 1.550397502mUNIT
│         Event Contracts ➜ Called
│           caller: Signed(5GrwvaEF5zXb26Fz9rcQpDWS57CtERHpNehXCPcNoHGKutQY)
│           contract: 5CLPm1CeUvJhZ8GCDZCR7nWZ2m3XXe4X5MtAQK69zEjut36A
│         Event TransactionPayment ➜ TransactionFeePaid
│           who: 5GrwvaEF5zXb26Fz9rcQpDWS57CtERHpNehXCPcNoHGKutQY
│           actual_fee: 1.550397502mUNIT
│           tip: 0UNIT
│         Event System ➜ ExtrinsicSuccess
│           dispatch_info: DispatchInfo { weight: Weight { ref_time: 1302708493, proof_size: 9064 }, class: Normal, pays_fee: Yes }
│  
└  Call completed successfully!
```

Did it work? See if the value has been flipped by calling the contract again. Remember to update the contract address accordingly.

```shell
pop call contract -p ./flipper --contract 5CLPm1CeUvJhZ8GCDZCR7nWZ2m3XXe4X5MtAQK69zEjut36A --message get --suri //Alice
```

```
┌   Pop CLI : Calling a contract
│
◐  Calling the contract...                                                                                                          
⚙  Result: Ok(true)
│  
▲  Your call has not been executed.
│  
▲  To submit the transaction and execute the call on chain, add -x/--execute flag to the command.
│  
└  Call completed successfully!
```

The `value` is now `true`.

Awesome, you now know how to make calls to your smart contract.

\
**Need help?**

Ask on [Polkadot Stack Exchange](https://polkadot.stackexchange.com/) (tag it [`pop`](https://substrate.stackexchange.com/tags/pop/info)) or drop by [our Telegram](https://t.me/onpopio). We're here to help!
