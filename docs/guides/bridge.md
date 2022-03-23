# kBTC/interBTC Bridge Guide

## Issue

kBTC or interBTC allows you to receive a representation of BTC to be used any way you see fit in the Polkadot ecosystem.
To get you started, follow this guide.

At the end of this guide you will have:

- [x] Locked BTC with a collateralized Vault
- [x] Issued your first kBTC/interBTC on the kBTC/interBTC app

### Video Guide (OLD UI - New Guide is WIP)

<iframe width="560" height="315" src="https://www.youtube.com/embed/hMZTj6ctGQE" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

### Prerequisites

Make sure you have the required [polkadot-js extension and a Bitcoin wallet](guides/wallets-explorers.md).

### Issue interBTC

#### 1. Go to the bridge page.

<!-- tabs:start -->

##### **Kintsugi**
[kintsugi.interlay.io/bridge](https://kintsugi.interlay.io/bridge)

##### **Interlay**

Coming soon

##### **Testnet**

[testnet.interlay.io/bridge](https://testnet.interlay.io/bridge)
<!-- tabs:end -->

The bridge has 2 tabs: Issue and Redeem. (Sometimes a third tab, Burn, will be visible.) Ensure you are on the Issue tab.

#### 2. Obtain KINT/INTR to pay the transaction fees.
You will need some of the native on-chain currency (KINT on Kintsugi, INTR on Interlay) to pay the transaction fee. Additionally, to prevent spam, you will need to place a small deposit which will be returned to you once the request has been completed.

<!-- tabs:start -->
##### **Kintsugi**
A list of exchanges with KINT listings can be found on [Coingecko](https://www.coingecko.com/en/coins/kintsugi).

##### **Interlay**
Coming soon

##### **Testnet**
On testnet, you can obtain some test KINT by clicking on the "KINT Faucet" button on the right-hand side of the sidebar.
<!-- tabs:end -->

#### 3. Enter the amount of BTC you want to bridge to Polkadot/Kusama

Enter the amount of kBTC or interBTC you want to issue. The app will automatically select a Vault for you.

Check the details of your issue request and click **"Confirm"**. Sign the transaction via the `polkadot-js` extension when asked and wait a few moments.

#### 4. Transfer BTC from your Bitcoin wallet to the Vault address

Use your Bitcoin wallet to transfer the specified `amount` to the given `address`.

?> Optional: you can use a hardware wallet

<details>
<summary>
<b>Send BTC with the Ledger wallet</b>

</summary>
<!-- tabs:start -->
##### **Kintsugi/Interlay**
On mainnet, no setup for [Ledger Live](https://www.ledger.com/ledger-live) is needed.

##### **Testnet**
On testnet, you will need to configure [Ledger Live](https://www.ledger.com/ledger-live) to work with Bitcoin testnet. Go to `Setting` > `Experimental features` and enable `Developer mode`. Using the `Manager`, install the `Bitcoin testnet` app onto your device.

<!-- tabs:end -->

Enter the recipient address or scan the QR code. ([Support](https://support.ledger.com/hc/en-us/articles/360019123593-Send-crypto-assets))

![Enter Recipient](../_assets/img/ledger/1-recipient.png)

Enter the amount - this may be auto-completed.

![Enter Amount](../_assets/img/ledger/2-amount.png)

Review the summary and click **"Continue"**.

![Summary](../_assets/img/ledger/3-summary.png)

Confirm the recipient address, amount and fees on the device.

![Confirm](../_assets/img/ledger/4-device-2.png)

The receipt will show the transaction ID, click **"View in explorer"** to check whether your transaction is included in the Bitcoin network.

![Receipt](../_assets/img/ledger/5-receipt.png)

</details>

<details>
<summary>
<b>Send BTC with the Trezor wallet</b>
</summary>

<!-- tabs:start -->
##### **Kintsugi/Interlay**
On mainnet, no setup for the [Trezor Wallet](https://wallet.trezor.io/#/) is needed.

##### **Testnet**
On testnet, you will need to configure the [Trezor Wallet](https://wallet.trezor.io/#/) to work with Bitcoin testnet. go to the `Wallet Settings` and set `Backend Server URL` to `https://tbtc2.trezor.io`.

For up-to-date details please checkout the [Trezor Wiki](https://wiki.trezor.io/Bitcoin_testnet).
<!-- tabs:end -->

![Configuration](../_assets/img/trezor/1-configuration.png)

Enter the recipient address and amount manually or scan the QR code. ([User Manual](https://wiki.trezor.io/User_manual:Making_payments#Enter_the_destination_address_and_the_amount))

![Enter Recipient & Amount](../_assets/img/trezor/2-send-testnet.png)

Confirm the recipient address, amount and fees on the device.

![Confirm](../_assets/img/trezor/3-confirm-device.png)

The payment will appear in the `Transactions` tab as unconfirmed. Once this is included in the Bitcoin network the status should update.
If configured, you may also check the status of the transaction in a block explorer.

![Receipt](../_assets/img/trezor/4-transactions.png)

</details>

#### 5. Wait for confirmation of your BTC transaction and receive kBTC/interBTC automatically

Once you've made the payment, the app will automatically locate your transaction on the Bitcoin blockchain. If this transaction is correct, you can wait for a few minutes and you will receive your interBTC: a Vault will eventually execute your request once your transaction has sufficient confirmations.

<details>
<summary>
<b>I've accidentally sent more BTC than required</b>
</summary>
If you sent more BTC than was necessary, one of two things will happen. If the vault had sufficient capacity to accomodate your larger request, it will be executed automatically, and no further action on your part is required.

However, if the vault does not have sufficient capacity, then a **Refund request** will be automatically created, giving the vault the option to return the excess BTC to you. However, since interBTC is a decentralized system, there is no way to ensure that the vault fulfils this.
</details>
<details>
<summary>
<b>I've accidentally sent less BTC than required</b>
</summary>
If you accidentally sent less BTC than was necessary, then automatic execution is disabled for security reasons. In this case, you will have to execute your request manually - see below.

You also have the options to try again to send the correct amount. Note that multiple transactions can **not** be used with a single issue request - the funds from the first transaction **will be lost**. This is useful if you accidentally sent a trace amount of Bitcoin (such as only a few Satoshi), and would rather forfeit that than have to create a new issue request.
</details>

#### 6. Optional: Manually claim your interBTC

You can check the status of your issue request in the Transactions view in the **"Issue Requests"** table.

<!-- tabs:start -->

##### **Kintsugi**
[kintsugi.interlay.io/transactions](https://kintsugi.interlay.io/transactions)

##### **Interlay**

Coming soon

##### **Testnet**

[testnet.interlay.io/transactions](https://testnet.interlay.io/transactions)
<!-- tabs:end -->

If your Bitcoin transaction has enough confirmations but has not been executed by a Vault yet, click on the issue request that is "Pending". This will open a modal, where you will see an **"Execute"** button. To finalize the Issue process and claim your kBTC/interBTC, either wait for a Vault to auto-execute your request, or click **"Execute"** yourself.


## Redeem

kBTC or interBTC can be redeemed at any point in time for BTC on the Bitcoin blockchain. To receive BTC for your existing kBTC/interBTC, follow this guide.

At the end of this guide you will have:

- [x] Redeemed your first kBTC/interBTC from the app
- [X] Received BTC for the redeemed kBTC/interBTC in your Bitcoin wallet

### Video Guide (OLD UI - New Guide is WIP)

<iframe width="560" height="315" src="https://www.youtube.com/embed/-TZ2XUmXh9I" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

### Prerequisites

- Make sure you have the required [polkadot-js extension and a Bitcoin wallet](guides/wallets-explorers.md).
- Make sure you [have kBTC or interBTC in your wallet](#issue)
- Make sure you [have KINT/INTR to pay for transaction fees](#_2-obtain-kintintr-to-pay-the-transaction-fees)

### Redeem kBTC/interBTC

#### 1. Go to the bridge page.

<!-- tabs:start -->

##### **Kintsugi**
[kintsugi.interlay.io/bridge](https://kintsugi.interlay.io/bridge)

##### **Interlay**

Coming soon

##### **Testnet**

[testnet.interlay.io/bridge](https://testnet.interlay.io/bridge)
<!-- tabs:end -->

The bridge has 2 tabs: Issue and Redeem. (Sometimes a third tab, Burn, will be visible.) Ensure you are on the Issue tab.

#### 3. Enter the amount of kBTC/interBTC you want to redeem and the BTC address you want to receive your BTC to

Enter the amount of kBTC/interBTC you want to redeem, and the Bitcoin address where you want to receive the redeemed Bitcoin amount. Supported address types are: [P2SH](https://en.bitcoin.it/wiki/P2SH), [P2PKH](https://en.bitcoin.it/wiki/P2PKH) and [P2WPKH](https://wiki.trezor.io/P2WPKH).

Check the bridge fee that is subtracted from your redeemed amount and click **"Confirm"**. Sign the transaction via the `polkadot-js` extension when asked and wait a few moments.

#### 4. Wait for confirmation of your request and receive BTC automatically

The Redeem request is now being processed by the Vault. Wait for a few minutes (might take up to 24 hours) and you will receive your Bitcoin at the address you specified.

If the Vault does not fulfil the request within 24 hours, you have the option to either reimburse your BTC or retry your request; see below.

#### 5. Optional: Retry or Reimburse your request
You can check the status of your issue request in the Transactions view in the **"Redeem Requests"** table.

<!-- tabs:start -->

##### **Kintsugi**
[kintsugi.interlay.io/transactions](https://kintsugi.interlay.io/transactions)

##### **Interlay**

Coming soon

##### **Testnet**

[testnet.interlay.io/transactions](https://testnet.interlay.io/transactions)
<!-- tabs:end -->

Vaults have 24 hours to complete your request. If it is not completed in time, you have the option to either Reimubse or Retry.

##### Reimburse
Reimbursing a redeem request that hasn't been fulfilled in time means accepting a payout in the Vault's collateral currency instead of BTC. The Vault's collateral will be slashed to the value equivalent to the BTC amount in the redeem request, plus a convenience fee. Your kBTC/interBTC will then be redeemed for this amount of collateral, rather than BTC.

Click on the redeem request that is "Pending". This will open a modal, where you will see a **"Reimburse"** button. Click on it to reimburse your request, forfeiting your BTC and receiving a greater value in collateral in return.

##### Retry
If you wish to receive BTC directly rather than any collateral currency, then you have the option to cancel the redeem request, which will give you the opportunity to open a new one. The Vault will be slashed a percentage of the request for failing to fulfil it in time, which will be transferred to you as a convenience fee; otherwise, you will retain ownership of your kBTC/interBTC and will need to open a new redeem request if you still wish to redeem for BTC.


Click on the redeem request that is "Pending". This will open a modal, where you will see a **"Retry"** button. Click on it to cancel your request, receiving a percentage in collateral as a convenience fee and allowing you to open a new redeem request for your kBTC/interBTC.
