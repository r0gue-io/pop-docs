---
description: This guide will teach how to bridge tokens from Paseo (Asset Hub) to Pop
---

# Get tokens on Pop Network

## Important note

Pop Network is deprioritised and will not be deployed to Polkadot. Testnet will be sunsetted Decemeber 31 2025. More information on this decision can be found [here](https://forum.polkadot.network/t/final-update-pop-network-treasury-proposal-683/14663).

## Introduction

Pop uses the Polkadot Relay chain's native token: **DOT**.&#x20;

Paseo is Polkadot's Testnet. Its relay chain's native token is **PAS**.

This guide will teach you how to bridge **PAS** tokens from the Paseo's Asset Hub to Pop.

### Setup Polkadot Account <a href="#setup-polkadot-account" id="setup-polkadot-account"></a>

Make sure you have a Polkadot account. You can use one of the following recommended wallets:

* [PolkadotJs Signer Extension](https://polkadot.js.org/extension/)
* [Talisman Wallet](https://www.talisman.xyz/)
* [Nova Wallet](https://novawallet.io/)
* [SubWallet](https://www.subwallet.app/)

### Paseo Faucet <a href="#paseo-faucet" id="paseo-faucet"></a>

Go to the [Paseo Faucet](https://faucet.polkadot.io/) and request some PAS tokens. Make sure:

* "Network" is "Paseo"
* "Chain" is "AssetHub"

<figure><img src="../.gitbook/assets/Screenshot 2025-03-18 at 3.03.51 PM.png" alt=""><figcaption><p>Paseo Faucet</p></figcaption></figure>

You should now see 100 PAS tokens in your account on Paseo:'s Asset Hub. [Go here to check for yourself!](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Fasset-hub-paseo.dotters.network#/accounts)



<figure><img src="../.gitbook/assets/Screenshot 2025-03-18 at 3.11.11 PM.png" alt=""><figcaption><p>Account Balance on Paseo's Asset Hub</p></figcaption></figure>

### Bridging from Paseo to Pop <a href="#bridging-from-paseo-to-pop-network" id="bridging-from-paseo-to-pop-network"></a>

You can use the Pop Onboarding UI to bridge tokens from Paseo to Pop: [https://onpop.io/network/onboard](https://onpop.io/network/onboard)

<figure><img src="../.gitbook/assets/Screenshot 2025-03-18 at 3.07.43 PM.png" alt=""><figcaption><p>Transfer PAS from Paseo's Asset Hub to Pop</p></figcaption></figure>

The token amount (minus the tx fee) will appear on Pop:

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption><p>Pop Network</p></figcaption></figure>

Congrats! You have successfully bridged tokens from Paseo to Pop!
