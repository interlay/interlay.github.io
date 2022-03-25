# Operating the Vault Client

Usually, you will not need to interact with your Vault client as it automatically executes its [main tasks](vault/overview?id=what-do-vaults-do).
However, sometimes it is necessary to interact with the Vault client, for example, if you would like to add or withdraw collateral.

At the end of this document you will have:

- [x] Deposited additional collateral
- [x] Withdrawn surplus collateral
- [x] Learned about automatic actions of your Vault
- [x] Visited the Vault dashboard

## Changing Collateral

### Increasing Collateral

**Web UI**

Go to the Vault tab and click on button next to the `Collateral: X DOT for Y BTC` text (above the Issue Requests table). Then, follow the instructions.

**interbtc-js library**

You can use [interbtc-js](https://github.com/interlay/interbtc-js) to lock additional collateral.

```js
import { createinterbtcAPI } from "@interlay/interbtc";

const interbtc = await createinterbtcAPI("ws://127.0.0.1:9944", "testnet");
interbtc.setAccount(KEYRING);

// 100 DOT denominated in Planck
const additionalCollateralInPlanck = "1000000000000";
await interbtc.vaults.lockAdditionalCollateral(additionalCollateralInPlanck);
```

### Withdrawing Collateral

**Web UI**

Go to the Vault tab and click on the button next to the `Collateral: X DOT for Y BTC` text (above the Issue Requests table). Then, follow the isntructions.

**interbtc-js library**

You can use [interbtc-js](https://github.com/interlay/interbtc-js) to withdraw collateral.

```js
import { createinterbtcAPI } from "@interlay/interbtc";

const interbtc = await createinterbtcAPI("ws://127.0.0.1:9944", "testnet");
interbtc.setAccount(KEYRING);

// 100 DOT denominated in Planck
const collateralToWithdrawInPlanck = "1000000000000";
await interbtc.vaults.withdrawCollateral(collateralToWithdrawInPlanck);
```

## Automatic Actions

### Registering your Vault

The default behavior on testnet is **automatic registration** using Interlay's DOT faucet as set in the `auto-register-with-faucet-url` arg. Another option for registering is the `auto-register-with-collateral` flag, as described in the [README](https://github.com/interlay/interbtc-clients/tree/master/vault).

You can also register your Vault through our web UI, going to the "Vault" tab, clicking the `Register` button and completing the steps.

Moreover, you can interact with the Vault pallet directly using [interbtc-js](https://github.com/interlay/interbtc-js).

```js
import { createinterbtcAPI } from "@interlay/interbtc";

const interbtc = await createinterbtcAPI("ws://127.0.0.1:9944", "testnet");
interbtc.setAccount(KEYRING);

// 100 DOT denominated in Planck
const collateralInPlanck = "1000000000000";
await interbtc.vaults.register(collateralInPlanck, BTC_PUBLIC_KEY);
```

### Earning Fees

The Vault client is automatically earning fees as described in the [Fee Model](vault/overview?id=fee-model).

### Accepting Issue and Redeem Requests

Issue and Redeem requests are processed automatically at the moment, signing transactions on the Bitcoin and Polkadot networks using the mnemonic/account credentials you provide to the client when running it.

### Leaving interBTC

The process to leave interBTC depends on whether or not your Vault client holds BTC in custody.

If you Vault has _no BTC in custody_, you can withdraw all your DOT collateral at any time and leave the system. It is safe to stop the Vault client without risking being penalized. You will not participate in any issue or redeem requests once you have removed your DOT collateral.

If your Vault clients holds at least _some BTC in custody_, you have two options to leave the system. Both options require that the BTC that you have in custody is moved. Option A, leaving through _replace_, requires you to request being replaced by another Vault. You can request to be replaced through the [Vault dashboard](https://testnet.interlay.io/vault). Option B, leaving through _redeem_ requires you to wait for a user to redeem the entire amount of BTC that the Vault has in custody. Only after you have 0 BTC, can the Vault client withdraw its entire collateral.

## Self-Minting

The current bridge user interface randomizes Vaults during the issue process.

To mint with your own (or another specific) Vault, you can use the Polkadot.js interface:

1. Select Extrinsics in the Developer tab
2. Select "issue" in the "state query" drop down
3. Select "requestIssue" as the function to be executed
4. Enter your Vault account and the issue amounts.

   -  The BTC amount must be entered with 8 decimals (1 BTC = 100000000)
   -  On Kintsugi, select "KSM" as the collateral token and kBTC as the wrapped token (DOT and interBTC on Interlay)
   -  Enter 1000000000000 (= 1 KINT) as griefing collatral, you will get this back once you complete the issue operation. You can also check see how griefing collateral is needed via the "Issue" UI but 1 KINT will suffice in most cases.


![Screenshot: self-minting via Polkadot.js Developer tab](../_assets/img/guide/self-mint.png)

### Why Self-Mint?

You Vault only starts earning rewards once BTC is locked - and rewards are determined by your share of the total BTC locked in the system. Hence, you can increase your rewards by bringing your own BTC into the system.

## Dashboard

You can monitor the operation of your Vault on the Vault dashboard by adding the key to the [polkadot{.js} extension](https://polkadot.js.org/extension/).

Once the Vault is up and running, a "Vault" tab will appear in the topbar of the app at [testnet.interlay.io](https://testnet.interlay.io/) (or you can access directly at [testnet.interlay.io/vault](https://testnet.interlay.io/vault)).

## Security

For added security, you may want to encrypt the Bitcoin wallet with a password.

<!-- tabs:start -->

#### **Regtest**

```shell
bitcoin-cli -regtest -rpcwallet=interbtcvault encryptwallet "password"
bitcoin-cli -regtest -rpcwallet=interbtcvault walletpassphrase "password" 100000000
```

#### **Testnet**

```shell
bitcoin-cli -testnet -rpcwallet=interbtcvault encryptwallet "password"
bitcoin-cli -testnet -rpcwallet=interbtcvault walletpassphrase "password" 100000000
```

#### **Mainnet**

```shell
bitcoin-cli -rpcwallet=interbtcvault encryptwallet "password"
bitcoin-cli -rpcwallet=interbtcvault walletpassphrase "password" 100000000
```

<!-- tabs:end -->

This will keep the decryption key in memory for the specified timeout - in this example 100000000 seconds or 3 years.
Once this timeout expires (or if the node is terminated) the wallet must be unlocked manually.
