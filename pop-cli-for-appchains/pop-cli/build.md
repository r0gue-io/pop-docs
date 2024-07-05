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
pop build parachain -p ./my-appchain
```

or

```bash
cd my-app
pop build parachain
```

To build your parachain for production:

```bash
cd my-app
pop build parachain --release
```

{% hint style="info" %}
Pop CLI versions > `0.2.0` will support a simplified command for building parachains.

Simply: `pop build` inside the parachain directory to build the parachain or specify the project path: `pop build --path ./my-appchain`
{% endhint %}
