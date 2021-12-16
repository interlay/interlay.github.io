# APIs

## RPC API

Connecting to parachain nodes:

* **Kintsugi (by Kintsugi Labs)**: `wss://api-kusama.interlay.io/parachain`
* **Kintsugi (by OnFinality)**: `wss://kintsugi.api.onfinality.io/public-ws`
* **Testnet**: `wss://api.interlay.io/parachain`

## Asset API

### Overview of assets

| Asset    | CURRENCY_ID | CURRENCY_INDEX | DECIMALS |
|----------|-------------|----------------|----------|
| DOT      | DOT         | 0              | 10       |
| interBTC | INTERBTC    | 1              | 8        |
| INTR     | INTR        | 2              | 10       |
| KSM      | KSM         | 10             | 12       |
| kBTC     | KBTC        | 11             | 8        |
| KINT     | KINT        | 12             | 12       |

?> For any newly added assets, please refer to the [on-chain implementation](https://github.com/interlay/interbtc/blob/master/primitives/src/lib.rs#L472). We will not update the currency id or index of already published assets.

### Querying account balances

```js
api.query.tokens.accounts('address', {Token: CURRENCY_ID})
```

### Transferring tokens extrinsic

```js
api.tx.tokens.transfer('address', {Token: CURRENCY_ID}, amount)
```
