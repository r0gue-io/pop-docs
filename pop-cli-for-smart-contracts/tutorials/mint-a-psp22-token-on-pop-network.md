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

Create a PSP22 token:

```
pop up contract --constructor new --args 10000000, 'Some("AWESOME")', 'Some("AWE")', 10
```

