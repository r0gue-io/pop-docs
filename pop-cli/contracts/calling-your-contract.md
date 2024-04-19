# Calling your contract

Now that your contract is deployed locally, we can start interacting with it.

Grab the contract address that was outputted from the [previous step](deploy-your-contract-locally.md).

> If you lost the address, you can always pull up PolkadotJs Apps and check the chain state for the contract address.

```
pop call contract -p ./flipper --contract 5CLPm1CeUvJhZ8GCDZCR7nWZ2m3XXe4X5MtAQK69zEjut36A --message get --suri //Alice --url ws://127.0.0.1:56545
```

```
┌   Pop CLI : Calling a contract
│
◐  Calling the contract...                                                                                                                   ⚙  Result: Ok(false)
│  
▲  Your call has not been executed.
│  
▲  To submit the transaction and execute the call on chain, add -x/--execute flag to the command.
│  
└  Call completed successfully!
```

The result here is `false` meaning that `value` in the flipper storage is set to `false`.

Let's try _flipping_ it.

```
pop call contract -p ./flipper --contract 5CLPm1CeUvJhZ8GCDZCR7nWZ2m3XXe4X5MtAQK69zEjut36A --message flip --suri //Alice --url ws://127.0.0.1:56545 -x
```

```
┌   Pop CLI : Calling a contract
│
◐  Doing a dry run to estimate the gas...                                                                                                    ⚙  Gas limit Weight { ref_time: 382198493, proof_size: 18636 }
│  
◓  Calling the contract...                                                                                                                   ⚙        Events
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

> Notice the `-x` flag at the end of the `pop call` command. This is needed because we are executing a transaction on the network and it will cost gas.

Did it work? See if the value has been flipped.

```
pop call contract -p ./flipper --contract 5CLPm1CeUvJhZ8GCDZCR7nWZ2m3XXe4X5MtAQK69zEjut36A --message get --suri //Alice --url ws://127.0.0.1:56545
```

```
┌   Pop CLI : Calling a contract
│
◐  Calling the contract...                                                                                                                   ⚙  Result: Ok(true)
│  
▲  Your call has not been executed.
│  
▲  To submit the transaction and execute the call on chain, add -x/--execute flag to the command.
│  
└  Call completed successfully!
```

The `value` is now `true`.

Awesome you now know how to make calls to your smart contract.
