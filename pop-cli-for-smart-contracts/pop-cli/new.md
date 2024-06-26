---
description: Generate a new smart contract
---

# new

There are different ways of generating a smart contract with Pop CLI.

The recommended way is the **interactive** **way**:

```
pop new contract
```

```
┌   Pop CLI : Generate a contract
│
◇  Name of your contract?
│  my_contract
│
◇  Where should your project be created?
│  ./
│
◆  Select a contract type: 
│  ● Examples (Contract examples for ink!. 1 available option(s))
│  ○ ERC 
└  
```

You can also create a contract **manually**:

```
pop new contract my_contract --contract-type erc --template erc721
```

Note that the template has to be part of a specific contract type.



Additional options:

```
pop new contract --help

Generate a new smart contract

Usage: pop new contract [OPTIONS] [NAME]

Arguments:
  [NAME]  Name of the contract

Options:
  -c, --contract-type <CONTRACT_TYPE>  Contract type. [default: examples] [possible values: examples, erc]
  -p, --path <PATH>                    Path for the contract project, [default: current directory]
  -t, --template <TEMPLATE>            Template to use. [possible values: standard, erc20, erc721, erc1155]
```
