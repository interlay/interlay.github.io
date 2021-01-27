# User guide

PolkaBTC allows you to receive a representation of BTC to be used any way you see fit in the Polkadot ecosystem.
To get you started, follow this guide.

At the end of this document you will have:

- [x] Installed the polkdot-js browser extension and a Bitcoin wallet
- [x] Received testnet BTC and testnet DOT
- [x] Issued your first PolkaBTC
- [x] Redeemed your first PolkaBTC
- [x] Observed the system using the dashboard

## Prerequisites

### Bitcoin testnet wallet

You will need a Bitcoin testnet wallet to test PolkaBTC.
For a general overview of Bitcoin wallets you can consult [bitcoin.org's wallet selector](https://bitcoin.org/en/choose-your-wallet?step=5).

### Getting testnet BTC

Make sure you have at least some tBTC in your wallet. You can get them from a faucet:

*   [https://testnet-faucet.mempool.co/](https://testnet-faucet.mempool.co/)
*   [https://bitcoinfaucet.uo1.net/](https://bitcoinfaucet.uo1.net/)
*   [https://coinfaucet.eu/en/btc-testnet/](https://coinfaucet.eu/en/btc-testnet/)
*   [https://kuttler.eu/en/bitcoin/btc/faucet/](https://kuttler.eu/en/bitcoin/btc/faucet/)
*   [https://tbtc.bitaps.com/](https://tbtc.bitaps.com/)


### Polkadot testnet wallet (polkadot-js browser extension)

You will need the polkadot-js browser extension to test PolkaBTC.

1.   Install the polkadot-js extension in your browser: [https://github.com/polkadot-js/extension](https://github.com/polkadot-js/extension).
2.   Create a new account.
3.   Connect account to polkabtc.io via the polkadot-js pop-up.

**Please make sure to first create an account before connecting your wallet to polkabtc.io!**.

### Getting Testnet DOT

You can get testnet DOT by clicking on the faucet link in the top bar of polkabtc.io.

**Note: these testnet DOT are only usable on PolkaBTC. They are not real DOT that you can trade anywhere.**

## Issue PolkaBTC (PolkaBTC UI + Bitcoin Testnet Wallet)


### Issue page

The Issue page displays your current PolkaBTC and DOT balances. In addition, a table shows all of your ongoing/pending Issue requests.


### Issue Process

**Don't forget to get some testnet DOT via the faucet ("Request DOT" button, right side of top bar) before making an issue request. You will need this to pay for parachain transaction fees**.

To issue PolkaBTC, follow the “Issue PolkaBTC” button on the Issue page. In the shown pop-up dialogue:

1. Enter the amount of PolkaBTC that you want to issue.
    1. We will search for a vault to serve your request.
    2. The maximum amount of PolkaBTC that can be issued in a single request depends on the maximum amount of collateral a vault has locked.(high-volume Issue requests, executed with multiple vaults simultaneously, will be added as a feature before mainnet launch).
    3. You will currently no pay any fees. On mainnet, you will pay a small fee to the vault.
2. Review your issue request and make the BTC payment. Make sure to **keep hold of you Bitcoin transaction ID**
3. Once you have made the BTC payment, continue to the next page.
4. Enter the TXID of your Bitcoin transaction and close the dialogue.
5. Wait. Once your transaction is included in Bitcoin and has gained sufficient confirmations, a button to finalize the issue process in the table.
6. Click on the “Execute” button to finalize the Issue process and claim your PolkaBTC.


## Redeem PolkaBTC (PolkaBTC UI)


### Redeem page

The Redeem page displays your current PolkaBTC and DOT balances.

In addition, the table shows all of your ongoing Redeem requests.


### Redeem Process

To redeem PolkaBTC, follow the “Redeem PolkaBTC” button on the Redeem page. In the shown modal:



1. Enter the amount of PolkaBTC that you want to redeem.
    1. A vault will be assigned to you for this request.
    2. The maximum amount of PolkaBTC that you can redeem at in a single request depends on the maximum amount of BTC a vault has locked (high-volume Redeem requests, executed with multiple vaults simultaneously, will be added as a feature before mainnet launch).
2. Enter your Bitcoin address. Supported address types: P2WPKH, P2WSH **currently in bech32 format only!**.
3. Review and confirm the Redeem request.
4. The Redeem request is now being processed by the vault. Updates will appear in the table on the Redeem page.

## Dashboard

The Dashboard shows the current status of the PolkaBTC bridge / the BTC-Parachain.

At the top of the page, you can see the total amount of locked DOT collateral across all vaults, the amount of minted PolkaBTC, as well as the current overall collateralization rate.


### Bitcoin Relay Status

This table shows the state of the Bitcoin mainchain, as tracked by the BTC-Parachain and the [Blockstream testnet explorer](https://blockstream.info/testnet/) (“Bitcoin Core”).

You can safely use the bridge as long as the table shows "Online".


### Vaults

This table displays all vaults registered with the bridge. For each vault, you can see:

*   the BTC-Parachain account,
*   the BTC address,
*   the amount of locked DOT collateral,
*   the amount of BTC held in custody,
*   the collateralization rate, and
*   whether the vault is currently active.


### Oracle Status

This table shows the status of all exchange rate oracles connected to the BTC-Parachain, serving the latest BTC/DOT price.

You can safely use the bridge as long as the table shows "Online".


### Staked Relayer

This table displays all Staked Relayers registered with the bridge. For each Staked Relayer, you can see:

*   the BTC-Parachain account,
*   the amount of locked DOT stake,
*   whether the Staked Relayer is currently active.

You can safely use the bridge as long as the table shows "Ok".

