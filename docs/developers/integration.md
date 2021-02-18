# Interacting with PolkaBTC

## Overview

The PolkaBTC system consists of a range of different components.

?> _TODO_ Add the components

## JavaScript

Building new apps directly on PolkaBTC is enabled by using a dedicated TypeScript SDK.

### Installation

Follow the instructions at [polkabtc-js](git@github.com:interlay/btc-parachain-spec.git).

### Usage

Create an API instance to the PolkaBTC chain with:

```js
import { createPolkabtcAPI } from "@interlay/polkabtc";

const defaultParachainEndpoint = "wss://beta.polkabtc.io/api/parachain";
const isMainnet = false;
const polkaBTC = await createPolkabtcAPI(defaultParachainEndpoint, isMainnet);
```

You are now able to issue and redeem new PolkaBTC.

?> _TODO_ Need to add guides on issue and redeem.

## Substrate

?> _TODO_ Add description on how to integrate with XCM etc.



