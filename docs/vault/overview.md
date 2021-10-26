# Vaults

Vaults are the heart of the interBTC and Kintsugi bridge. They are responsible for maintaining the physical 1:1 peg between BTC and interBTC/kBTC.
Vaults receive BTC for safekeeping from users and ensure BTC remains locked while interBTC/kBTC exists

Vaults are **non-trusted** and **collateralized** and **any user can become a Vault** by providing collateral. This means: as a user, you can freely choose any Vault you like or be your own Vault. You don’t have to trust anyone else if you want to be extra cautious.

The correct behavior of Vaults is enforced by the bridge. Specifically, Vaults must prove correct behavior to the BTC-Relay component - a Bitcoin SPV client implemented directly on top of the bridge. If a Vault tries to steal BTC, this will be automatically detected and the Vault will lose its collateral - and users will be reimbursed using this collateral (at a beneficial rate).

The secondary responsibility of a Vault is to monitor both Bitcoin and the bridge to ensure that the BTC-Relay stays up to date with the Bitcoin mainchain by relaying Bitcoin block headers. BTC-Relay is self-healing and automatically detects and recovers from Bitcoin forks.

### What do Vaults do?

1. **Provide Collateral** and upload their Bitcoin public key to the bridge. The amount of collateral provided determines how much BTC the Vault can accept for safekeeping. Collateral can initially be provided in KSM (on Kintsugi) and DOT (on interBTC).
2. **Issue**: Vaults receive BTC from users for safekeeping. This locks the Vault's collateral until BTC is redeemed again.
3. **Redeem**: Vaults monitor the interBTC/Kintsugi bridge for redeem requests. When a user requests to redeem interBTC/kBTC, Vaults release BTC to the user and prove that they behaved correctly via the BTC-Relay. Only if this proof is correct, the Vault's collateral is unlocked.

To support the integrity of the bridge, Vaults are also able to assume the role of a Relayer:

1. **Maintain BTC-Relay**: submit Bitcoin block headers to BTC-Relay and make sure the bridge stays up to date with the Bitcoin mainchain.
2. **Report Vault Theft**: monitor Vault Bitcoin addresses and BTC holdings and report theft to the bridge (providing an SPV proof to BTC-Relay)

### Why operating a Vault?

1. **Earning potential:**

    - *interBTC/kBTC*: All Vaults are part of a fee pool and earn fees in interBTC/kBTC when any user issues or redeems interBTC/kBTC.
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

Each time a user issues or redeems interBTC/kBTC, they pay the following fees to a **global fee pool**:

- **Issue Fee**: `0.5%` of the Issue volume, paid in *interBTC/kBTC*
- **Redeem**: `0.5%` of the redeem volume, paid in *interBTC/kBTC*

From this fee pool, `100%` is distributed among all active Vaults based on the following factor:

- 100% based on the Vault's **BTC in custody** ( = issued interBTC/kBTC) in proportion to the total locked BTC (= issued interBTC/kBTC) across all Vaults

Specifically, each Vault's fee is calculated according to the following formula:

    vault_fee =
    pool * (vault_locked_btc / total_locked_btc)

The Vault fee is paid each time an Issue or Redeem request is executed.

### KINT Vault Block Rewards (Kintsugi-only)

Vaults receive KINT as fees for keeping BTC locked and providing the required insurance collateral in KSM and other assets. Early Vaults receive more rewards as they take up higher risk in terms of protocol maturity.

For the full details, see the [Kintsugi whitepaper](https://raw.githubusercontent.com/interlay/whitepapers/master/Kintsugi_Token_Economy.pdf).

## Collateral

To ensure Vaults have no incentive to steal user's BTC, Vaults provide collateral in DOT to the interBTC bridge or KSM to the Kintsugi bridge.
To mitigate exchange rate fluctuations, the bridge employs *over-collateralization* and a *multi-level collateral balancing* scheme.

### Over-collateralization

Vaults must over-collateralize their BTC holdings by `150%` with DOT collateral.

This means, the amount of BTC a Vault can accept for safekeeping is calculated by:

    max_vault_btc = vault_dot_collateral / (1.5 * btc_dot_exchange_rate)

### Vault-Level Collateral Re-balancing

To protect against short and long term exchange rate fluctuations, Vaults are **instructed to keep their collateralization rate up to date**.
This can be achieved in 2 ways:

- **interBTC Redeem**: if users redeem with the Vault, the collateralization ratio increases. The Vault can also maintain a interBTC reserve and execute self-redeems for quick rebalancing
- **Increase Collateral**: alternatively, the Vault can also add more collateral to the system.

#### Thresholds and Balancing Mechanisms

The interBTC bridge introduces multiple thresholds with different actions to ensure Vaults never drop below 100% collateralization:

**Secure Collateral**:

- *Threshold*: `150%`
- *Actions*: None necessary. The Vault can freely redeem any "unused" collateral above the `150%` threshold.

**Premium Redeem**:

- *Threshold*: `135%`
- *Actions*: Users can execute redeem with this Vault and receive a premium of `5%` in DOT in addition to the redeemed BTC.

**Vault Liquidation**:

- *Threshold*: `110%`
- *Action*: The undercollateralized Vault is liquidated.
    1. The Vaults entire DOT collateral is slashed
    2. The interBTC bridge initiates a first-come-first-served liquidation swap: any user can **burn interBTC** in return for DOT collateral at a premium rate. See [**Burn Event**](/overview?id=burn-event-restoring-a-11-physical-peg) below.

## Slashing

If Vaults fail to behave according to protocol rules, they face punishment through slashing of collateral.
There are 2 types of failures: **safety failures** and **crash failures**.

If a Vault fails to execute a redeem on time, steals BTC or falls below the liquidation collateral threshold, a slashing event is initiated.

### Safety Failures

A safety failure occurs in two cases:

- **Theft**: a Vault is considered to have committed theft if it moves/spends BTC from unauthorized by the interBTC bridge. Theft is detected and reported by [Vaults](/vault/overview) via an SPV proof.

- **Severe Undercollteralization**: a Vaults drops below the `110%` liquidation collateral threshold.

In both cases, the **the Vault's entire BTC holdings are liquidated and its DOT collateral is slashed - up to `150%` (secure collateral threshold) of the liquidated BTC value**.

Consequently, the interBTC bridge initiates a [**Burn Event**](/overview?id=burn-event-restoring-a-11-physical-peg) to restore the 1:1 balance between BTC and interBTC.

### Crash Failures (Failed Redeem)

If Vaults go offline and fail to execute redeem, they are:

- **Penalized** (**punishment fee** slashed) and
- **Temporarily banned** for `24 hours` from accepting further redeem requests.

The **punishment fee** is calculated as a percentage (`10%`) of the redeem amount at the current exchange rate.

## Burn Event: Restoring a 1:1 Physical Peg

When a Vault is liquidated, its DOT collateral is slashed up to `150%` of the liquidated BTC value, given the exchange rate at the time of liquidation.

The interBTC bridge now has less BTC locked than interBTC minted - but more than enough DOT collateral to maintain economic security.
To re-establish the physical 1:1 peg between BTC and interBTC, the interBTC bridge allows users to **burn interBTC in return for DOT at a premium rate**.

Specifically, the user's payout is calculated as follows:


    burn_dot_payout =
        (total_liquidated_dot_collateral / total_liquidated_interbtc)
        * user_burned_interbtc

As long as the economic value of `burn_dot_payout` is higher than that of `user_burned_interbtc`, which may include private information of the user (that is, the user may think that DOT will become worth more soon), users are incentivized to burn interBTC in return for DOT and to re-balance the system.

This Burn Event continues until the 1:1 ratio of BTC to interBTC is restored.
