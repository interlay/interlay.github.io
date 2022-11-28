# APIs

## RPC API

Connecting to parachain nodes:

<!-- tabs:start -->
#### **Interlay**

- `wss://api.interlay.io:443/parachain`
- `wss://interlay.api.onfinality.io/public-ws`

#### **Kintsugi**

- `wss://api-kusama.interlay.io:443/parachain`
- `wss://kintsugi.api.onfinality.io/public-ws`

#### **Testnet (Interlay)**

- `wss://api-testnet.interlay.io:443/parachain`

#### **Testnet (Kintsugi)**

- `wss://api-dev-kintsugi.interlay.io:443/parachain`

<!-- tabs:end -->

## Asset API

### Overview of assets in the `tokens` pallet

The `tokens` pallet is used for Interlay/Kintsugi native assets and the relaychain assets.

| Asset       | CURRENCY_ID | CURRENCY_INDEX | DECIMALS | Multilocation                                       |
|-------------|-------------|----------------|----------|-----------------------------------------------------|
| DOT         | DOT         | 0              | 10       | (Parent, Here)                                      |
| interBTC    | IBTC        | 1              | 8        | (Parent, (X2, Parachain: 2032, GeneralKey: 0x0001)) |
| INTR        | INTR        | 2              | 10       | (Parent, (X2, Parachain: 2032, GeneralKey: 0x0002)) |
| KSM         | KSM         | 10             | 12       | (Parent, Here)                                      |
| kintsugiBTC | KBTC        | 11             | 8        | (Parent, (X2, Parachain: 2092, GeneralKey: 0x000b)) |
| KINT        | KINT        | 12             | 12       | (Parent, (X2, Parachain: 2092, GeneralKey: 0x000c)) |

?> For any newly added assets, please refer to the [on-chain implementation](https://github.com/interlay/interbtc/blob/master/primitives/src/lib.rs#L472). We will not update the currency id or index of already published assets.

### Overview of assets in the `assetRegistry` pallet

The `assetRegistry` pallet is used for assets from other parachains.

| Chain    | Asset        | CURRENCY_ID | ASSET_INDEX | DECIMALS | Multilocation                                       |
|----------|--------------|-------------|-------------|----------|-----------------------------------------------------|
| Interlay | Liquid DOT   | LDOT        | 1           | 10       | (Parent, (X2, Parachain: 2000, GeneralKey: 0x0003)) |
| Interlay | Tether USD   | USDT        | 2           | 6        | (Parent, (X3, Parachain: 1000, PalletInstance: 50, GeneralIndex: 1984)) |
| Kintsugi | Acala Dollar | AUSD        | 1           | 12       | (Parent, (X2, Parachain: 2000, GeneralKey: 0x0081)) |
| Kintsugi | Liquid KSM   | LKSM        | 2           | 12       | (Parent, (X2, Parachain: 2000, GeneralKey: 0x0083)) |
| Kintsugi | Tether USD   | USDT        | 3           | 6        | (Parent, (X3, Parachain: 1000, PalletInstance: 50, GeneralIndex: 1984)) |

!> Before using foreign assets in your application or interacting with them via polkadot.js directly, please verify the correctness of the data above for the network you are trying to use via [https://polkadot.js.org/apps/#/chainstate](https://polkadot.js.org/apps/#/chainstate) -> `assetRegistry` -> `metadata()`.

### Querying account balances

Returns the `amount` of tokens in the smallest denomination.

```js
// for asset in the tokens pallet
api.query.tokens.accounts('address', {Token: CURRENCY_ID});
// for asset in the asset registry pallet
api.query.tokens.accounts('address', {ForeignAsset: ASSET_INDEX});
```

### Transferring tokens extrinsic

Transfers an `amount` of tokens in the smallest denomination.

```js
// for asset in the tokens pallet
api.tx.tokens.transfer('address', {Token: CURRENCY_ID}, amount)
// for asset in the asset registry pallet
api.tx.tokens.transfer('address', {ForeignAsset: ASSET_INDEX}, amount)
```

### Implementation notes

All assets in Kintsugi and Interlay networks are implemented using [orml](https://github.com/open-web3-stack/open-runtime-module-library).

* Asset implementation: [orml-tokens](https://github.com/open-web3-stack/open-runtime-module-library/tree/master/tokens)
* Cross-chain transfer implementation (XCM): [orml-xtokens](https://github.com/open-web3-stack/open-runtime-module-library/tree/master/xtokens)
* Foreign assets: [orml-asset-registry](https://github.com/open-web3-stack/open-runtime-module-library/tree/master/asset-registry)

?> Querying the `system` account is not supported on Kintsugi or Interlay. Please use the API methods described above to achieve transfers.

## GraphQL APIs

Interlay provides a range of [subsquid](https://github.com/subsquid/squid)-based API endpoints for building front-ends and analytic tools.

<!-- tabs:start -->
#### **Interlay**

- Squid: [https://api.interlay.io/graphql/graphql](https://api.interlay.io/graphql/graphql)
- Archive (hosted by subsquid): [https://interlay.explorer.subsquid.io/graphql](https://interlay.explorer.subsquid.io/graphql)
- Archive (hosted by Interlay Labs): [https://api.interlay.io/subsquid-explorer/graphql](https://api.interlay.io/subsquid-explorer/graphql)

#### **Kintsugi**

- Squid: [https://api-kusama.interlay.io/graphql/graphql](https://api-kusama.interlay.io/graphql/graphql)
- Archive (hosted by subsquid): [https://kintsugi.explorer.subsquid.io/graphql](https://kintsugi.explorer.subsquid.io/graphql)
- Archive (hosted by Kintsugi Labs): [https://api-kusama.interlay.io/subsquid-explorer/graphql](https://api-kusama.interlay.io/subsquid-explorer/graphql)

#### **Testnet (Interlay)**

- Squid: [https://api-testnet.interlay.io/graphql/graphql](https://api-testnet.interlay.io/graphql/graphql)
- Archive: [https://api-testnet.interlay.io/subsquid-explorer/graphql](https://api-testnet.interlay.io/subsquid-explorer/graphql)

#### **Testnet (Kintsugi)**

- Squid: [https://api-dev-kintsugi.interlay.io/graphql/graphql](https://api-dev-kintsugi.interlay.io/graphql/graphql)
-  Archive: [https://api-dev-kintsugi.interlay.io/subsquid-explorer/graphql](https://api-dev-kintsugi.interlay.io/subsquid-explorer/graphql)

<!-- tabs:end -->
