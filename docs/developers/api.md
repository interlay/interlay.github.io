# APIs

## RPC API

Connecting to parachain nodes:

* **Kintsugi (by Kintsugi Labs)**: `wss://api-kusama.interlay.io/parachain`
* **Kintsugi (by OnFinality)**: `wss://kintsugi.api.onfinality.io/public-ws`
* **Testnet**: `wss://api.interlay.io/parachain`

## Asset API

Querying account balances:

* **KINT**: `api.query.tokens.accounts('address', {Token: 'KINT'})`
* **kBTC**: `api.query.tokens.accounts('address', {Token: 'KBTC'})`
* **KSM**: `api.query.tokens.accounts('address', {Token: 'KSM'})`
* **INTR**: `api.query.tokens.accounts('address', {Token: 'INTR'})`
* **interBTC**: `api.query.tokens.accounts('address', {Token: 'INTERBTC'})`
* **DOT**: `api.query.tokens.accounts('address', {Token: 'DOT'})`
