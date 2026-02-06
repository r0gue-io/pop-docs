---
description: Deploy a chain using the Polkadot Deployment Portal (PDP).
---

# Deploy with Polkadot Deployment Portal

Use the Polkadot Deployment Portal (PDP) to deploy a chain with managed collators and a UI for monitoring.

## Prerequisites

- A chain project with genesis artifacts.
- Access to the Polkadot Deployment Portal.
- A PDP API key.

If you need to generate artifacts or register a chain manually, start with [Deploy a chain to Paseo](launch-a-chain-to-paseo.md).

## Get access and an API key

Request beta access and an API key:

- https://docs.google.com/forms/d/1th3GKJCSjzrmqwzDs62yA1hGUZnQUCPqmaUYLwSiHo4/viewform?edit_requested=true
- https://www.deploypolkadot.xyz/

> During the beta, use the staging portal: https://staging.deploypolkadot.xyz/

## Start the deployment

Run `pop up` in your chain project and select the PDP provider when prompted. Pop CLI reads `PDP_API_KEY` if it is set.

```shell
pop up
```

Pop CLI registers your chain when needed, then hands off deployment to PDP. After the flow completes, Pop CLI prints the portal URL where you can monitor the deployment.

## Pure proxy option

PDP can use a pure proxy account for registration. A pure proxy has no private keys and is controlled by a proxy account.

Use a `ParaRegistration` proxy type when possible to limit permissions to para ID reservation and registration.

If you need to create a pure proxy account, use `pop call` and fund it before running `pop up`.

## Deterministic runtime builds

PDP deployments support deterministic runtime builds. This requires Docker or Podman so Pop CLI can run `srtool`.

For the full flow, see [Build your runtime deterministically](../build-deterministic-runtime.md).

## Resources

#### Learning Resources

* 🧑‍🏫 [https://docs.polkadot.com/develop/parachains/deployment/](https://docs.polkadot.com/develop/parachains/deployment/)
  * ⭕ Learn more about deterministic runtimes [here](https://docs.polkadot.com/develop/parachains/deployment/build-deterministic-runtime/).
* 🧑‍🔧 Learn more about [Pure Proxies Accounts](https://wiki.polkadot.network/docs/learn-proxies-pure).
* [Polkadot Deployment Portal Documentation](https://www.deploypolkadot.xyz/docs).

**Need help?**

Ask on [Polkadot Stack Exchange](https://polkadot.stackexchange.com/) (tag it [`pop`](https://substrate.stackexchange.com/tags/pop/info)) or drop by [our Telegram](https://t.me/onpopio). We're here to help!
