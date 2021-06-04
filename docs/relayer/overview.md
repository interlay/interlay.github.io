# Staked Relayers

Staked Relayers (or "Relayers") monitor both Bitcoin and the interBTC bridge, and ensure that BTC-Relay (the Bitcoin light/SPV client deployed on Polkadot) stays up to date with the Bitcoin mainchain by relaying Bitcoin block headers.

Note: BTC-Relay is self healing and automatically detects and recovers from Bitcoin forks.

### What do Relayers do?

1. **Maintain BTC-Relay**: submit Bitcoin block headers to interBTC's BTC-Relay and make sure the bridge stays up to date with the Bitcoin mainchain.
2. **Report Vault Theft**: monitor Vault Bitcoin addresses and BTC holdings and report theft to the interBTC bridge (providing an SPV proof to BTC-Relay)

### Why would I want to become a Relayer?

- **Yield farming:** Relayers earn fees in interBTC and receive a subsidy in DOT. As such, Relayers earn yield on their staked DOT and have **exposure to both DOT and (Polka)BTC**.

### What do I need to become a Relayer?

1. Relayer client ([source](https://github.com/interlay/polkabtc-clients))
2. Bitcoin full node ([instructions](https://bitcoin.org/en/full-node))
2. Polkadot account ([public/private keypair](https://wiki.polkadot.network/docs/en/learn-keys))
3. Some DOTs to provide as collateral and pay for transaction fees

Head over to ["Running a Relayer"](/relayer/guide) for a detailed setup guide.

## Fee Model

Relayers receive a share of the bridge fees, earned from the interBTC issue and redeem volume.

In addition, in the first year (and subject to extension), Relayers will receive a subsidy from the Polkadot Treasury for correctly operating the interBTC bridge (Note: this is still subject to final confirmation!).

### Pool-based Fee Distribution
Relayers earn fees based on the issued and redeemed interBTC volume. To reduce variance of payouts, the interBTC bride implements a **pooled fee model**.
From this fee pool, `3%` is distributed among all active Relayers based on their SLA (Service Level Agreement) and locked DOT stake.

Specifically, each Relayer's fee is calculated according to the following formula:

    relayer_score = relayer_sla * relayer_dot_stake

    relayer_fee =
    (pool * relayer_score ) / total_relayer_score

where ``total_relayer_score`` is the sum of the score of all active relayers.

The Relayer fee is allocated each time an Issue or Redeem request is executed.

### Subsidy

Initially, the Issue and Redeem fees will likely not suffice to offer a competitive APY compared to most other DeFi protocols.
To this end, Relayers will receive a subsidy from the Polkadot Treasury.

The exact size of the subsidy is to be determined by the Polkadot Council when parachains launch.

## Service Level Agreements

Relayers provide DOT stake and have clearly defined tasks they must execute - and face punishment in case of misbehavior.
Since any user can run a Relayer, Service Level Agreements are used to reward highly active and reliable Relayers a higher fraction of the bridge fees.

### SLA Value

The SLA value is a number between `0` and `100`.
When a Relayer joins the interBTC bridge, it starts with an SLA of `0`.

### SLA Actions

When Relayers execute desirable actions, their SLA increases - and decreases in case of deviation from the protocol rules.

#### Increasing the SLA

- **Submit Bitcoin Valid Block Header**: Submit a correct Bitcoin block header to BTC-Relay. The block header must be accepted by BTC-Relay as a main-chain or fork-header.
    - *Value*: `+1`

- **Correct Theft Report**: Correctly report a Vault theft event by providing the necessary SPV proof to BTC-Relay. Validation of the report is performed on-chain by the BTC-Relay component.
    - *Value*: `+1`
