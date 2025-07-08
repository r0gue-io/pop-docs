---
description: The following guide shows how to build deterministic runtimes.
---

# Build your runtime deterministically

By default, the Rust compiler generates optimized Wasm binaries, but they aren't always deterministically reproducible. If the Wasm runtime isn't deterministic, each build might produce slightly different bytecode, This can be a problem for blockchain networks where every node must run the exact same runtime.

To ensure deterministic Substrate runtime builds, Pop CLI integrates [SRTool (Substrate Runtime Toolbox)](https://github.com/paritytech/srtool). `SRTool` guarantees that every runtime build produces identical Wasm bytecode, making it reliable for production use. This build requires [Docker](https://www.docker.com/) or [Podman](https://podman.io/) to be installed and running. Pop CLI automatically invokes the `SRTool` image to generate a reproducible and verifiable runtime.

This guide provides a quick overview of the importance of deterministic builds and how to achieve them using Pop CLI:

## Build a deterministic runtime with Pop CLI

While generating your chain spec with `pop build spec`, Pop CLI also gives you the option to build your runtime deterministically and inject it automatically.

Once you select the option to build deterministically, Pop CLI will prompt you to provide the runtime path (by default it's typically located in the runtime/ folder). After confirming, the deterministic build process will begin.

> ⏳ The build may take 10–15 minutes or more, depending on your setup, so be patient!

Once the build is complete, Pop CLI will automatically inject the resulting runtime code into your generated chain spec.

```
◇  Would you like to build the runtime deterministically? This requires a containerization solution (Docker/Podman) and is recommended for production builds.
│  Yes 
│
◆  Enter the directory path where the runtime is located:
│  ./runtime/mainnet 
└ 

◒  Building deterministic runtime...                    
│  ▲  WARNING: You are using docker. It is recommend to use podman instead.
│  
◓  NOTE: This process may take longer than 10-15 minutes. Please be patient...  
```

If you'd prefer not to be prompted interactively, you can specify everything up front via command line:

```
pop build spec --deterministic --runtime ./runtime/mainnet
```

## Resources

#### Learning Resources

* 🧑‍🏫 [Build a Deterministic Runtime](https://docs.polkadot.com/develop/parachains/deployment/build-deterministic-runtime/) in the Polkadot docs is a good starting point.
* 🧑‍🔧 For technical introduction, [srtool repository](https://github.com/paritytech/srtool).

**Need help?**

Ask on [Polkadot Stack Exchange](https://polkadot.stackexchange.com/) (tag it [`pop`](https://substrate.stackexchange.com/tags/pop/info)) or drop by [our Telegram](https://t.me/onpopio). We're here to help!
