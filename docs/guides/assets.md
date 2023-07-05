# Asset Onramp Guide

This page explains how to get assets onto Interlay.

?> If you are a developer and looking for the list of supported assets and their IDs, please go [check out the API docs](/developers/api).

## iBTC

**What is it?**

iBTC is a fully decentralized, trustless and 1:1 Bitcoin backed asset.

**Use cases:** 

Used in many DeFi applications. On Interlay it can be used for lending and borrowing, for swaps and other products coming soon.

**How to get iBTC**

* **From Bitcoin:** If you already have BTC on the bitcoin chain, you can use the [Interlay bridge](https://app.interlay.io/bridge) to mint iBTC directly on Interlay. For an extensive guide on how to mint iBTC please see the [docs](/guides/bridge).
* **From Exchanges**: When minting iBTC you can deposit BTC from a centralized exchange. *Careful: pay close attention to the exact amounts the exchange will send and compare to the amount of BTC you need to deposit into the Vault! These need to be exactly the same for automatic processing, otherwise you will need to manually finalize the mint request via the bridge UI.*
* **From other parachains:** If you already have iBTC on other parachains, use the [bridge tab](https://app.interlay.io/transfer) on the app to bridge it to Interlay. iBTC is also available on decentralized exchanges such as: [Pulsar](https://app.stellaswap.com/exchange/swap), [HydraDX](https://app.hydradx.io/#/trade), [Acala](https://apps.acala.network/swap), [Arthswap](https://app.arthswap.org/#/swap), [Parallel](https://app.parallel.fi/swap)
* **From other chains (Ethereum, Solana, Cosmos,..)**: if you are coming from other networks, please see our cross-chain onramp guide below.

## DOT

**What is it?** 

DOT is the native asset of the Polkadot relay chain.

**Use cases** 

Used in many applications in the ecosystem, such as on-chain governance or DeFi. On Interlay it can be used for lending and borrowing, for swaps, or as vault collateral to secure the iBTC bridge.

**How to get DOT**

* **From centralised exchanges**: Withdraw DOT to Polkadot, then use the [bridge tab](https://app.interlay.io/transfer) on the app to bridge it to Interlay. [List of supported exchanges](https://coinmarketcap.com/currencies/polkadot-new/markets/).
* **From other parachains**: If you already have DOT on a parachain, it first needs to be bridged to the Polkadot relay chain using the cross-chain transfer/xcm-bridging feature of the respective parachain. Once the DOT has been transferred to the relay chain, use the [bridge tab](https://app.interlay.io/transfer) on the app to bridge it to Interlay. 
* **From other chains (Ethereum, Solana, Cosmos,..)**: If you are coming from other networks, please see our cross-chain onramp guide below.
* **From USD/Bank/Credit card**: You can use [Banxa](https://talisman.banxa.com/?coinType=DOT&fiatType=EUR) to buy DOT. You will receive it on the Polkadot relay chain and can then use the [bridge tab](https://app.interlay.io/transfer) to bridge it to Interlay.

## INTR

**What is it?**

INTR is the native asset of the Interlay parachain.

**Use cases:** 

Used for on-chain governance, to pay for transaction fees or DeFi applications. 

**How to get INTR**

* **From centralised exchanges:** INTR can be bought on [supported exchanges](https://coinmarketcap.com/currencies/interlay-intr/markets/) and directly withdrawn to Interlay by using the exchange’s withdrawal feature.
* **From other parachains:** If you already have INTR on other parachains, use the [bridge tab](https://app.interlay.io/transfer) on the app to bridge it to Interlay. INTR is also available on decentralized exchanges such as: [Pulsar](https://app.stellaswap.com/exchange/swap), [Acala](https://apps.acala.network/swap), [Arthswap](https://app.arthswap.org/#/swap), [Parallel](https://app.parallel.fi/swap)
* **From other chains (Ethereum, Solana, Cosmos,..)**: If you are coming from other networks, please see our cross-chain onramp guide below.
* **From USD/Bank/Credit card (Coming soon)**: Soon will you be able to use [Banxa](https://talisman.banxa.com/?coinType=DOT&fiatType=EUR) to buy INTR. You will receive the purchased tokens directly on the Interlay chain.

## USDT

**What is it?**

USDT is a USD pegged stable coin issued by Tether.

**Use cases:** 

Used in many DeFi applications. On Interlay it can be used for lending and borrowing, for swaps, or as vault collateral to secure the iBTC bridge.

**How to get USDT**

* **From centralised exchanges:** USDT can be bought on [supported exchanges](https://coinmarketcap.com/currencies/tether/markets/) and directly withdrawn to Statemint (soon to be renamed to "Polkadot Asset Hub") from [Binance and Bitfinex](https://support.polkadot.network/support/solutions/articles/65000181634-how-to-withdraw-usdt-from-bitfinex-on-statemine). Once you have USDT on Statemint, use the [bridge tab](https://app.interlay.io/transfer) in the transfer app to bridge it to Interlay.

!> You will need some DOT on Asset Hub to pay for fees. You can send DOT from the Polkadot Relay Chain to Asset Hub using [Talisman]( https://app.talisman.xyz/transfer/transport) or [Polkadot.js Teleport](https://support.polkadot.network/support/solutions/articles/65000181119-polkadot-js-ui-how-to-teleport-dot-or-ksm-to-asset-hub). This is a temporary UX issue and will be resolved shortly. 

* **From other parachains:** If you already have USDT on other parachains, it first needs to be bridged to the Statemint/Polkadot Asset Hub using the cross-chain transfer/xcm-bridging feature of the respective parachain. Once the USDT has been transferred to Statemint/Polkadot Asset Hub, use the [bridge tab](https://app.interlay.io/transfer) on the app to bridge it to Interlay. 
* **From other chains (Ethereum, Solana, Cosmos,..)**: if you are coming from other networks, please see our cross-chain onramp guide below.




## Cross-Chain On-Ramp Guide

If you have assets on other chains, such as Ethereum, you can use existing crypto-crypto onramps.

?> Attention: Mentions of 3rd party platforms below are **not** endorsements and you should due proper due diligence before using any of these products.


### Swap for BTC and mint IBTC

One of the easiest ways to get your assets into Interlay is to swap them for BTC and then mint iBTC.

Here are some platforms that support crypto-BTC swaps.
- [Thorchain](https://thorchain.org/swap)
- [Shapeshift](https://shapeshift.com/)
- [Sideshift.ai](https://sideshift.ai/eth/btc)
- [Changelly](https://shapeshift.com/)

### Bridge and Swap on a Substrate DEX

Another option is to bridge assets like ETH, wBTC via bridges like Wormhole and swap them on a Substrate-based DEX that is connected to Interlay.

- [HydraDX](https://app.hydradx.io/trade) via [Wormhole's Portal Bridge](https://www.portalbridge.com/#/transfer). Supports ETH, DAI and wBTC.
- [Stellaswap](https://app.stellaswap.com/bridge) via Wormhole (routed through Moonbeam). Supports ETH, wBTC and BUSD.