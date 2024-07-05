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

{% hint style="info" %}
Pop CLI versions > `0.2.0` will support a simplified command for building contracts.

Simply: `pop build` inside the parachain directory to build the parachain or specify the project path: `pop build --path ./my-appchain`
{% endhint %}
