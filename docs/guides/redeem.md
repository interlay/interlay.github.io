# Redeem interBTC

interBTC can be redeemed at any point in time for BTC on the Bitcoin blockchain. To receive BTC for your existing interBTC, follow this guide.

At the end of this guide you will have:

- [x] Redeemed your first interBTC from the interBTC app
- [X] Received BTC for the redeemed interBTC in your Bitcoin wallet

## Video Guide (OLD UI - New Guide is WIP)

<iframe width="560" height="315" src="https://www.youtube.com/embed/-TZ2XUmXh9I" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Prerequisites

?> You can skip these prerequisites if you have successfully completed issuing interBTC.

Make sure you have the required [polkadot-js extension and a Bitcoin wallet](start/prereq.md).

## Redeem interBTC

### 1. Go to [ bridge.interlay.io](https://bridge.interlay.io)

The app has 3 tabs: Issue, Redeem, and Transfer. Ensure you are on the Redeem tab.

?> If you have any historic or pending Redeem requests, they are displayed in a table below the modal. Clicking on a row of this table will open a pop-up with more details about the request.

### 2. Fill in the details of your Redeem request

Enter the amount of interBTC you want to redeem, and the Bitcoin address where you want to receive the redeemed Bitcoin amount. Supported address types: [P2SH](https://en.bitcoin.it/wiki/P2SH), [P2PKH](https://en.bitcoin.it/wiki/P2PKH) and [P2WPKH](https://wiki.trezor.io/P2WPKH).

?> Ensure you have some testnet DOT before making a redeem request. If you haven't already, you can request some via the faucet ("Request DOT" button, right-hand side of top bar). You will need this to pay for parachain transaction fees.

Check the bridge fee that will be subtracted from your redeemed amount and click **"Confirm"**. Sign the transaction via the `polkadot-js` extension when asked and wait a few moments.

The Polkadot address of the vault assigned to fulfill this request will be displayed.

?> The maximum amount of interBTC that you can redeem in a single request is limited by the maximum amount of BTC locked in a single vault. High-value Redeem requests, executed with multiple vaults simultaneously, will be added as a feature before mainnet launch.

### 3. Confirm your request was successful

The Redeem request is now being processed by the vault. Wait for a few minutes and you will receive your Bitcoin at the address you specified. You can see more details about your redeem request by clicking on it in the  **"Redeem Requests"** table below the app modal.
