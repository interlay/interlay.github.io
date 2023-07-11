# Asset API

## SS58 Format

The SS58 format for accounts on different networks is as follows:

<!-- tabs:start -->
#### **Interlay**

- SS58: `2032`
- Prefix: `wd`

#### **Kintsugi**

- SS58: `2092`
- Prefix: `wd`

#### **Testnet (Interlay)**

- SS58: `2032`
- Prefix: `wd`

#### **Testnet (Kintsugi)**

- SS58: `2092`
- Prefix: `wd`

<!-- tabs:end -->

## Tokens

All assets in Kintsugi and Interlay networks are implemented using [orml-tokens](https://github.com/open-web3-stack/open-runtime-module-library).

* Asset implementation: [orml-tokens](https://github.com/open-web3-stack/open-runtime-module-library/tree/master/tokens)
* Cross-chain transfer implementation (XCM): [orml-xtokens](https://github.com/open-web3-stack/open-runtime-module-library/tree/master/xtokens)
* Foreign assets: [orml-asset-registry](https://github.com/open-web3-stack/open-runtime-module-library/tree/master/asset-registry)

?> Querying the `system` account is not supported on Kintsugi or Interlay. Please use the API methods described below to achieve transfers.


### Token Types

The Interlay and Kitnsugi networks have five token types as part of the `InterbtcPrimitivesCurrencyId` enum:

- `Token`: Used for native tokens and relay chain assets. Examples: INTR, IBTC, DOT.
- `ForeignAsset`: Used for assets from other chains. Requires registering the asset. Examples: USDT, VDOT, wBTC, ETH.
- `LendToken`: Used to represent lending positions. Referred to as `qTokens` in user-facing documentation. Examples: qUSDT, qDOT.
- `LpToken`: Used to represent a liquidity position in the general AMM. Examples: LP-DOT-IBTC.
- `StableLpToken`: Used to represent a liquidity position in the stable AMM. Examples: LP-USDT-USDC.

Querying and transferring tokens is done using the `tokens` pallet.

```ts
// Query
api.query.tokens.accounts('address', InterbtcPrimitivesCurrencyId);
// Transfer
api.tx.tokens.transfer('address', InterbtcPrimitivesCurrencyId, amount);
```

## `Token` Assets

The same `Token` assets are registered on all networks.

| Asset       | CURRENCY_ID | CURRENCY_INDEX | DECIMALS | Multilocation                                       |
|-------------|-------------|----------------|----------|-----------------------------------------------------|
| DOT         | DOT         | 0              | 10       | (Parent, Here)                                      |
| interBTC    | IBTC        | 1              | 8        | (Parent, (X2, Parachain: 2032, GeneralKey: 0x0001)) |
| INTR        | INTR        | 2              | 10       | (Parent, (X2, Parachain: 2032, GeneralKey: 0x0002)) |
| KSM         | KSM         | 10             | 12       | (Parent, Here)                                      |
| kintsugiBTC | KBTC        | 11             | 8        | (Parent, (X2, Parachain: 2092, GeneralKey: 0x000b)) |
| KINT        | KINT        | 12             | 12       | (Parent, (X2, Parachain: 2092, GeneralKey: 0x000c)) |

### Available assets

```ts
api.rpc.system.properties();
```

<details>
<summary>
Example response
</summary>

```json
{
  "ss58Format": 2032,
  "tokenDecimals": [
    10,
    8,
    10,
    12,
    8,
    12
  ],
  "tokenSymbol": [
    "DOT",
    "IBTC",
    "INTR",
    "KSM",
    "KBTC",
    "KINT"
  ]
}
```
</details>

### Account balance

This query will return the smallest unit of the token. To get the balance in the token's decimals, divide by `10^decimals`.

```ts
api.query.tokens.accounts('address', {Token: 'IBTC'});
```
<details>
<summary>
Example response
</summary>


```json
{
    "free": 817517,
    "reserved": 0,
    "frozen": 0,
}
```
</details>

### Transfer

```ts
api.tx.tokens.transfer('address', {Token: 'IBTC'}, amount)
```

### `ForeignAsset` Assets

The `assetRegistry` pallet is used for assets from other chains.

?> Before using foreign assets in your application or interacting with them via polkadot.js directly, please verify the correctness of the data for the network you are trying to use via [https://polkadot.js.org/apps/#/chainstate](https://polkadot.js.org/apps/#/chainstate) -> `assetRegistry` -> `metadata()`.


<!-- tabs:start -->
#### **Interlay**

| Asset        | CURRENCY_ID | ASSET_INDEX | DECIMALS | Multilocation                                       |
|--------------|-------------|-------------|----------|-----------------------------------------------------|
| Liquid DOT   | LDOT        | 1           | 10       | (Parent, (X2, Parachain: 2000, GeneralKey: 0x0003)) |
| Tether USD   | USDT        | 2           | 6        | (Parent, (X3, Parachain: 1000, PalletInstance: 50, GeneralIndex: 1984)) |

#### **Kintsugi**

| Asset        | CURRENCY_ID | ASSET_INDEX | DECIMALS | Multilocation                                       |
|--------------|-------------|-------------|----------|-----------------------------------------------------|
| Acala Dollar | AUSD        | 1           | 12       | (Parent, (X2, Parachain: 2000, GeneralKey: 0x0081)) |
| Liquid KSM   | LKSM        | 2           | 12       | (Parent, (X2, Parachain: 2000, GeneralKey: 0x0083)) |
| Tether USD   | USDT        | 3           | 6        | (Parent, (X3, Parachain: 1000, PalletInstance: 50, GeneralIndex: 1984)) |

<!-- tabs:end -->

### Available assets

```ts
api.query.assetRegistry.metadata.entries();
```

<details>
<summary>
Example response for Interlay
</summary>

```json
[
  [
    [
      1
    ],
    {
      "decimals": 10,
      "name": "Liquid DOT",
      "symbol": "LDOT",
      "existentialDeposit": 0,
      "location": {
        "V2": {
          "parents": 1,
          "interior": {
            "X2": [
              {
                "Parachain": 2000
              },
              {
                "GeneralKey": "0x0003"
              }
            ]
          }
        }
      },
      "additional": {
        "feePerSecond": 20427078323,
        "coingeckoId": "liquid-staking-dot"
      }
    }
  ],
  [
    [
      2
    ],
    {
      "decimals": 6,
      "name": "Tether USD",
      "symbol": "USDT",
      "existentialDeposit": 0,
      "location": {
        "V3": {
          "parents": 1,
          "interior": {
            "X3": [
              {
                "Parachain": 1000
              },
              {
                "PalletInstance": 50
              },
              {
                "GeneralIndex": 1984
              }
            ]
          }
        }
      },
      "additional": {
        "feePerSecond": 20427078323,
        "coingeckoId": "tether"
      }
    }
  ]
]
```
</details>


### Account balance

This query will return the smallest unit of the token. To get the balance in the token's decimals, divide by `10^decimals`.

```ts
// USDT on Interlay
api.query.tokens.accounts('address', {ForeignAsset: 2});
```

<details>
<summary>
Example response
</summary>

```json
{
  "free": 817517,
  "reserved": 0,
  "frozen": 0,
}
```
</details>

### Transfer

```ts
// USDT on Interlay
api.tx.tokens.transfer('address', {ForeignAsset: 2}, amount)
```

## `LendToken` Assets

The `LendToken` assets are used for the lending protocol. In frontend applications, the `LendToken` types are written as `q` tokens, e.g., `qDOT` or `qKSM`.

### Available assets

- Tokens that can be used in the lending protocol can be queried using polkadot.js by iterating over the `loans.markets` map. 
- Of the returned items, only those that have `state: Active` are usable by the lending protocol. 
- The key of the map is the underlying currency, and the lend token id is stored in the `lendTokenId` field.

For example, on Interlay:

```ts
[
  // query key
  {
    Token: IBTC // underlying currency
  }
]
{
  ...
  state: Active // only active markets can be used
  ...
  lendTokenId: {
    LendToken: 1 // lend token id
  }
}
```

Getting all markets can be achieved through:

```ts
api.query.loans.markets.entries();
```

<details>
<summary>
Example response for Interlay
</summary>

```ts
[
  [
    [
      {
        Token: IBTC
      }
    ]
    {
      collateralFactor: 63.00%
      liquidationThreshold: 67.00%
      reserveFactor: 10.00%
      closeFactor: 50.00%
      liquidateIncentive: 1,100,000,000,000,000,000
      liquidateIncentiveReservedFactor: 0.00%
      rateModel: {
        Jump: {
          baseRate: 0
          jumpRate: 50,000,000,000,000,000
          fullRate: 500,000,000,000,000,000
          jumpUtilization: 90.00%
        }
      }
      state: Active
      supplyCap: 3,000,000,000
      borrowCap: 3,000,000,000
      lendTokenId: {
        LendToken: 1
      }
    }
  ]
  [
    [
      {
        ForeignAsset: 2
      }
    ]
    {
      collateralFactor: 67.00%
      liquidationThreshold: 74.00%
      reserveFactor: 10.00%
      closeFactor: 50.00%
      liquidateIncentive: 1,100,000,000,000,000,000
      liquidateIncentiveReservedFactor: 0.00%
      rateModel: {
        Jump: {
          baseRate: 0
          jumpRate: 100,000,000,000,000,000
          fullRate: 500,000,000,000,000,000
          jumpUtilization: 90.00%
        }
      }
      state: Active
      supplyCap: 600,000,000,000
      borrowCap: 600,000,000,000
      lendTokenId: {
        LendToken: 3
      }
    }
  ]
  [
    [
      {
        Token: DOT
      }
    ]
    {
      collateralFactor: 67.00%
      liquidationThreshold: 77.00%
      reserveFactor: 10.00%
      closeFactor: 50.00%
      liquidateIncentive: 1,100,000,000,000,000,000
      liquidateIncentiveReservedFactor: 0.00%
      rateModel: {
        Jump: {
          baseRate: 50,000,000,000,000,000
          jumpRate: 200,000,000,000,000,000
          fullRate: 500,000,000,000,000,000
          jumpUtilization: 80.00%
        }
      }
      state: Active
      supplyCap: 10,000,000,000,000,000
      borrowCap: 10,000,000,000,000,000
      lendTokenId: {
        LendToken: 2
      }
    }
  ]
]
```
</details>


### Account balance 

This query will return the smallest unit of the token.

```ts
// qDOT on Interlay
api.query.tokens.accounts('address', {LendToken: 2});
```

<details>
<summary>
Example response
</summary>

```json
{
  "free": 817517,
  "reserved": 432143,
  "frozen": 0,
}
```
</details>

#### Lend tokens: Calculating the underlying token balance

<details>
<summary>
Expand
</summary>

- The `free` balance is the amount of tokens that can be transferred and used, e.g., as collateral for vaults.
- The `reserved` balance is the number of tokens currently used as collateral in the lending protocol or as vault collateral.
- To get the exchange rate of the `LendToken` to the underlying token, use the `exchangeRate` function:

```ts
// DOT to qDOT on Interlay
api.query.loans.exchangeRate({Token: 'DOT'});
```

This will return, e.g., `20,002,062,210,874,674`. The return value is a `FixedU128` type.

With this, the claim on the underlying token can be calculated as follows:

```python
underlying_amount = exchange_rate * user_balance / 10^18
```

</details>

### Transfer

```ts
// qDOT on Interlay
api.tx.tokens.transfer('address', {LendToken: 2}, amount)
```

### Supplied amounts

The supplied amount of an account is locked in the lending pallet itself. To calculate the total balance of an account, it is sufficient to use the account balance from the `tokens` pallet, and no query to the `loans` pallet is required.

### Borrowed amounts (debt)

When calculating the total balance of an account, the borrowed amounts needs to be subtracted.

```ts
// USDT borrowed by an account on Interlay
api.query.loans.accountBorrows({ForeignAsset: 2}, "address");
```

<details>
<summary>
Example response
</summary>

```json
{
  "principal": 575679114,
  "borrowIndex": 1000145453030201746
}
```
</details>

The `principal` amount is the amount of tokens that have been borrowed by the account without interest. The `borrowIndex` is the index of the borrow amount, which is used to calculate the interest rate together with the `globalBorrowIndex`.

## `LpToken` Assets

The `LpToken` assets are used for the general (Uniswap v2-style) AMM protocol. In frontend applications, the `LpToken` types are written as a combination of the two underlying token tickers, e.g., `LP-DOT-IBTC` or `LP-USDT-INTR`.

?> All LP tokens can be assumed to have 18 decimals.

### Available assets

- Tokens that can be used in the general AMM protocol can be queried using polkadot.js by iterating over the `dexGeneralAmm.pairStatus` map.

For example, on Interlay:

```ts
[
  [ // query key [Token0, Token1]
    [
      {
        Token: IBTC
      }
      {
        ForeignAsset: 2
      }
    ]
  ]
  // pool status and properties
  {
    Trading: {
      pairAccount: wd9yNSwR3vpgebhpPs1j4oEc7WeavPC88JqyDucW85HuM1kp9 // balance of pool tokens
      totalSupply: 7,677,975,842
      feeRate: 0
    }
  }
]
```

Getting all pools and their accounts can be achieved with:

```ts
api.query.dexGeneralAmm.pairStatus.entries();
```

<details>
<summary>
Example response
</summary>

```ts
[
  [
    [
      [
        {
          Token: IBTC
        }
        {
          ForeignAsset: 2
        }
      ]
    ]
    {
      Trading: {
        pairAccount: wd9yNSwR3vpgebhpPs1j4oEc7WeavPC88JqyDucW85HuM1kp9
        totalSupply: 7,677,975,842
        feeRate: 0
      }
    }
  ]
  [
    [
      [
        {
          Token: DOT
        }
        {
          Token: IBTC
        }
      ]
    ]
    {
      Trading: {
        pairAccount: wd9yNSwR3vpgebhpPs1j2T8cCkEj6bawhi87izYtVVYGx2KsN
        totalSupply: 271,186,213,319
        feeRate: 0
      }
    }
  ]
  [
    [
      [
        {
          Token: INTR
        }
        {
          ForeignAsset: 2
        }
      ]
    ]
    {
      Trading: {
        pairAccount: wd9yNSwR3vpgebhpPs1j7R22vFCQ1GfXyBk68cLZcy1Z7aLSq
        totalSupply: 13,485,053,212,266
        feeRate: 0
      }
    }
  ]
]
```
</details>

### Account balance

```ts
// LP-DOT-IBTC on Interlay
api.query.tokens.accounts('address', LpToken: [{Token: 'DOT'}, {Token: 'IBTC'}]);
```

<details>
<summary>
Example response
</summary>

```json
{
  "free": 817517,
  "reserved": 432143,
  "frozen": 0,
}
```
</details>

#### LpTokens: Calculating the underlying token balance

<details>
<summary>
Expand
</summary>

- The `free` balance is the amount of tokens that can be transferred and then used, e.g., as collateral for vaults.
- The `reserved` balance is the number of tokens that are locked for farming.


To get the exchange rate of the `LpToken` to the underlying token:

1. Get the pair account and total supply of the pool:

```ts
// LP-DOT-IBTC on Interlay
api.query.dexGeneralAmm.pairStatus({Token: 'DOT', Token: 'IBTC'});
// response
{
  Trading: {
    pairAccount: wd9yNSwR3vpgebhpPs1j2T8cCkEj6bawhi87izYtVVYGx2KsN
    totalSupply: 271,186,213,319
    feeRate: 0
  }
}
```

2. Get the balance of the pool account: 
 
```ts
// pairAccount: wd9yNSwR3vpgebhpPs1j2T8cCkEj6bawhi87izYtVVYGx2KsN
api.query.tokens.accounts(pairAccount).entries();
// response
[
  [
    [
      wd9yNSwR3vpgebhpPs1j2T8cCkEj6bawhi87izYtVVYGx2KsN
      {
        Token: DOT
      }
    ]
    {
      free: 207,193,509,612,667
      reserved: 0
      frozen: 0
    }
  ]
  [
    [
      wd9yNSwR3vpgebhpPs1j2T8cCkEj6bawhi87izYtVVYGx2KsN
      {
        Token: IBTC
      }
    ]
    {
      free: 354,945,660
      reserved: 0
      frozen: 0
    }
  ]
]
```

3. Get the balance of the account

```ts
api.query.tokens.accounts(address, LpToken: [{Token: 'DOT'}, {Token: 'IBTC'}])`
```

4. Calculate the two token balances of the pool:

```python
# DOT
dot_balance = pool.dot_balance * account.lp_token_balance / pool.total_supply
# IBTC
ibtc_balance = pool.ibtc_balance * account.lp_token_balance / pool.total_supply
```
</details>

### Transfer

```ts
// LP-DOT-IBTC on Interlay
api.tx.tokens.transfer('address', {LpToken: [{Token: 'DOT'}, {Token: 'IBTC'}]}, amount)
```

## `StableLpToken` Assets

LP tokens in the "stable" (Curve v1) AMM can be queried using the `tokens` pallet and `CurrencyId::StableLpToken(u32)`, where the `u32` is the pool ID which can be queried using `dexStableAmm.pools(u32)`.

?> All LP tokens can be assumed to have 18 decimals.

?> The `StableLpToken` assets are not yet available on mainnet.

### Available assets

- Tokens that can be used in the stable AMM protocol (Curve v1-style) can be queried using polkadot.js by iterating over the `dexStable.pools` map.

```ts
api.query.dexStable.pools.entries();
```

### Account balance

```ts
api.query.tokens.accounts('address', StableLpToken: 0);
```

### Transfer

```ts
api.tx.tokens.transfer('address', {StableLpToken: 0}, amount)
```

## User Portfolio

The user's portfolio consists of assets and liabilities. Assets are tokens that the user owns, and liabilities are tokens that the user has borrowed.

Both assets and liabilities are tracked separately on-chain. The user's net worth can be computed by:

```python
net_worth = assets - liabilities
```

### User Assets

All tokens are stored in the `tokens` pallet. A user's balance can be retrieved with:

```ts
api.query.tokens.accounts(<account_id>).entries();
```

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

Calculating the user's net worth in another currency like USD, requires multiplying the balance of each token by the exchange rate of that token to the USD.

- `Token`: Tokens should have a direct USD exchange rate, e.g., `Token.DOT` should have a USD exchange rate.
- `ForeignAsset`: Foreign assets should have a USD exchange rate, e.g., `ForeignAsset.2` (USDT) should have a USD exchange rate.
- `LendToken`: Lending tokens should be converted to their underlying token as described in [calculating the underlying token balance](#lend-tokens-calculating-the-underlying-token-balance).
- `LpToken`: LP tokens should be converted to their underlying tokens as described in [calculating the underlying token balance](#lp-tokens-calculating-the-underlying-token-balance).
- `StableLpToken`: Stable LP tokens should be converted to their underlying tokens.

### User Liabilities

Borrowed tokens are stored in the `loans` pallet and can be retrieved:

```ts
api.query.loans.accountBorrows(<currency>, <account_id>)
```

This returns an object:

```ts
// For example, Token.KSM for a single account
{
  principal: 100,000,000,000,000
  borrowIndex: 1,000,000,000,000,000,000
}
```

The `principal` is the number of tokens that the user has borrowed *without interest*. The `borrowIndex` is the `usersBorrowIndex` and is specific to that user.

To calculate the liability, the *global* `globalBorrowIndex` must be retrieved:

```ts
api.query.loans.borrowIndex(<currency>)`
```

The liability is then:

```python
principal * globalBorrowIndex / userBorrowIndex
```

## Bring your own fees

Users can pay for transaction fees with the native currency (INTR on Interlay and KINT on Kintsugi), or they can pay with assets listed in the AMM pools.

All extrinsics send via `api.tx` use INTR or KINT as the fee currency by default.

Paying with an asset listed in the AMM pools is achieved by wrapping the transaction through the `multiTransactionPayment` pallet. Under the hood, this performs a swap from the selected currency into the native currency.

```ts
api.tx.multiTransactionPayment.withFeeSwapPath(
  path: [
    {ForeignAsset: 2}, // paying with USDT
    {Token: 'INTR'} // swapping to INTR
  ],
  amountInMax: 412, // amount of USDT to swap
  call: { // transferring DOT on Interlay and paying tx fees in USDT
    api.tx.tokens.transfer('address', {Token: 'DOT'}, amount)
  }
)
```
