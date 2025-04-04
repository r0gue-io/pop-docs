---
description: Build your parachain
---

# build

```bash
pop build <COMMAND>
```

{% hint style="info" %}
For Pop CLI versions <`0.3.0` the `pop build` command is `pop build parachain`
{% endhint %}

To build your parachain using Pop CLI:

```bash
# Build your parachain
pop build -p ./my-appchain
```

or

```bash
cd my-appchain
pop build
```

To build your parachain for production:

```bash
cd my-appchain
pop build --release
```

To build your chain for benchmarking, run the below command which will build your binary with the `runtime-benchmarks` feature enabled:

```bash
cd my-appchain
pop build --benchmark
```

To build your chain for runtime testing, run the below command which will build your binary with the `try-runtime` feature enabled:

```bash
cd my-appchain
pop build --try-runtime
```

On the other hand, you can also provide a list of features with the `--features` flag:

```bash
cd my-appchain
pop build --features runtime-benchmarks,try-runtime
```

Build and generate files for onboarding the parachain to the Relay chain (Pop CLI versions >`.0.2.0`):&#x20;

```
pop build -p ../my-appchain --para_id 2000
```

This command will build your parachain and generate the chain spec, WebAssembly runtime for the parachain, and generate the parachain genesis state needed for registering and onboarding a parachain onto the Relay chain.



## build spec

```
pop build spec [OPTIONS]
```

To build the chain specification for your appchain along with its genesis artifacts, you can run the following command:

```
cd my-appchain
pop build spec --id 3000
```

For additional parameters and customizations run:

```
pop build spec --help
```
