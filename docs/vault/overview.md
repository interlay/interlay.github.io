# Vaults

Vaults are the heart of the interBTC and Kintsugi bridge. They are responsible for maintaining the physical 1:1 peg between BTC and interBTC.
Vaults receive BTC for safekeeping from users and ensure BTC remains locked while interBTC exists

## Introduction

Vaults are **non-trusted** and **collateralized** and **any user can become a Vault** by providing collateral. This means: as a user, you can freely choose any Vault you like or be your own Vault. You don’t have to trust anyone else if you want to be extra cautious.

The correct behavior of Vaults is enforced by the bridge. Specifically, Vaults must prove correct behavior to the BTC-Relay component - a Bitcoin SPV client implemented directly on top of the bridge. If a Vault tries to steal BTC, this will be automatically detected and the Vault will lose its collateral - and users will be reimbursed using this collateral (at a beneficial rate).

The secondary responsibility of a Vault is to monitor both Bitcoin and the bridge to ensure that the BTC-Relay stays up to date with the Bitcoin mainchain by relaying Bitcoin block headers. BTC-Relay is self-healing and automatically detects and recovers from Bitcoin forks.

### What do Vaults do?

1. **Provide Collateral** and upload their Bitcoin public key to the bridge. The amount of collateral provided determines how much BTC the Vault can accept for safekeeping. Collateral can initially be provided in assets white-listed by Governance. Initially KSM on Kintsugi and DOT on Interlay.
2. **Issue**: Vaults receive BTC from users for safekeeping. This locks the Vault's collateral until BTC is redeemed again.
3. **Redeem**: Vaults monitor the interBTC/Kintsugi bridge for redeem requests. When a user requests to redeem interBTC, Vaults release BTC to the user and prove that they behaved correctly via the BTC-Relay. Only if this proof is correct, the Vault's collateral is unlocked.

To support the integrity of the bridge, Vaults are also able to assume the role of a Relayer:

1. **Maintain BTC-Relay**: submit Bitcoin block headers to BTC-Relay and make sure the bridge stays up to date with the Bitcoin mainchain.
2. **Report Vault Theft**: monitor Vault Bitcoin addresses and BTC holdings and report theft to the bridge (providing an SPV proof to BTC-Relay)

### Why operating a Vault?

1. **Earning potential:**

    - *interBTC*: All Vaults are part of a fee pool and earn fees in interBTC when any user issues or redeems interBTC.
    - *KINT* (Kintsugi-only): Vaults receive a KINT block reward.
    - *KSM/DOT* (planned feature): Subject to governance, Vaults are able to provide collateral in liquid staked assets like LKSM or LDOT to receive both staking rewards from the relay chain and the rewards form the Kintsugi/interBTC bridge.

2. **Self-custody:** Vaults hold BTC of users in custody. If you are a large liquidity provider, you can be your own vault and retain custody over your BTC holdings until you sell interBTC.

### What do I need to become a Vault?

1. Vault client ([source](https://github.com/interlay/interbtc-clients))
2. Bitcoin full node ([instructions](https://bitcoin.org/en/full-node))
3. Polkadot account ([public/private keypair](https://wiki.polkadot.network/docs/en/learn-keys))
4. Some KSM/DOT to provide as collateral and pay for transaction fees

Head over to ["Installation"](/vault/installation) for a detailed setup guide.

## Fee Model

Vaults earn fees on issue and redeem, based on the BTC volume.

### Pool-based Fee Distribution

Vaults earn fees based on the issued and redeemed BTC volume. To reduce variance of payouts, the bridge implements a **pooled fee model**.

Each time a user issues or redeems interBTC, they pay the following fees to a **global fee pool**:

- **Issue Fee**: `0.5%` of the Issue volume, paid in *interBTC*
- **Redeem**: `0.5%` of the redeem volume, paid in *interBTC*

From this fee pool, `100%` is distributed among all active Vaults based on the following factor:

- 100% based on the Vault's **BTC in custody** ( = issued interBTC) in proportion to the total locked BTC (= issued interBTC) across all Vaults

Specifically, each Vault's fee is calculated according to the following formula:

    vault_fee =
    pool * (vault_locked_btc / total_locked_btc)

The Vault fee is paid each time an Issue or Redeem request is executed.

### Vault Block Rewards

Vaults receive governance tokens as fees for keeping BTC locked and providing the required insurance collateral in whitelisted assets. Early Vaults receive more rewards as they take up higher risk in terms of protocol maturity.

#### Kintsugi

For the full details of the Vault rewards on the Kintsugi canary network, see the [Kintsugi token economy paper](https://raw.githubusercontent.com/interlay/whitepapers/master/Kintsugi_Token_Economy.pdf) published by Kintsugi Labs.

#### Interlay

Vaults rewards on the main Interlay network are tbd.

## Collateral

To ensure Vaults have no incentive to steal user's BTC, Vaults provide collateral in whitelisted assets - following a similar process as [MakerDAO](https://docs.makerdao.com/smart-contract-modules/collateral-module). To mitigate exchange rate fluctuations, interBTC employs *over-collateralization* and a *multi-level collateral balancing* scheme.

### Minimum

Each currency & network has different minimum deposits, noted here:

<!-- tabs:start -->

#### **Testnet**

Testnet KSM: 0

#### **Kintsugi (Canarynet)**

KSM: 3

#### **Interlay (Mainnet)**

DOT: tbd

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

This means:

- Liquidations only affect a specific `VaultId`.
- Vault operators must take pro-active measures to re-balance between different collateral assets

When users requests to mint interBTC, they selects a specific `VaultId` to lock BTC with. Typically, users will not care with which Vault they want to mint with (unless there is a competitive fee market in the future) and will accept the automatic selection offered by the UI.

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
This can be achieved in 2 ways:

- **interBTC Redeem**: if users redeem with the Vault, the collateralization ratio increases. The Vault can also maintain a interBTC reserve and execute self-redeems for quick rebalancing
- **Increase Collateral**: alternatively, the Vault can also add more collateral to the system.

## Collateral Thresholds

The interBTC bridge introduces multiple thresholds with different actions to ensure Vaults never drop below 100% collateralization:

We will release a post detailing how the calculation of this thresholds is achieved considering the liquidity and risk profile for each collateral asset.

### Secure Collateral

#### Actions

None necessary. The Vault can freely redeem any "unused" collateral above the secure threshold.

#### Thresholds

<!-- tabs:start -->

#### **Testnet**

Testnet KSM: `150%`

#### **Kintsugi (Canarynet)**

KSM: `260%`

#### **Interlay (Mainnet)**

DOT: tbd

<!-- tabs:end -->

### Premium Redeem

#### Actions

Users can execute redeem with this Vault and receive a premium of `5%` in DOT in addition to the redeemed BTC.

#### Thresholds

<!-- tabs:start -->

#### **Testnet**

Testnet KSM: `135%`

#### **Kintsugi (Canarynet)**

KSM: `200%`

#### **Interlay (Mainnet)**

DOT: tbd

<!-- tabs:end -->

### Vault Liquidation

#### Action

The undercollateralized Vault is liquidated.

1. The Vaults entire collateral is slashed
2. The interBTC bridge initiates a first-come-first-served liquidation swap: any user can **burn interBTC** in return for collateral at a premium rate. See **[Burn Event](/overview?id=burn-event-restoring-a-11-physical-peg)** below.

#### Thresholds

<!-- tabs:start -->

#### **Testnet**

Testnet KSM: `110%`

#### **Kintsugi (Canarynet)**

KSM: `150%`

#### **Interlay (Mainnet)**

DOT: tbd

<!-- tabs:end -->

## Liquidations

If Vaults fail to behave according to protocol rules, they face punishment through liquidation of collateral.
There are 2 types of failures: **safety failures** and **crash failures**.

If a Vault fails to execute a redeem on time, steals BTC or falls below the liquidation collateral threshold, a liquidation event is initiated.

### Safety Failures

A safety failure occurs in two cases:

- **Theft**: a Vault is considered to have committed theft if it moves/spends BTC from unauthorized by the interBTC bridge. Theft is detected and reported by [Vaults](/vault/overview) via an SPV proof.

- **Severe Undercollateralization**: a Vaults drops below the liquidation collateral threshold (e.g., `110%` on testnet).

In both cases, the **the Vault's entire collateral is liquidated - up to the secure collateral threshold (e.g., `150%` on testnet) of the liquidated BTC value - and BTC holdings are considered lost**.

Consequently, the interBTC bridge initiates a **[Burn Event](/vault/overview?id=burn-event-restoring-a-11-physical-peg)** to restore the 1:1 balance between BTC and interBTC.

#### Severe Undercollateralization

Sever undercollateralization might occur when either the collateral asset (e.g. KSM) and the wrapped asset (e.g. BTC) exchange rates as reported by the oracle are changing. A Vault is liquidated when either (1) the the collateral (e.g., KSM) exchange rate drops significantly in relation to the wrapped asset (e.g., BTC) or (2) the wrapped asset (e.g., BTC) exchange rate rises significantly in relation to the collateral (e.g., KSM). 

(1) and (2) are ultimately the same thing, but possibly it helps to think about it in KSM/USD and BTC/USD rates, which means that if either (1) or (2) or both (1) and (2) happen at the same time, a Vault is liquidated.

**Example**

*Liquidation Price*

Let's say a Vault has currently 1000 KSM collateral and 1 BTC locked (the wrapped asset). Let's also assume that this represents a collateralization of 200%. And let's use 1000 KSM = $100,000 (i.e., $100/KSM) and 1 BTC = $50,000 for simplicity. Last, let's say the liquidation threshold is at 150% collateralization, so when a Vault is below this threshold, the Vault will be automatically liquidated.

The liquidation price is reached when:

- Case (1) from above: 1000 KSM = $75,000 -> $75k of collateral are backing $50k of BTC -> 150% collateralization -> liquidation price for KSM is at $75/KSM assuming only the KSM price moves
- Case (2) from above: 1 BTC = $66,666 -> $100k of collateral are backing $66k of BTC -> 150% collateralization -> liquidation price for BTC is at $66,666/BTC assuming only the BTC price moves.

Note that if (1) and (2) happen at the same time, i.e., BTC price rises and KSM price drops, liquidations might happen earlier so the liquidation price updates everytimee the price in the exchange rate oracle changes.

*Value at risk*

The value at risk is the current collateral in the Vault, i.e., the value of KSM in the vault. So if the KSM price drops in case (1), value at risk would go from $100k to $75k whereas in case (2) value at risk stays at $100k since only BTC price moves.

### Crash Failures (Failed Redeem)

If Vaults go offline and fail to execute redeem, they are:

- **Penalized** (**punishment fee** slashed) and
- **Temporarily banned** for e.g. `24 hours` from accepting further redeem requests.

The **punishment fee** is calculated as a percentage (e.g. `10%`) of the redeem amount at the current exchange rate.

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
