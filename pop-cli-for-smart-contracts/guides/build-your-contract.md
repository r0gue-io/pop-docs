# Build your contract

To build your ink! smart contract, make sure you are inside your ink! smart contract directory and run the following command:

```shell
cd flipper
pop build
```

When you run `pop build contract` the default is to build in debug mode which is faster for development. It is important to note that when your contract is ready for production you can build the contract using `pop build --release` which will create an optimized build ready for production.

You should get output like the following:

```
┌   Pop CLI : Building a contract
│
 [==] Checking clippy linting rules
   Compiling proc-macro2 v1.0.79
   Compiling unicode-ident v1.0.12
   Compiling syn v1.0.109
   Compiling equivalent v1.0.1
   Compiling hashbrown v0.14.3
   Compiling winnow v0.5.40
   ....
   Finished release [optimized] target(s) in 21.17s
   Running `target/ink/release/metadata-gen`
 [==] Generating bundle
◆  
│  Original wasm size: 21.3K, Optimized: 1.6K
│  
│  The contract was built in RELEASE mode.
│  
│  Your contract artifacts are ready. You can find them in:
│  /Users/pop/src/flipper/target/ink
│  
│    - flipper.contract (code + metadata)
│    - flipper.wasm (the contract's code)
│    - flipper.json (the contract's metadata)
│  
└  Build completed successfully!
```
