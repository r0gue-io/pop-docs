---
description:
  This guide explains how to deploy a chain using an external provider.
---

# Deploy a Chain Using an External Provider

Pop CLI integrates an external provider for chain deployment, streamlining the process with seamless automation. It handles registration, as detailed in [Launch a Chain to Paseo](./launch-a-chain-to-paseo.md), and once the chain is registered, Pop CLI completes the deployment by integrating with the provider. The provider manages collators and offers a portal UI for monitoring your chain’s status.

Currently, the only supported deployment provider is the [Polkadot Development Portal](https://www.deploypolkadot.xyz/), which supports a limited set of templates.

Execute the following command to start the interactive deployment process:
```shell
pop up
```
And follow the interactive guide 

<figure><img src="../.gitbook/assets/pdpflow.png" alt="pop up"><figcaption><p>pop up</p></figcaption></figure>

At the end of the process, Pop CLI will display the URL to the external provider's portal, where you can monitor the status of your deployment.

<figure><img src="../.gitbook/assets/pdpui.png" alt="polkadot development portal"><figcaption><p>Polkadot Development Portal</p></figcaption></figure>

During the process, Pop CLI will prompt you for two important choices:

1. Whether to use a pure proxy for registration.
2. Whether to build the runtime deterministically.

#### What is a Pure Proxy?
A pure proxy is an account without private keys, controlled entirely by a designated any proxy.

*Why Use a Pure Proxy?*

A pure proxy enhances security by keeping private keys hidden and delegating control to an any proxy. It simplifies multisig setups by allowing signatory changes without creating a new account. Highly recommended for parachain registration!

To create a pure proxy run:
```shell
pop call
```

```
┌   Pop CLI : Call a chain
│
◇  Which chain would you like to interact with?
│  wss://pas-rpc.stakeworld.io
│
◇  What would you like to do?
│  Create a pure proxy 
│
◇  Select the value for the parameter: proxy_type
│  Any 
│
◇  Enter the value for the parameter: delay
│  0
│
◇  Enter the value for the parameter: index
│  0
│
```

Once the pure proxy is created, retrieve the generated address from the event `PureCreated` and fund it using the [Paseo Faucet](https://faucet.polkadot.io/) to enable transaction execution.

<figure><img src="../.gitbook/assets/eventspureproxy.png" alt="pop up"><figcaption><p>events pop call chain</p></figcaption></figure>

#### What is a Deterministic Runtime Build?
By default, the Rust compiler generates optimized Wasm binaries, but they aren't always deterministically reproducible. If the Wasm runtime isn't deterministic, each build might produce slightly different bytecode, This can be a problem for blockchain networks where every node must run the exact same runtime.

To ensure deterministic Substrate runtime builds, Pop CLI integrates [SRTool (Substrate Runtime Toolbox)](https://github.com/paritytech/srtool). `SRTool` guarantees that every runtime build produces identical Wasm bytecode, making it reliable for production use.
This build requires [Docker](https://www.docker.com/) or [Podman](https://podman.io/) to be installed and running. Pop CLI automatically invokes the `SRTool` image to generate a reproducible and verifiable runtime.

## Resources

#### Learning Resources

* 🧑‍🏫 [https://docs.polkadot.com/develop/parachains/deployment/](https://docs.polkadot.com/develop/parachains/deployment/)
    * ⭕ Learn more about deterministic runtimes [here](https://docs.polkadot.com/develop/parachains/deployment/build-deterministic-runtime/).
* 🧑‍🔧 Learn more about [Pure Proxies Accounts](https://wiki.polkadot.network/docs/learn-proxies-pure).
* [Polkadot Deployment Portal Documentation](https://www.deploypolkadot.xyz/docs).

**Technical Support**

* [Polkadot Stack Exchange](https://polkadot.stackexchange.com/)
  * Create a question and tag it with "[`pop`](https://substrate.stackexchange.com/tags/pop/info)"
  * Share the StackExchange question in our [Pop Support Telegram channel](https://t.me/pop\_support)
