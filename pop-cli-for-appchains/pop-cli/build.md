---
description: Build your parachain
---

# build

```bash
pop build <COMMAND>
```

To build your parachain using Pop CLI:

```bash
# Build your parachain
pop build -p ./my-appchain
```

or

```bash
cd my-app
pop build
```

To build your parachain for production:

```bash
cd my-app
pop build --release
```

{% hint style="info" %}
Pop CLI versions > `0.2.0` will support a simplified command for building parachains.

Simply: `pop build` inside the parachain directory to build the parachain or specify the project path: `pop build --path ./my-appchain`
{% endhint %}

Build and generate files for onboarding the parachain to the Relay chain (Pop CLI versions >`.0.2.0`):&#x20;

```
pop build -p ../my-appchain --para_id 2000
```

This command will build your parachain and generate the chain spec, WebAssembly runtime for the parachain, and generate the parachain genesis state needed for registering and onboarding a parachain onto the Relay chain.
