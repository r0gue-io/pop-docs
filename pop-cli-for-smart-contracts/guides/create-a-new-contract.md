# Create a new contract

To create a smart contract with Pop CLI:

```
pop new contract
```

The above command will guide you to create a new smart contract:

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
│  ○ Examples 
│  ● ERC (ERC-based contracts in ink!. 3 available option(s))
└  
```

Notice that there are several templates available.&#x20;

To get a full list of available templates run `pop new contract --help`

```shell

pop new contract flipper
```

> Note: 'flipper' has been used as the name above as the default template is based on the `flipper` contract example. Additional contract templates will be added in the next release!

You should get output like the following:

```
┌   Pop CLI : Generating new contract "flipper"!
│
◇  Smart contract created! Located in the following directory "/flipper"
│
└  cd into "flipper" and enjoy hacking! 🚀
```

You can now `cd` into the folder and start hacking on your contract:

```shell
cd flipper
```

You should see the directory structure of your ink! smart contract:

```
flipper
  └─ lib.rs
  └─ Cargo.toml
  └─ .gitignore
```

**Further Reading Material**

* [https://use.ink/basics/contract-template](https://use.ink/basics/contract-template)
