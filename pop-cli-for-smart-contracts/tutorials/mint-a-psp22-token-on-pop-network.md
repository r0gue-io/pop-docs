---
description: This tutorial will teach you how to mint a PSP22 token on Pop Network
hidden: true
---

# Mint a PSP22 token on Pop Network

## Introduction

PSP stands for Polkadot Smart Contract Proposal. These are specifications for Polkadot smart contracts. More info can be found [here](https://github.com/inkdevhub/standards).

In this tutorial, we will use an ink! smart contract that implements the [PSP22](https://github.com/inkdevhub/standards/blob/master/PSPs/psp-22.md) fungible token standard for Polkadot.

## Get Started

Let's start!

```
pop new contract
```

```
┌   Pop CLI : Generate a contract
│
◇  Select a template type: 
│  PSP 
│
◇  Select the contract:
│  Psp22 
│
◆  Where should your project be created?
│  ./my_psp_token 
└  
```

```
cd my_psp_token
pop build
```

### Create the PSP22 token

In the next command, we will deploy and create a PSP22 token with the following params:

**`Name`**`: AWESOME`

**`Symbol`**`: AWE`

**`Decimals`**`: 10`

**`Supply`**`: 10_000_000`

\
The following command will spin up a local blockchain, deploy the PSP22 smart contract to the blockchain, and instantiate it with the above parameters.

```
pop up contract --constructor new --args 10000000, 'Some("AWESOME")', 'Some("AWE")', 10
```

```
┌   Pop CLI : Deploy a smart contract
│
◇  Gas limit estimate: Weight { ref_time: 742850263, proof_size: 24819 }
│
◇  Contract deployed and instantiated: The Contract Address is "5EmcjhRR4MznE9quijW4vvNZXzhgYNWVE4BiT2M5jTxjcfdt"
│
└  🚀 Deployment complete
```

Awesome! The contract has been deployed. This means your token has been created!



**Let's check by doing a few RPC calls.**

Check the token name:

```
pop call contract --contract 5EmcjhRR4MznE9quijW4vvNZXzhgYNWVE4BiT2M5jTxjcfdt --message PSP22Metadata::token_name
```

The above call should return "AWESOME" as the token name.



Check the total supply:

```
pop call contract --contract 5EmcjhRR4MznE9quijW4vvNZXzhgYNWVE4BiT2M5jTxjcfdt --message PSP22::total_supply
```

The above should return a total supply of 10\_000\_000



Let's transfer 1\_000 AWE tokens from the owner (Alice) to Bob:

```
pop call contract --contract 5EmcjhRR4MznE9quijW4vvNZXzhgYNWVE4BiT2M5jTxjcfdt --message PSP22::transfer --args 5FHneW46xGXgs5mUiveU4sbTyGBzmstUspZC92UhjJM694ty 1000 '[]' --execute
```

Let's confirm that Bob now has 1\_000 AWE tokens:

```
pop call contract --contract 5EmcjhRR4MznE9quijW4vvNZXzhgYNWVE4BiT2M5jTxjcfdt --message PSP22::balance_of --args 5FHneW46xGXgs5mUiveU4sbTyGBzmstUspZC92UhjJM694ty
```



Congrats! You have successfully used a PSP22 contract to mint a token and call its transfer message.
