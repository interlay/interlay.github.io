# Lend and Borrow

The lending protocol allows users to trustlessly lend and borrow. Assets that are supplied to the lending pool start earning interest immediately, and can be used as a security deposit for over-collateralized borrowing. Borrowers can repay their loan at any time, along with the interest they owe, while lenders can only withdraw their deposit if there is more liquidity left in the pool (i.e. not borrowed) than the amount to be withdrawn. Interest rates are variable and depend on the supply and demand of each asset.

## Prerequisites

Make sure you have a compatible [wallet plugin](guides/wallets-explorers.md#substrate-wallets) installed.

## Lend and Borrow

At the end of this guide you will have:

- [x] [Supplied an asset and started earning interest](#3-deposit-to-a-lending-market)
- [x] [Enabled the deposit as borrow collateral](#4-enable-the-collateral-toggle)
- [x] [Borrowed using the collateral](#5-borrow)
- [x] [Repaid a loan](#6-repay-a-loan)
- [x] [Withdrawn part of the supplied amount](#7-withdraw-a-deposit)

### 1. Go to the lending page

<!-- tabs:start -->

#### **Interlay**

[app.interlay.io/lending](https://app.interlay.io/lending)

#### **Kintsugi**

[kintsugi.interlay.io/lending](https://kintsugi.interlay.io/lending)

#### **Testnet-Kintsugi**

[kintnet.interlay.io/lending](https://kintnet.interlay.io/lending)

#### **Testnet-Interlay**

[testnet.interlay.io/lending](https://testnet.interlay.io/lending)

<!-- tabs:end -->

There are three markets available in the screenshot attached. You can take a look at the various interest rates and decide which ones you are interested in supplying or borrowing.

![The Lending page](../_assets/img/guide/lending-overview.png)

### 2. Bring tokens for lending and transaction fees

You will need some tokens to pay for transaction fees (KINT on Kintsugi, INTR on Interlay). Additionally, you also need tokens to deposit into the lending pool.

<!-- tabs:start -->

#### **Interlay**

A list of exchanges with INTR listings can be found on [Coingecko](https://www.coingecko.com/en/coins/interlay). Those exchanges should also have the assets in the Interlay lending markets listed.

#### **Kintsugi**

A list of exchanges with KINT listings can be found on [Coingecko](https://www.coingecko.com/en/coins/kintsugi). Those exchanges should also have the assets in the Kintsugi lending markets listed.

#### **Testnet**

On testnet, you can obtain some tokens (KINT/INTR, KBTC/IBTC, KSM/DOT, USDT) by clicking on the "Tokens Faucet" button on the right-hand side of the top bar.

<!-- tabs:end -->

### 3. Deposit to a lending market

Select a lending market and enter the amount you wish to supply.

![Lend KBTC](../_assets/img/guide/lending-lend-asset.png)

Observe the "My Lend Positions" table that has been populated with the deposit.

![My Lend Position KBTC](../_assets/img/guide/lending-overview-lend-positions.png)

### 4. Enable the collateral toggle

!> **Attention:** Deposits that are enabled as collateral are subject to liquidation when the borrowed balance becomes undercollateralized.

In the "My Lend Positions" table, in the "Collateral" column, click on the toggle of the deposit you wish to enable as collateral. Doing so will allow you to take out an overcollateralized loan that is backed by this deposit.

![Enable Collateral Toggle](../_assets/img/guide/lending-enable-collateral.png)

If there are deposits in multiple asset types, it is easy to see which one is enabled as collateral and which one is not.

![KBTC enabled as collateral](../_assets/img/guide/lending-lend-position-overview.png)

Whenever there is collateral, a section dedicated to your LTV (loan-to-value) appears, which will provide you with in-depth insights into the status of your positions

![LTV Section](../_assets/img/guide/lending-ltv-section.png)

### 5. Borrow

Select an asset from the "Borrow Markets" table and enter the amount you wish to borrow. Note that if the loan-to-value ratio increases too much, your collateral deposits could get liquidated.

![Borrow KBTC](../_assets/img/guide/lending-borrow-asset.png)

The loan is now visible in the "My Borrow Positions" table.

![KBTC Loan](../_assets/img/guide/lending-overview-borrow-positions.png)

### 6. Repay a loan

When you are ready to repay a loan, click on an item in the "My Borrow Positions" and enter an amount. There is a graph that shows how your loan-to-value (LTV) ratio will improve as a result.

![Repay KBTC](../_assets/img/guide/lending-repay-asset.png)

### 7. Withdraw a deposit

Click on an item in the "My Lend Positions" and select the "Withdraw" tab. You can withdraw the full deposit by clicking on the blue amount next to the "Limit:" text, or do a partial withdraw by entering a custom amount. Depending on the interest rates, you should be able to withdraw more than the initially deposited amount.

![Withdraw KBTC](../_assets/img/guide/lending-withdraw-asset.png)

## Liquidate Loans

To ensure that loans are always over-collateralized, such that borrowers maintain an economical interest to repay their loans, under-collateralized loans need to be liquidated. This can be done by repaying the borrowed tokens on behalfs of the borrower to receive the borrowers collateral, including a premium. This guide instructs users how to process a liquidation of an under-collateralized loan using polkadot.js.

For an automated liquidation bot, see [here](https://github.com/interlay/bots/tree/master/bots/lending-liquidator). Overall, we recommend the automated bot or a script for liquidations.

At the end of this guide you will know how to:

- [x] [Funded your wallet with the debt token and gas token](#_1-fund-wallet)
- [x] [Liquidated a borrow position using polkadot.js](#_2-liquidate-borrow-position)
- [x] [Withdraw the received collateral tokens](#_3-withdraw-collateral-optional)
- [x] [Batch liquidations](#batch-liquidations)

### 1. Discover under-collateralized loans

1. Go to [polkadot.js](https://polkadot.js.org/apps/#/chainstate) and select the network where you want to make the liquidation
2. Get all accounts that have borrowed via `loans.accountBorrows(CurrencyId)`. For example, to get all accounts that borrowed DOT, use `loans.accountBorrows({Token: DOT})`. The result is a list of accounts that have borrowed DOT.
3. Get the liquidation threshold for the asset via the `rpc`: `loans.getLiquidationThresholdLiquidity()` for the accounts above. In the response, the `shortfall` is the amount that can be liquidated denominted in BTC. It's decoded as a `FixedU128` and needs to be divided by `10^18` to get the actual amount. Accounts that have a shortfall > 0 can be liquidated. To get the shortfall in other currencies, the `shortfall` BTC amount needs to be converted to other currencies via the on-chain oracle.
4. Get the collateral currencies via `loans.accountDeposits({LendToken: ID})`.
5. Determine the underlying currency amount as described in the [asset developer notes](developers/assets?id=lend-tokens-calculating-the-underlying-token-balance).
6. From there, iterate through the accounts from step 3 with a `shortfall` > 0, check the qToken asset as in step 4, and then calculate the underlying asset as described in step 5.

The above is implemented in `getUndercollateralizedBorrowers` in the [interbtc-api](https://docs.interlay.io/interbtc-api/interfaces/LoansAPI.html#getundercollateralizedborrowers).


### 2. Fund wallet

1. The wallet needs a token to pay for [transaction fees](guides/assets.md).
2. The wallet needs to be funded with the amount of borrowed tokens which shall be repaid. For example, if the liquidator aims to liquidate a debt position of 1,000 USDT, the wallet needs to have at least 1000 USDT.

### 3. Liquidate borrow position

1. Go to [https://polkadot.js.org/apps/#/extrinsics](https://polkadot.js.org/apps/#/extrinsics) and select the network where you want to make the liquidation
2. Set up the parameters for the call
   1. **using the selected account**: `your_accound_address
   2. **submit the following extrinsic**: `loans.liquidateBorrow()`
   3. **borrower:** `target_account_address` (address of the borrower to liquidate) 
   4. **liquidationAssetId:** Borrowed currency to repay
      1. Select `Token` for INTR, DOT, IBTC, KINT, KBTC, KSM
      2. Select `ForeignAsset` for other tokens
         1. see [here](developers/assets?id=foreignasset-assets) for `id:token` mapping
   5. **repayAmount:** `amount_to_be_repaid`. The maximum that can be repaid is the `shortfall` equivalent of one of the collateral currencies as described in [discover undercollateralized loans](#_1-discover-under-collateralized-loans). Note that the amount depends on the number of decimals the token uses.
   6. **collateralAssetId:** The currency to receive in exchange for liquidating the borrower's loan. The liquidation premium is also paid in this currency. Note that this currency has to be one of the collateral currencies used by the liquidated borrower (as described [above](#_1-discover-under-collateralized-loans)). Also note that while `collateralAssetId` represents the underlying currency of a lending market (e.g. KBTC), the liquidator receives its qToken version instead (e.g., qKBTC), which can be redeemed for KBTC from the lending market (see [withdraw collateral](#_7-withdraw-a-deposit)).
      1. Select `Token` for INTR, DOT, IBTC, KINT, KBTC, KSM
      2. Select `ForeignAsset` for other tokens
         1. see [here](developers/assets?id=foreignasset-assets) for `id:token` mapping
3. Submit the transaction

### Example

This example would liquidate a $1,000 USDT position in order to receive kBTC as collateral.

![Liquidate Borrow Extrinsic](../_assets/img/guide/liquidate-borrow-extrinsic.png)

### Batch Liquidations

It is also possible to reduce the required amount needed to liquidate a position, by splitting the amount of debt to be repaid into several calls with smaller amounts. This does require to swap the received `collateral tokens` back into the `debt token` after the collateral has been claimed.

Hence, the batched call would look something like this:

[(`liquidate_borrow`, `redeem_all`, `swap_exact_input`), (`liquidate_borrow`, `redeem_all`, `swap_exact_input`), …]

### Scripting

As the manual steps to discover and liquidate positions are quite involved, an easier option would be to script the steps above. Using the [interbtc-api](https://github.com/interlay/interbtc-api), one could achieve the following:

```typescript
// see https://github.com/interlay/interbtc-api/blob/master/test/integration/parachain/staging/sequential/loans.test.ts#L512
const undercollateralizedBorrowers = await api.loans.getUndercollateralizedBorrowers();
// Assuming a borrower took out a loan of IBTC against a DOT collateral position
const liquidate_tx = api.loans.liquidateBorrowPosition(
   // borrower to liquidate
   undercollateralizedBorrowers[0].accountId,
   // repayment currency for the tokens that were borrowed from the market (e.g., IBTC)
   undercollateralizedBorrowers[0].borrowPositions[0].currencyId,
   // amount to repay. If using IBTC/KBTC to repay, then oracle conversions are not needed
   undercollateralizedBorrowers[0].shortfall,
   // currency to receive as collateral. For example, receiving the DOT that the borrower used as collateral
   undercollateralizedBorrowers[0].collateralPositions[0].currencyId
)
// sign and send the tx
await liquidate_tx.sign_and_send();
```
