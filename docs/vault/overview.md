# Vaults

Vaults are the heart of the Interlay and Kintsugi bridge. By safeguarding BTC in their wallets, they maintain the physical 1:1 relationship between BTC and IBTC/KBTC. In addition, vaults provide financial collateral to ensure users have insurance to receive their BTC back.

## Introduction

Vaults are **non-trusted** and **collateralized**. **Any user can become a Vault** by providing collateral. Users can freely choose any Vault or be their own Vault. Running their Vault means users don’t have to trust anyone else if they want to be extra cautious.

The bridge enforces the correct behavior of Vaults. Specifically, Vaults must prove correct behavior to the BTC-Relay component - a Bitcoin SPV client implemented directly on top of the bridge. If a Vault fails to fulfill a redeem request the Vault may lose its collateral which can be reimbursed to the user (at a beneficial rate).

The secondary responsibility of a Vault is to monitor both Bitcoin and the bridge to ensure that the BTC-Relay stays up to date with the Bitcoin blockchain by relaying Bitcoin block headers. BTC-Relay is self-healing and automatically detects and recovers from Bitcoin forks.

### What do Vaults do?

1. **Provide Collateral** and upload their Bitcoin public key to the bridge. The amount of collateral provided determines how much BTC the Vault can accept for safekeeping. Collateral is provided in assets white-listed by Governance.
2. **Issue**: Vaults receive BTC from users for safekeeping. This locks the Vault's collateral until BTC is redeemed again.
3. **Redeem**: Vaults monitor the Interlay/Kintsugi bridge for redeem requests. When a user requests to redeem IBTC, Vaults release BTC to the user and prove that they behaved correctly via the BTC-Relay. Only if this proof is correct, the Vault's collateral is unlocked.

To support the integrity of the bridge, Vaults are also able to assume the role of a Relayer:

1. **Maintain BTC-Relay**: submit Bitcoin block headers to BTC-Relay and make sure the bridge stays up to date with the Bitcoin mainchain.

### Why operating a Vault?

1. **Earning potential:**

    - *IBTC/KBTC fees*: All Vaults are part of a fee pool and earn fees in IBTC/KBTC when any user issues or redeems IBTC/KBTC.
    - *KINT/INTR*: Vaults receive a KINT/INTR block reward.
    - *Interest-generating Collateral* (planned feature): Subject to governance, Vaults are able to provide collateral in interested genrating assets such as staking derivatives (e.g., LKSM, LDOT) and LP tokens.

2. **Self-custody:** Vaults hold BTC of users in custody. If you are a large liquidity provider, you can be your own vault and retain custody over your BTC holdings until you exchange IBTC/KBTC.

### What do I need to become a Vault?

1. Vault client ([source](https://github.com/interlay/interbtc-clients))
2. Bitcoin node ([instructions](https://bitcoin.org/en/full-node))
3. Polkadot account ([public/private keypair](https://wiki.polkadot.network/docs/en/learn-keys))
4. Collateral like KSM or DOT as well as other collateral assets.
5. Native chain tokens like KINT and INTR to pay for transaction fees.

Head over to ["Installation"](/vault/installation) for a detailed setup guide.

### Risks

Running a Vault and providing liquidity to Interlay or Kintsugi comes with risks. Please research and understand the risks.

1. **Exchange Rate and Collateralization**: Vaults provide collateral to back locked BTC. If the collateralization falls below the liquidation collateral threshold, the Vault is liquidated. In case of a liquidation, the [Vault's collateral is seized](vault/overview?id=severe-undercollateralization). Vaults with different collateral assets are [isolated](vault/overview?id=vault-isolation). This means that if, e.g., a Vault operator uses the same account id to run a DOT and USDC Vault, liquidating the DOT vault has no impact on the liquidation risk of the USDC Vault. [Learn how to maintain your collateralization here](vault/guide?id=managing-collateral).
2. **Vault Client Offline**: If a Vault fails to process a redeem request from a user within the given time limit, then part or all of the Vaults collateral is slashed depending on the size of the redeem request. See [failed redeem requests for more details](vault/overview?id=failed-redeem). [The Vault uptime requirement is specified here](vault/installation?id=uptime).
3. **Bitcoin Fee Fluctution**: Bitcoin fees fluctuate. The Interlay and Kintsugi chain use an oracle to submit the current Bitcoin fee estimates that Vault take into account when sending BTC. However, the actual fees that the Vault's Bitcoin wallet choose might differ from the estimate given by the Interlay or Kintsugi chain. Vault operators need to ensure that they have at least the same amount of BTC in their Bitcoin wallets as the amount that their Vault has locked on the Interlay or Kintsugi chain. [You can check this following the guide here](vault/guide?id=bitcoin-balance-check).
4. **Software Bugs**: The Interlay teams seeks to eliminate software bugs as much as possible but it is impossible to exclude software risks completely. Using the Vault client is optional (but recommended). All actions the Vault client executes can also be done manually. Our [Interlay/Kintsugi chain](https://github.com/interlay/interbtc) and [Vault client](https://github.com/interlay/interbtc-clients/tree/master/vault) are open-source. The code has been [audited by NCC Group, Informal Systems, and Quarkslab](https://github.com/interlay/interbtc/tree/master/docs/audits). We collaborate with [Immunefi for our bug bounty program](https://immunefi.com/bounty/interlay/).

## Fee Model

Vaults earn fees on issue and redeem, based on the BTC volume.

### Pool-based Fee Distribution

Vaults earn fees based on the issued and redeemed BTC volume. To reduce variance of payouts, the bridge implements a **pooled fee model**.

Each time a user issues or redeems interBTC, they pay the following fees to a **global fee pool**:

- **Issue Fee**: `0.5%` of the Issue volume, paid in *IBTC/KBTC*
- **Redeem**: `0.5%` of the redeem volume, paid in *IBTC/KBTC*

From this fee pool, `100%` is distributed among all active Vaults based on the Vault's **BTC in custody** ( = issued IBTC/KBTC) in proportion to the total locked BTC (= issued IBTC/KBTC) across all Vaults

Specifically, each Vault's fee is calculated according to the following formula:

    vault_fee =
    pool * (vault_locked_btc / total_locked_btc)

The Vault fee is paid each time an Issue or Redeem request is executed.

### Vault Block Rewards

Vaults receive governance tokens as fees for keeping BTC locked and providing the required insurance collateral in whitelisted assets. Early Vaults receive more rewards as they take up higher risk in terms of protocol maturity.

#### Kintsugi

For the full details of the Vault rewards on the Kintsugi canary network, see the [Kintsugi token economy paper](https://raw.githubusercontent.com/interlay/whitepapers/master/Kintsugi_Token_Economy.pdf) published by Kintsugi Labs.

#### Interlay

For the full details of the Vault rewards on the Interlay network, see the [Interlay token economy paper](https://raw.githubusercontent.com/interlay/whitepapers/master/Interlay_Token_Economy.pdf) published by Kintsugi Labs.

## Collateral

To ensure Vaults have no incentive to steal user's BTC, Vaults provide collateral in whitelisted assets - following a similar process as [MakerDAO](https://docs.makerdao.com/smart-contract-modules/collateral-module). To mitigate exchange rate fluctuations, Interlay and Kintsugi employ *over-collateralization* and a *multi-level collateral balancing* scheme.

### Minimum collateral

Each currency & network has different minimum deposits, noted here:

<!-- tabs:start -->

#### **Testnet-Kintsugi**

* Testnet KSM: 0
* Testnet KINT: 0

#### **Testnet-Interlay**

* Testnet DOT: 0
* Testnet INTR: 0

#### **Kintsugi (Canarynet)**

* KSM: 3
* KINT: 55
* LKSM: 20

#### **Interlay (Mainnet)**

* DOT: 30

<!-- tabs:end -->

### Multi-Collateral System

The parachain supports the usage of different assets for usage as collateral. Governance white-lists asset that are accepted as collateral, specifying the various safety thresholds, as well as the maximum supply for each asset.

Vaults are identified by a unique `VaultId` that is a tuple of:

``(AccountId, CollateralCurrency, WrappedCurrency)``

where `CollateralCurrency` is the collateral asset used by this Vault, and `WrappedCurrency` is the 1:1 backing asset (BTC).

?> This distinction between `AccountId` and `VaultId` allows us to easily add *more collateral assets*, as well as *backing assets other than BTC*, such as Litecoin, Dogecoin, ZCash, etc.

#### Vault Isolation

A vault operator can run multiple vaults with different `VaultId`s with different collateral currencies using the same `AccountId`.
Each Vault identified by a unique `VaultId` is isolated from all other Vaults.

?> At any given time there should only be one vault client running for any given `AccountId`. Having multiple vault clients running and using the same `AccountId` can lead to double payments (e.g. on redeem requests).

This means:

- Liquidations only affect a specific `VaultId`.
- Vault operators must take pro-active measures to re-balance between different collateral assets

When users requests to mint IBTC/KBTC, they selects a specific `VaultId` to lock BTC with. Typically, users will not care with which Vault they want to mint with (unless there is a competitive fee market in the future) and will accept the automatic selection offered by the UI.

When redeeming interBTC for BTC, users again select a specific `VaultId`. Here, the selection is *security relevant*: If Vaults fails to execute redeem requests, users have the right to

- (a) retry with another Vault and claim a small penalty in the Vault's collateral (the `CollateralCurrency` associated with the `VaultId`);
- (b) to request reimbursement in that specific Vault’s `CollateralCurrency`.

#### Multiple VaultIDs - One Vault Client

The [Vault client](https://github.com/interlay/interbtc-clients/tree/master/vault) manages all `VaultId`s associated with a given `AccountId`. This means, a Vault operator only needs to run *one off-chain client*.

Vault operators can register new `VaultIds` through the UI and the Vault client will automatically start to manage these.

### Over-collateralization

Vaults must over-collateralize their BTC holdings. The exact threshold is thereby determined for each accepted collateral asset. The up-to-date collateral thresholds can be checked by accessing the parachain storage, e.g. via [Polkadot.js Apps](https://polkadot.js.org/apps/#/explorer).

**Example used for explanation:** In the following, we use DOT collateral with an over-collateralization rate of `150%` **as example**.

This means, the amount of BTC a Vault can accept for safekeeping is calculated by:

    max_vault_btc = vault_dot_collateral / (dot_collateral_threshold * btc_dot_exchange_rate)

Where `dot_collateral_threshold = 1.5` (`150%`) according to our example.

### Vault-Level Collateral Re-balancing

To protect against short and long term exchange rate fluctuations, Vaults are **instructed to keep their collateralization rate up to date**.
This is achieved in three ways:

- **Increase Collateral** - *instant collateral increase*: the Vault can add more collateral to the system. This increases the collateralization immediatly.
- **Redeem** - *collateral increase after 3 to 36 hours* (up to the redeem period - up to 24 hours): if users redeem with the Vault, the collateralization ratio increases. However, the collateral only increases when the redeem request is executed. The lower bound for this are the required Bitcoin and parachain confirmation. In practice, it will take at least 3 hours until such a request is processed. The Vault can also maintain an IBTC/KBTC reserve and execute self-redeems for quick rebalancing.
- **Replace** - *possible collateral increase*: A Vault can request to be replaced by another Vault. By sending such a request to the parachain, the Vault offers other Vaults to take over the locked BTC. This strategy should work well when there is free capacity to issue new BTC in the system as Vaults will try to maximize their locked BTC to increase their share of fees and block rewards. However, there is no guarantee that a Vault will accept the replace request. Especially, when the bridge has little to no capacity to issue new BTC all Vaults are saturated and it is unlikely that other Vaults will accept the replace request.

## Collateral Thresholds

The Interlay and Kintsugi bridges introduces multiple thresholds with different actions to ensure Vaults never drop below 100% collateralization:

### Secure collateral

#### Actions

None necessary. The Vault can freely withdraw any "unused" collateral above the secure threshold.

#### Thresholds

<!-- tabs:start -->

#### **Testnet-Kintsugi**

* Testnet KSM: `150%`
* Testnet KINT: `400%`

#### **Testnet-Interlay**

* Testnet DOT: `150%`
* Testnet INTR: `400%`

#### **Kintsugi (Canarynet)**

* KSM: `260%`
* KINT: `900%`
* LKSM: `260%`

#### **Interlay (Mainnet)**

* DOT: `260%`

<!-- tabs:end -->

### Premium Redeem

#### Actions

Users can execute redeem with this Vault and receive a premium of `5%` in the collateral asset (e.g., KSM or DOT) in addition to the redeemed BTC.

#### Thresholds

<!-- tabs:start -->

#### **Testnet-Kintsugi**

* Testnet KSM: `135%`
* Testnet KINT: `300%`


#### **Testnet-Interlay**

* Testnet DOT: `135%`
* Testnet INTR: `300%`

#### **Kintsugi (Canarynet)**

* KSM: `200%`
* KINT: `650%`
* LKSM: `200%`

#### **Interlay (Mainnet)**

* DOT: `200%`

<!-- tabs:end -->

### Vault Liquidation

#### Action

The undercollateralized Vault is liquidated.

1. The Vaults entire collateral is slashed
2. The bridge initiates a first-come-first-served liquidation swap: any user can **burn IBTC/KBTC** in return for collateral at a premium rate. See **[Burn Event](/vault/overview?id=burn-event-restoring-a-11-physical-peg)** below.

#### Thresholds

<!-- tabs:start -->

#### **Testnet-Kintsugi**

* Testnet KSM: `110%`
* Testnet KINT: `200%`

#### **Testnet-Interlay**

* Testnet DOT: `110%`
* Testnet INTR: `200%`

#### **Kintsugi (Canarynet)**

* KSM: `150%`
* KINT: `500%`
* LKSM: `150%`

#### **Interlay (Mainnet)**

* DOT: `150%`

<!-- tabs:end -->

### Checking Thresholds On-Chain

While we endeavour to keep the docs up to date and clearly communicate and announce any changes, the source of truth is ultimately the on-chain data. To verify for yourself that the values above are correct, follow these steps:

1. Go to polkadot.js.org/apps
2. Select the appropriate network - Interlay (under "Polkadot and parachains"), Kintsugi (under "Kusama and parachains") or Interlay Testnet (under "Test networks")
3. Go to the Developer -> Chain State page
4. Select the `vaultRegistry` pallet and either `secureCollateralThreshold`, `premiumRedeemThreshold` or `liquidationCollateralThreshold`
5. Select the desired collateral currency - e.g. either KSM or KINT on Kintsugi, DOT on Interlay, etc.
6. Select `KBTC` as the wrapped asset on Kintsugi, and `IBTC` on Interlay
7. Click the `+` icon to run the query
8. Divide the displayed value by 10^16 (10000000000000000) to obtain the percentage - e.g. a return of `3,000,000,000,000,000,000` means a 300% threshold

## Liquidations

If Vaults fail to behave according to protocol rules, they face punishment through liquidation of collateral - specifically, if a Vault fails to execute a redeem on time or falls below the liquidation collateral threshold.

### Failed Redeem

If Vaults go offline and fail to execute redeem, there are two possible outcomes.

**Outcome 1: User retries with another Vault**

After the redeem request expires, the user can cancel it and decide to retry with another Vault. In this case:

- **A punishment fee** is slashed from the Vaults collateral and paid to the user, and
- **A temporary ban** is incurred upon the Vault e.g. `24 hours` from accepting further requests.

**Outcome 2: User liquidates (part of) Vault**

After the redeem request expires, the user can cancel it and decide to liquidate (part of) the Vault. In this case:

- **A punishment fee** is slashed from the Vaults collateral and paid to the user,
- **Vault collateral is slashed** at the current spot collateral-to-BTC exchange rate and paid to the user,
- **A temporary ban** is incurred upon the Vault e.g. `24 hours` from accepting further requests (if it still has collateral left), and
- **The Vault keeps the BTC**.

### Severe Undercollateralization

If a Vaults drops below the liquidation collateral threshold, the **the Vault's entire remaining collateral is liquidated** and BTC holdings are considered lost (i.e., the Vault gets to keep the BTC).
Consequently, the interBTC bridge initiates a **[Burn Event](/vault/overview?id=burn-event-restoring-a-11-physical-peg)** to restore the 1:1 balance between BTC and interBTC.

**What causes undercollateralization?**
Severe undercollateralization might occur when either the collateral asset (e.g. KSM) and the wrapped asset (e.g. BTC) exchange rates as reported by the oracle are changing. A Vault is liquidated when either (1) the the collateral (e.g., KSM) exchange rate drops significantly in relation to the wrapped asset (e.g., BTC) or (2) the wrapped asset (e.g., BTC) exchange rate rises significantly in relation to the collateral (e.g., KSM).

(1) and (2) are ultimately the same thing, but possibly it helps to think about it in KSM/USD and BTC/USD rates, which means that if either (1) or (2) or both (1) and (2) happen at the same time, a Vault is liquidated.

**Example**

*Liquidation Price*

Let's say a Vault has currently 1000 KSM collateral and 1 BTC locked (the wrapped asset). Let's also assume that this represents a collateralization of 200%. And let's use 1000 KSM = $100,000 (i.e., $100/KSM) and 1 BTC = $50,000 for simplicity. Last, let's say the liquidation threshold is at 150% collateralization, so when a Vault is below this threshold, the Vault will be automatically liquidated.

The liquidation price is reached when:

- Case (1) from above: 1000 KSM = $75,000 -> $75k of collateral are backing $50k of BTC -> 150% collateralization -> liquidation price for KSM is at $75/KSM assuming only the KSM price moves
- Case (2) from above: 1 BTC = $66,666 -> $100k of collateral are backing $66k of BTC -> 150% collateralization -> liquidation price for BTC is at $66,666/BTC assuming only the BTC price moves.

Note that if (1) and (2) happen at the same time, i.e., BTC price rises and KSM price drops, liquidations might happen earlier so the liquidation price updates every time the price in the exchange rate oracle changes.

*Value at risk*

The value at risk is the current collateral in the Vault minus the BTC it has locked. The BTC locked is subtracted as the Vault gets to keep the BTC when it is liquidated.

The value at risk in the example above is the value of KSM in the Vault minus the value of BTC the Vault holds. If the KSM price drops in case (1), value at risk would go from $50k ($100k KSM minus $50k BTC) to $75k ($75k KSM minus $50k BTC) whereas in case (2) value at risk goes from $50k ($100k KSM minus $50k BTC) to $33,333 ($100k KSM minus $66,666 BTC).

## Burn Event: Restoring a 1:1 Physical Peg

When a Vault is liquidated, its collateral is slashed up to the secure collateral threshold (e.g., `150%` on testnet) of the liquidated BTC value, given the exchange rate at the time of liquidation.

The interBTC bridge now has less BTC locked than interBTC minted - but more than enough collateral to maintain economic security.
To re-establish the physical 1:1 peg between BTC and interBTC, the interBTC bridge allows users to **burn interBTC in return for collateral at a premium rate**.

Specifically, the user's payout is calculated as follows:

    burn_payout =
        (total_liquidated_collateral / total_liquidated_interbtc)
        * user_burned_interbtc

As long as the economic value of `burn_payout` is higher than that of `user_burned_interbtc`, which may include private information of the user (that is, the user may think that the collateral asset will become worth more soon), users are incentivized to burn interBTC in return for the collateral asset and to re-balance the system.

This Burn Event continues until the 1:1 ratio of BTC to interBTC is restored.
