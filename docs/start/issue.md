# Issue PolkaBTC

PolkaBTC allows you to receive a representation of BTC to be used any way you see fit in the Polkadot ecosystem.
To get you started, follow this guide.

At the end of this guide you will have:

- [x] Locked BTC with a collateralized Vault
- [x] Issued your first PolkaBTC on the PolkaBTC app

?> _TODO_ Add the video here.

## Prerequisites

Make sure you have the required [polkadot-js extension and a Bitcoin wallet](start/prereq.md).


## Issue PolkaBTC


### 1. Go to [ rococo.polkabtc.io](https://rococo.polkabtc.io/app).

The Issue page displays your current PolkaBTC and DOT balances. In addition, a table shows all of your ongoing/pending Issue requests.



### 2. Enter the amount of PolkaBTC you want to issue in the app

?> Don't forget to get some testnet DOT via the faucet ("Request DOT" button, right side of top bar) before making an issue request. You will need this to pay for parachain transaction fees.

Enter the amount of PolkaBTC you want to issue. The app will automatically select a vault for you. Click **"Next"**.


### OLD


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
