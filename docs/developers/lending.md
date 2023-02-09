# Lending Development Notes

## Deposit to a Lending Market

- A user calls the `loans.mint()` extrinsic to deposit an asset into a market.
- The user is credited with newly minted `qTokens`. `qTokens` are of type `{LendToken: Id}` where the `Id` is the market Id representing a claim on the underlying, deposited asset.
- The loans pallet is credited with the assets deposited by the user.

## qToken as Collateral

- A user can use qTokens as collateral. qToken as collateral is subject to governance votes for both the lending protocols and the BTC bridge.
- A single account id can declare the qTokens as collateral only in a single protocol: either all of the tokens of a specific qToken type are collateral in the lending protocol or in the bridge.
- If a user wishes to have some qTokens as collateral in lending and some tokens as collateral in the bridge, the user has to distribute the tokens across two accounts.

## qToken as Collateral in Lending

- A user calls the `loans.deposit_all_collateral()` extrinsic to deposit all of their qTokens as collateral.
- Subsequently minted qTokens of the same type will automatically be deposited as collateral.
- When a user deposits qTokens as collateral, the `{LendToken: Id}` `free` balance is moved to the `reserved` balance on that account.

## qToken Exchange Rates

- The exchange rate from `qTokens` to the underlying asset is stored in the `ExchangeRate` storage map and can be fetched with the `loans.exchangeRate()` storage query.
- Exchange rates are initialized at 20 * 10^15 and are updated during:

  - Minting or redeeming qTokens for the underlying token.
  - Borrowing and repaying debt from a loan.
  - Liquidations of loans.
  - Adding or removing reserves to a market.
