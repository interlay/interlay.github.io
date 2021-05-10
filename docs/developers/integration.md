# Interacting with PolkaBTC

## Overview

The PolkaBTC system consists of a range of different components.

![Components Overview](../_assets/img/developers/components.svg)

*Overview of PolkaBTC bridge components.*

## JavaScript

Building new apps directly on PolkaBTC is enabled by using a dedicated TypeScript SDK.

### Installation

Follow the instructions at [polkabtc-js](https://github.com/interlay/polkabtc-js).

### Usage

Create an API instance to the PolkaBTC chain with:

```js
import { createPolkabtcAPI } from "@interlay/polkabtc";

const defaultParachainEndpoint = "wss://beta.polkabtc.io/api/parachain";
const isMainnet = false;
const api = await createPolkabtcAPI(defaultParachainEndpoint, isMainnet);
```

You are now able to issue and redeem new PolkaBTC.

### Examples

#### 1. Attach an account

Most actions require you to use an account on the PolkaBTC bridge. Add either a default one or import your own:

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

#### 3. Issue PolkaBTC

After you instantiated the PolkaBTC API as described above, you can request to issue PolkaBTC:

```js
// denoted in satoshi
const amount = api.createType("Balance", 100000) as PolkaBTC;
const requestResult = await issueAPI.request(amount);
```

Next, you need to send BTC to the vault. You could use [bitcoinlib-js](https://github.com/bitcoinjs/bitcoinjs-lib) to achieve this programmatically or use the data from the `issueRequest` object to display the Vault's BTC address and the required amount.

After you have sent, the BTC you can execute the request by fetching the required BTC transaction inclusion proof and sending this information to the PolkaBTC bridge:

```js
const btcTxId = await api.btcCore.getTxIdByRecipientAddress(issueRequest.vaultBTCAddress);
await api.issue.execute(requestResult.id, btcTxId);
```

### More Examples

For fully working examples, head over to our [integration tests](https://github.com/interlay/polkabtc-js/tree/master/test/integration). These are automated and fully integrated with Bitcoin test clients and the PolkaBTC bridge.
