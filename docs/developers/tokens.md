# Token Development Notes

## User Portfolio

The user's portfolio consists of assets and liabilities. Assets are tokens that the user owns, and liabilities are tokens that the user has borrowed.

Both assets and liabilities are tracked separately on-chain. The user's net worth can be computed by:

`net_worth = assets - liabilities`

### User Assets

All tokens are stored in the `tokens` pallet. A user's balance can be retrieved from `tokens.accounts(<account_id>)`.

This returns an array of tuples, where each tuple contains the token and the balance:

```ts
[
  [
    [
      <account_id>
      {
        Token: KBTC
      }
    ]
    {
      free: 499,250
      reserved: 333,333
      frozen: 0
    }
  ]
  [
    [
      <account_id>
      {
        Token: KSM
      }
    ]
    {
      free: 2,000,000
      reserved: 0
      frozen: 23,000
    }
  ]
  ...
]
```

### User Liabilities

Borrowed tokens are stored in the `loans` pallet and can be retrieved from `loans.accountBorrows(<currency>, <account_id>)`.

This returns an object:

```ts
// For example, Token.KSM for a single account
{
  principal: 100,000,000,000,000
  borrowIndex: 1,000,000,000,000,000,000
}
```

The `principal` is the number of tokens that the user has borrowed *without interest*. The `borrowIndex` is the `usersBorrowIndex` and is specific to that user.

To calculate the liability, the *global* `globalBorrowIndex` must be retrieved from `loans.borrowIndex(<currency>)`.

The liability is then: `principal * globalBorrowIndex / userBorrowIndex`.

## Token Types

There are five different token types:

- `Token`: Tokens that are native to the Interlay/Kintsugi chains or are bridged from the relay chain, such as `KSM` or `DOT`.
- `ForeignAsset`: Tokens that are bridged from another chain, such as `ForeignAsset.1` where the first asset would be `USDT`.
- `LendToken`: Tokens represent an underlying token that has been supplied to the `loans` pallet. For example, `LendToken.DOT` represents the `DOT` that has been supplied to the `loans` pallet.
- `LpToken`: This token is a tuple consisting of two other tokens that represent a liquidity pool. For example, `LpToken(Token.DOT, ForeignAsset.1)` represents the liquidity pool for `DOT` and `USDT`.
- `StableLpToken`: This token is a tuple consisting of two or more tokens in a stable pool. For example, `StableLpToken.1` could represent a stable pool for `IBTC` and `WBTC`.
