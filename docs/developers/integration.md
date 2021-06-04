# Interacting with interBTC

## Overview

The interBTC system consists of a range of different components.

![Components Overview](../_assets/img/developers/components.svg)

*Overview of interBTC bridge components.*

## JavaScript

Building new apps directly on interBTC is enabled by using a dedicated TypeScript SDK.

### Installation

Follow the instructions at [polkabtc-js](https://github.com/interlay/polkabtc-js).

### Usage

Create an API instance to the interBTC chain with:

```js
import { createPolkabtcAPI } from "@interlay/polkabtc";

const defaultParachainEndpoint = "wss://beta.polkabtc.io/api/parachain";
const isMainnet = false;
const api = await createPolkabtcAPI(defaultParachainEndpoint, isMainnet);
```

You are now able to issue and redeem new interBTC.

### Examples

#### 1. Attach an account

Most actions require you to use an account on the interBTC bridge. Add either a default one or import your own:

```js
keyring = new Keyring({ type: "sr25519" });
alice = keyring.addFromUri("//Alice");
api.setAccount(alice);
```

#### 2. Display existing issue requests

Once you have instantiated the API and attached the account, loading issue requests is as simple as:

```js
const issueRequests = await issueAPI.list();
```

#### 3. Issue interBTC

After you instantiated the interBTC API as described above, you can request to issue interBTC:

```js
// denoted in satoshi
const amount = api.createType("Balance", 100000) as interBTC;
const requestResult = await issueAPI.request(amount);
```

Next, you need to send BTC to the vault. You could use [bitcoinlib-js](https://github.com/bitcoinjs/bitcoinjs-lib) to achieve this programmatically or use the data from the `issueRequest` object to display the Vault's BTC address and the required amount.

After you have sent, the BTC you can execute the request by fetching the required BTC transaction inclusion proof and sending this information to the interBTC bridge:

```js
const btcTxId = await api.btcCore.getTxIdByRecipientAddress(issueRequest.vaultBTCAddress);
await api.issue.execute(requestResult.id, btcTxId);
```

### More Examples

For fully working examples, head over to our [integration tests](https://github.com/interlay/polkabtc-js/tree/master/test/integration). These are automated and fully integrated with Bitcoin test clients and the interBTC bridge.
