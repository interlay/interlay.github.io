# Vaults

Vaults are the heart of the PolkaBTC bridge. They are responsible for maintaining the physical 1:1 peg between BTC and PolkaBTC.
Vaults receive BTC for safekeeping from users and ensure BTC remains locked while PolkaBTC exists

Vaults are **non-trusted** and **collateralized** and **any user can become a Vault** by providing DOT collateral. This means: as a user, you can freely choose any Vault you like or be your own Vault. You don’t have to trust anyone else if you want to be extra cautious.

The correct behavior of Vaults is enforced by the PolkaBTC bridge parachain. Specifically, Vaults must prove correct behavior to the BTC-Relay component - a Bitcoin SPV client implemented directly on top of Polkadot. If a Vault tries to steal BTC, this will be automatically detected and the Vault will lose its collateral - and users will be reimbursed using this collateral (at a beneficial rate).


### What do Vaults do?

1. **Provide DOT Collateral** and upload their Bitcoin public key to the PolkaBTC bridge. The amount of collateral provided determines how much BTC the Vault can accept for safekeeping / how many PolkaBTC this Vault can secure.
2. **Issue**: Vaults receive BTC from users for safekeeping. This locks the Vault's DOT collateral until BTC is redeemed again.
3. **Redeem**: Vaults monitor the PolkaBTC bridge for redeem requests. When a user requests to redeem PolkaBTC, Vaults release BTC to the user and prove that they behaved correctly to the PolkaBTC bridge (via the BTC-Relay). Only if this proof is correct, the Vault's collateral is unlocked again.

### Why would I want to become a Vault?

1. **Yield farming:** Vaults earn fees in PolkaBTC and receive a subsidy in DOT. As such, Vaults earn yield on their DOT collateral and have **exposure to both DOT and (Polka)BTC**.  
2. **Self-custody:** Vaults hold BTC of users in custody. If you are a large liquidity provider, you can be your own vault and retain custody over your BTC holdings until you sell PolkaBTC.

### What do I need to become a Vault?

1. The open-source Vault client
2. A Bitcoin account ([public/private keypair](https://en.bitcoin.it/wiki/Private_key))
3. A Polkadot account ([public/private keypair](https://wiki.polkadot.network/docs/en/learn-keys))
4. Some DOTs to provide as collateral and pay for transaction fees.

Head over to ["Running a Vault"](/vault/guide) for a detailed setup guide.

## Fee Model

Vaults earn fees on issue and redeem, based on the PolkaBTC volume.

In addition, in the first year (and subject to extension) , Vaults will receive a subsidy from the Polkadot Treasury for correctly operating the PolkaBTC bridge and providing DOT collateral (Note: this is still subject to final confirmation!).
### Pool-based Fee Distribution
Vaults earn fees based on the issued and redeemed PolkaBTC volume. To reduce variance of payouts, the PolkaBTC bride implements a **pooled fee model**.

Each time a user issues or redeems PolkaBTC, they pay the following fees to a **global fee pool**:

- **Issue Fee**: `0.5%` of the Issue volume, paid in *PolkaBTC*
- **Redeem**: `0.5%` of the redeem volume, paid in *PolkaBTC*

From this fee pool, `77%` is distributed among all active Vaults based on the following two factors:

- 90% based on the Vault's **BTC in custody** ( = issued PolkaBTC) in proportion to the total locked BTC ( = issued PolkaBtc) across all Vaults
- 10% based on the Vault's **locked DOT collateral** in proportion to the total locked DOT collateral across all Vaults

Specifically, each Vault's fee is calculated according to the following formula:

    vault_fee =
    pool * 0.9 (vault_locked_btc / total_locked_btc)
    + pool * 0.1 * (vault_locked_dot / total_locked_dot)_

The Vault fee is paid each time an Issue or Redeem request is executed.

### Collateral Subsidy

Initially, the Issue and Redeem fees will likely not suffice to offer a competitive APY compared to most other DeFi protocols, including the staking rewards on the Polkadot Relay Chain.
To this end, Vault will receive a subsidy from the Polkadot Treasury.

The exact size of the subsidy is to be determined by the Polkadot Council when parachains launch.
The aim is to offer Vaults an APY similar to that of the Relay Chain staking rewards.


## Collateral

To ensure Vaults have no incentive to steal user's BTC, Vaults provide collateral in DOT to the PolkaBTC bridge.
To mitigate exchange rate fluctuations, the PolkaBTC bridge employs *over-collateralization* and a *multi-level collateral balancing* scheme.
### Over-collateralization

Vaults must over-collateralize their BTC holdings by `150%` with DOT collateral.

This means, the amount of BTC a Vault can accept for safekeeping is calculated by:

    max_vault_btc = vault_dot_collateral / (1.5 * btc_dot_exchange_rate)

### Vault-Level Collateral Re-balancing

To protect against short and long term exchange rate fluctuations, Vaults are **instructed to keep their collateralization rate up to date**.
This can be achieved in 2 ways:

- **PolkaBTC Redeem**: if users redeem with the Vault, the collateralization ration increases. The Vault can also maintain a PolkaBTC reserve and execute self-redeems for quick rebalancing
- **Increase Collateral**: alternatively, the Vault can also add more collateral to the system.

#### Thresholds and Balancing Mechanisms
The PolkaBTC bridge introduces multiple thresholds with different actions to ensure Vaults never drop below 100% collateralization:

**Secure Collateral**:

- *Threshold*: `150%`
- *Actions*: None necessary. The Vault can freely redeem any "unused" collateral above the `150%` threshold.

**Premium Redeem**:

- *Threshold*: `135%`
- *Actions*: Users can execute redeem with this Vault and receive a premium of `5%` in DOT in addition to the redeemed BTC.

**Vault Auction**:

- *Threshold*: `120%`
- *Actions*: Other Vaults can forcefully replace this Vault ([Replace protocol](https://interlay.gitlab.io/polkabtc-spec/spec/replace.html)), adding DOT collateral and taking over the BTC holdings of the undercollateralized Vault - earning a `5%` premium fee on the replaced BTC volume.

**Vault Liquidation**:

- *Threshold*: `110%`
- *Action*: The undercollateralized Vault is liquidated.
    1. The Vaults entire DOT collateral is slashed
    2. The PolkaBTC bridge initiates a first-come-first-served liquidation swap: any user can **burn PolkaBTC** in return for DOT collateral at a premium rate. See [**Burn Event**](/overview?id=burn-event-restoring-a-11-physical-peg) below.



## Slashing

If Vaults fail to behave according to protocol rules, they face punishment through slashing of collateral.
There are 2 types of failures: **safety failures** and **crash failures**.

If a Vault fails to execute a redeem on time, steals BTC or falls below the liquidation collateral threshold, a slashing event is initiated.

### Safety Failures

A safety failure occurs in two cases:
- **Theft**: a Vault is considered to have committed theft if it moves/spends BTC from unauthorized by the PolkaBTC bridge. Theft is detected and reported by [Relayers](/relayer/overview) via an SPV proof. 

- **Severe Undercollteralization**: a Vaults drops below the `110%` liquidation collateral threshold.

In both cases, the **the Vault's entire BTC holdings are liquidated and its DOT collateral is slashed - up to `150%` (secure collateral threshold) of the liquidated BTC value**. 

Consequently, the PolkaBTC bridge initiates a [**Burn Event**](/overview?id=burn-event-restoring-a-11-physical-peg) to restore the 1:1 balance between BTC and PolkaBTC.

### Crash Failures (Failed Redeem)

If Vaults go offline and fail to execute redeem, they are:

- **Penalized** (**punishment fee** slashed) and
- **Temporarily banned** for `24 hours` from accepting further redeem requests.

The **punishment fee** is calculated based on the Vault's SLA ([Service Level Agreement](/vault/overview?id=service-level-agreements)) level, which is a value between `0` and `100`.
The higher the Vault's SLA, the lower the punishment for a failed redeem.

In detail, the punishment fee is calculated as follows:
- **Minimum Punishment Fee**: `10%` of the failed redeem value.
- **Maximum Punishment Fee**: `30%` of the failed redeem value.
- **Punishment Fee**: calculated based on the Vaults SLA value as follows. Note: the maximum SLA (`max_sla`) value is `100` (see [here](/vault/overview?id=service-level-agreements)).


    punishment_fee =
        min_punishment_fee +
        ((max_punishment_fee - min_punishment_fee) / max_sla * vault_sla)


**Note**: the SLA of a Vault is **reset to 0 after a single failed redeem request** and the Vault must behave correctly / be online for prolonged periods to build up a high SLA level again.

## Service Level Agreements

Vaults provide collateral to secure BTC held in custody and have clearly defined tasks they must execute - and face punishment in case of misbehavior.
However, slashing collateral for each minor protocol deviation would result in too high risk profiles for Vaults, yielding these roles unattractive to users.

To reduce the risk for Vaults, especially to protect Vaults against network/latency issues, the PolkaBTC bridge makes use of Service Level Agreements.
By being online and behaving correctly, Vaults increase their SLA value, one correct action at a time. Higher SLAs result in higher rewards and preferred treatment where applicable in the Issue and Redeem protocols. As mentioned above, SLAs are also used to reduce punishment fees for one-time failures of otherwise honest / reliable Vaults.

### SLA Value

The SLA value is a number between `0` and `100`.
When a Vaults joins the PolkaBTC bridge, it starts with an SLA of `0`.

### SLA Actions

When Vaults execute desirable actions, their SLA increases - and decreases in case of deviation from the protocol rules.


#### Increasing the SLA

- **Execute Issue**: accept an issue request, receiving BTC and locking DOT collateral.
    - *Value*: The SLA increase is calculated based on the issue request volume compared to the average volume of the last `N` issue requests. The maximum increase is thereby given by `max_sla_increase = 4`.


    sla_increase_issue =
        max(
            issue_request_size / average_issue_request_size_last_N * max_sla_increase,
            max_sla_increase
            )

- **Submit Issue Proof**: the Vault submits the SPV proof for the issue request Bitcoin payment on behalf of the user.
    - *Value*: `+1`


#### Decreasing the SLA

- **Failed Redeem**: Vault fails to execute redeem on time.
    - *Value*: resets the SLA to `0`
- **Theft**: Vault steals. Note: in this case, the Vault is also banned from the PolkaBTC bridge.
    - *Value*: resets the SLA to `0`


## Burn Event: Restoring a 1:1 Physical Peg

When a Vault is liquidated, its DOT collateral is slashed up to`150%` of the liquidated BTC value, given the exchange rate at the time of liquidation.

The PolkaBTC bridge now has less BTC locked than PolkaBTC minted - but more than enough DOT collateral to maintain economic security.
To re-establish the physical 1:1 peg between BTC and PolkaBTC, the PolkaBTC bridge allows users to **burn PolkaBTC in return for DOT at a premium rate**.


Specifically, the user's payout is calculated as follows:


    burn_dot_payout =
        (total_liquidated_dot_collateral / total_liquidated_polkabtc)
        * user_burned_polkabtc

As long as the economic value of `burn_dot_payout` is higher than that of `user_burned_polkabtc`, which may include private information of the user (that is, the user may think that DOT will become worth more soon), users are incentivized to burn PolkaBTC in return for DOT and to re-balance the system.

This Burn Event continues until the 1:1 ration of BTC to PolkaBTC is restored.