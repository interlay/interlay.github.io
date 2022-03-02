# Redeem kBTC/interBTC (Testnet)

interBTC can be redeemed at any point in time for BTC on the Bitcoin blockchain. To receive BTC for your existing interBTC, follow this guide.

At the end of this guide you will have:

- [x] Redeemed your first interBTC from the interBTC app
- [X] Received BTC for the redeemed interBTC in your Bitcoin wallet

## Video Guide (OLD UI - New Guide is WIP)

<iframe width="560" height="315" src="https://www.youtube.com/embed/-TZ2XUmXh9I" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Prerequisites

- Make sure you have the required [polkadot-js extension and a Bitcoin wallet](start/prereq.md).
- Make sure you [have interBTC in your wallet](guides/issue.md)

## Redeem interBTC

### 1. Go to [ testnet.interlay.io](https://testnet.interlay.io)

The app has 3 tabs: Issue, Redeem, and Transfer. Ensure you are on the Redeem tab.

### 2. Get testnet DOT via the Faucet

Get some testnet DOT via the faucet with the "Request DOT" button, right-hand side of top bar, before making a redeem request. You will need this to pay for the bridge transaction fees.

### 3. Enter the amount of interBTC you want to redeem and the BTC address you want to receive your BTC to

Enter the amount of interBTC you want to redeem, and the Bitcoin address where you want to receive the redeemed Bitcoin amount. Supported address types: [P2SH](https://en.bitcoin.it/wiki/P2SH), [P2PKH](https://en.bitcoin.it/wiki/P2PKH) and [P2WPKH](https://wiki.trezor.io/P2WPKH).

Check the bridge fee that is subtracted from your redeemed amount and click **"Confirm"**. Sign the transaction via the `polkadot-js` extension when asked and wait a few moments.

### 4. Wait for confirmation of your BTC transaction and receive interBTC automatically

The Redeem request is now being processed by the Vault. Wait for a few minutes (might take upt to 24 hours) and you will receive your Bitcoin at the address you specified.

### 5. Optional: Check the status of your redeem request

You can check the status of your redeem request in the [Transactions](https://testnet.interlay.io/transactions) view in the **"Redeem Requests"** table. Note that Vaults have 24 hours to complete your request.

## Questions?

- Checkout out the [FAQ](https://www.notion.so/interlay/Interlay-FAQ-5e3019b1cfd94f6693dc186e9640e607#277286bac5224dbbab565af4fe1ec5d5)
- Join our [Discord](https://discord.com/invite/KgCYK3MKSf)
