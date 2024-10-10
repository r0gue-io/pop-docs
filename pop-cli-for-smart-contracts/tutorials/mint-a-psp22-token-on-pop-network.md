---
description: This tutorial will teach you how to mint a PSP22 token on Pop Network
hidden: true
---

# Mint a PSP22 token on Pop Network

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

In the next command, we will deploy and create a PSP22 token with the following params:

**Name**: AWESOME

**Symbol**: AWE

**Decimals**: 10

**Supply**: 10000000

\
The following command will spin up a local blockchain, deploy the PSP22 smart contract to the blockchain, and instantiate it with the above parameters.

```
pop up contract --constructor new --args 10000000, 'Some("AWESOME")', 'Some("AWE")', 10
```

