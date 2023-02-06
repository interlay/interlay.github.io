# Bridge Development

?> All guides here are also applicable to Kintsugi and any Interlay or Kintsugi testnet.

## Issue and Redeem

### Understanding Issue Request

Bridging BTC to the Interlay network is done in four high-level steps:

Precondition: a Vault has locked collateral as described in the Vault Registry.

1. A user executes the `requestIssue` function to open an issue request on the Interlay chain. The issue request includes the amount of iBTC the user wants to issue, the selected Vault, and a collateral reserve to prevent [Griefing: https://spec.interlay.io/security_performance/xclaim-security.html#griefing).
2. A user sends the equivalent amount of BTC to issue to the Vault on the Bitcoin blockchain.
3. The user or Vault acting on behalf of the user extracts a transaction inclusion proof of that locking transaction on the Bitcoin blockchain. The user or a Vault acting on behalf of the user executes the `executeIssue` function on the Interlay chain. If the function completes successfully, the user receives the requested amount of interBTC into his account.
4. Optional: If the user is not able to complete the issue request within the predetermined time frame (`IssuePeriod`), the Vault is able to call the `cancelIssue` function to cancel the issue request and will receive the griefing collateral locked by the user.

#### Issue Request States

In more detail, an issue request can take the following states from the perspective of the [UI: https://github.com/interlay/interbtc-ui/), [lib: https://github.com/interlay/interbtc-api), and the [squid caching layer: https://github.com/interlay/interbtc-squid):

```ts
enum IssueStatus {
    Completed,
    Cancelled,
    Expired,
    PendingWithBtcTxNotFound,
    PendingWithBtcTxNotIncluded,
    PendingWithTooFewConfirmations,
    PendingWithEnoughConfirmations,
}
```

?> For the Interlay chain issue request status mapping, please refer to the [spec: https://spec.interlay.io/spec/issue.html).

The states are mapped to a more fine-grained process as follows. This is an extension to the high-level steps above.

1. A user requests to mint iBTC by reserving the Vault's collateral on the Interlay chain. Each issue requests starts with `PendingWithBtcTxNotFound`.
2. User sends BTC to a [uniquely generated address: https://spec.interlay.io/security_performance/xclaim-security.html#unique-addresses-via-on-chain-key-derivation) of the Vault.

  a. When the BTC transaction is just sent and is still in the Bitcoin mempool, the status of the issue request is `PendingWithBtcTxNotIncluded`

  b. When the BTC transaction is included in a Bitcoin block AND the Bitcoin block is already included in the BTC-Relay on the Interlay chain, but either the BTC transaction OR the BTC block in the BTC-Relay do [not have enough confirmations](https://spec.interlay.io/security_performance/btcrelay-security.html#security-parameter-k), the issue status is `PendingWithTooFewConfirmations`.

  c. When the BTC transaction is included with [sufficient Bitcoin confirmations](https://spec.interlay.io/spec/btc-relay/data-model.html?highlight=confirmation#stable-bitcoin-confirmations) AND the [BTC block has sufficient parachain confirmations](https://spec.interlay.io/spec/btc-relay/data-model.html?highlight=confirmation#stable-parachain-confirmations), then the issue status is `PendingWithEnoughConfirmations`.

  d. At this point, the issue request can be executed.

3. Someone executes the issue request by submitting a [Bitcoin transaction inclusion proof](https://spec.interlay.io/spec/btc-relay/functions.html#verifytransactioninclusion) to the Interlay chain within the [time limit](https://spec.interlay.io/spec/issue.html#issueperiod).

  a. If the BTC amount sent by the user matches OR is higher than the requested amount in the original issue request, anyone (user or Vault) can execute the request. In most cases, the Vault will automatically execute the request. However, the user can at manually execute the request as well. The issue status is changed to `Completed`.

  b. If the BTC amount sent bny the user is less than the request amount in the original issue request, the user MUST execute the request. If the user executes the request, the issue request status changes to `Completed`.

4. Optional: If nobody executes the issue requests within the [time limit](https://spec.interlay.io/spec/issue.html#issueperiod), the Vault will cancel the issue request, taking the user's griefing collateral and thereby setting the issue request status to `Cancelled`.

  a. If a Vault has sufficient capacity AND a user manually executes the issue request with a valid Bitcoin transaction inclusion proof, the issue request status is changed to `Completed`.

### Understanding Redeem Request

Bridging BTC from the Interlay network is done in four high-level steps:

Precondition: a user owns iBTC.

1. A user locks an amount of iBTC by calling the `requestRedeem` function. In this function call, the user selects a vault to execute the redeem request from the list of Vaults. The function creates a redeem request with a unique hash.
2. The selected Vault listens for the `RequestRedeem` event emitted from the Interlay chain by the user. The Vault then proceeds to transfer BTC to the address specified by the user in the `requestRedeem` function [including a unique hash in the OP_RETURN: https://spec.interlay.io/security_performance/xclaim-security.html#op-return) of one output.
3. The vault executes the `executeRedeem` function by providing the Bitcoin transaction from step 2 together with the redeem request identifier within the [time limit: https://spec.interlay.io/spec/redeem.html#redeemperiod). If the function completes successfully, the locked iBTC are destroyed and the user received its BTC.
4. Optional: If the user could not receive BTC within the given time (as required in step 3), the user calls `cancelRedeem` after the redeem time limit. The user can choose either to reimburse or to retry:

  a. Reimbursement: the user forwards the iBTC tokens to the Vault and receives collateral in exchange of the same value as the burned iBTC. The user also gets a [punishment fee: https://spec.interlay.io/spec/fee.html#punishmentfee) based on the requested redeem amount paid in the collateral of the Vault.

  b. Retry: the user gets back its iBTC. The user also gets a [punishment fee: https://spec.interlay.io/spec/fee.html#punishmentfee) based on the requested redeem amount paid in the collateral of the Vault.

5. Optional: If during a `cancelRedeem` the user selects reimbursement AND as a result the Vault falls below the [`SecureCollateralThreshold`: https://spec.interlay.io/spec/vault-registry.html#securecollateralthreshold), then the Vault does not receive the user’s tokens - they are burned, and the Vault’s `issuedTokens` decreases. When, at some later point, the Vault gets sufficient collateral, it can call `mintTokensForReimbursedRedeem` to get the previously burned iBTC.

#### Redeem Request States

In more detail, an redeem request can take the following states from the perspective of the [UI: https://github.com/interlay/interbtc-ui/), [lib: https://github.com/interlay/interbtc-api), and the [squid caching layer: https://github.com/interlay/interbtc-squid):

```ts
enum RedeemStatus {
    Completed,
    Expired,
    Reimbursed,
    Retried,
    PendingWithBtcTxNotFound,
    PendingWithBtcTxNotIncluded,
    PendingWithTooFewConfirmations,
    PendingWithEnoughConfirmations,
}
```

?> For the Interlay chain redeem request status mapping, please refer to the [spec: https://spec.interlay.io/spec/redeem.html).

The states are mapped to a more fine-grained process as follows. This is an extension to the high-level steps above.

1. A user locks an amount of iBTC by calling the `requestRedeem` function. In this function call, the user selects a vault to execute the redeem request from the list of Vaults. The function creates a redeem request with a unique hash. Each redeem request starts with `PendingWithBtcTxNotFound`.
2. The selected Vault listens for the `RequestRedeem` event emitted from the Interlay chain by the user. The Vault then proceeds to transfer BTC to the address specified by the user in the `requestRedeem` function [including a unique hash in the OP_RETURN](https://spec.interlay.io/security_performance/xclaim-security.html#op-return) of one output.

  a. When the BTC transaction is just sent and is still in the Bitcoin mempool, the status of the redeem request is `PendingWithBtcTxNotIncluded`

  b. When the BTC transaction is included in a Bitcoin block AND the Bitcoin block is already included in the BTC-Relay on the Interlay chain, but either the BTC transaction OR the BTC block in the BTC-Relay do [not have enough confirmations](https://spec.interlay.io/security_performance/btcrelay-security.html#security-parameter-k), the redeem status is `PendingWithTooFewConfirmations`.

  c. When the BTC transaction is included with [sufficient Bitcoin confirmations](https://spec.interlay.io/spec/btc-relay/data-model.html?highlight=confirmation#stable-bitcoin-confirmations) AND the [BTC block has sufficient Interlay chain confirmations](https://spec.interlay.io/spec/btc-relay/data-model.html?highlight=confirmation#stable-parachain-confirmations), then the redeem status is `PendingWithEnoughConfirmations`.

  d. At this point, the redeem request can be executed.
3. The vault executes the `executeRedeem` function by providing the Bitcoin transaction from step 2 together with the redeem request identifier within the [time limit](https://spec.interlay.io/spec/redeem.html#redeemperiod). If the function completes successfully, the locked iBTC are destroyed and the user received its BTC. The redeem request status is `Confirmed`.
4. Optional: If the user could not receive BTC within the given time (as required in step 3), the user calls `cancelRedeem` after the redeem time limit. The redeem request status is set to `Expired`. The user can choose either to reimburse or to retry:

  a. Reimbursement: the user forwards the the iBTC tokens to the Vault and receives collateral in exchange of the same value as the burned iBTC. The user also gets a [punishment fee](https://spec.interlay.io/spec/fee.html#punishmentfee) based on the requested redeem amount paid in the collateral of the Vault. The redeem request status is set to `Reimbursed`.

  b. Retry: the user gets back its iBTC. The user also gets a [punishment fee](https://spec.interlay.io/spec/fee.html#punishmentfee) based on the requested redeem amount paid in the collateral of the Vault. The redeem request status is set to `Retried`.
