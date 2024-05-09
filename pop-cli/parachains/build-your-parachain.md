# Build your parachain

To build your parachain using Pop CLI

```shell
cd my-chain
pop build parachain
```

```
┌   Pop CLI : Building a parachain
│
   Compiling parachain-template-runtime v0.1.0 (/Users/pop/src/my-chain/runtime)
   Compiling parachain-template-node v0.1.0 (/Users/pop/src/my-chain/node)
    Finished release [optimized] target(s) in 1m 20s

└  Build Completed Successfully!
```

If you are outside the project's directory, you can specify the path

```shell
pop build parachain -p ./my-chain
```