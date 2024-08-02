# Create a new contract

To create a smart contract with Pop CLI:

```
pop new contract
```

The above command will guide you through an interactive prompt to create a new smart contract:

```
┌   Pop CLI : Generate a contract
│
◆  Select a template type: 
│  ● Examples (Contract examples for ink!. 4 available option(s))
│  ○ ERC 
│  ○ PSP 
└  
```

Notice that there are several templates available.&#x20;

To get a full list of available templates run `pop new contract --help`

You can also create a new smart contract manually (without interactivity):

```shell
pop new contract my_contract
```

> Note: this will create a contract using the default template which is based on the [`flipper`](https://use.ink/basics/contract-template) contract example.

You should get output like the following:

```
┌   Pop CLI : Generating new contract "my_contract"!
│
◇  Smart contract created! Located in the following directory "/my_contract"
│
└  cd into "my_contract" and enjoy hacking! 🚀
```

You can now `cd` into the folder and start hacking on your contract:

```shell
cd my_contract
```

You should see the directory structure of your ink! smart contract:

```
my_contract
  └─ lib.rs
  └─ Cargo.toml
  └─ .gitignore
```



Several options are available when creating a smart contract manually.

To see all the available options, run `pop new contract --help`

For example:

```
pop new contract my_contract --contract-type erc --template erc721
```

**Further Reading Material**

* [https://use.ink/basics/contract-template](https://use.ink/basics/contract-template)
