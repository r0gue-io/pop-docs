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

**`Supply`**`: 10000000`

\
The following command will spin up a local blockchain, deploy the PSP22 smart contract to the blockchain, and instantiate it with the above parameters.

```
pop up contract --constructor new --args 10000000, 'Some("AWESOME")', 'Some("AWE")', 10
```

Let's make sure the token exists:

