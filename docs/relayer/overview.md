# Staked Relayers

Staked Relayers (or "Relayers") monitor both Bitcoin and the PolkaBTC bridge, and ensure that BTC-Relay (the Bitcoin light/SPV client deployed on Polkadot) stays up to date with the Bitcoin mainchain by relaying Bitcoin block headers. Should a major attack be launched on Bitcoin or the PolkaBTC bridge, Staked Relayers can vote to halt the bridge until the issue is resolved. 

Note: BTC-Relay is self healing and automatically detects and recovers from Bitcoin forks.

### What do Relayers do?

1. **Maintain BTC-Relay**: submit Bitcoin block headers to PolkaBTC's BTC-Relay and make sure the bridge stays up to date with the Bitcoin mainchain.
2. **Report Invalid BTC Blocks**: report that a block submitted to BTC-Relay is invalid under Bitcoin's consensus rules (e.g., contains an invalid transaction). Note: BTC-Relay cannot by itself check if a block contains an invalid transaction (only stored block headers).
3. **Report Vault Theft**: monitor Vault Bitcoin addresses and BTC holdings and report theft to the PolkaBTC bridge (providing an SPV proof to BTC-Relay)
4. **Report Oracle Failure** : monitor PolkaBTC price oracles and report crash failures/severe delays in data submission.
5. **Vote on Bridge Status Updates**: vote on security status updates of the PolkaBTC bridge. Relayers can temporarily halt the PolkaBTC bridge through a majority vote in case of critical failures, such as Bitcoin hard forks or coordinated attacks.

### Why would I want to become a Relayer?

- **Yield farming:** Relayers earn fees in PolkaBTC and receive a subsidy in DOT. As such, Relayers earn yield on their staked DOT and have **exposure to both DOT and (Polka)BTC**.  

### What do I need to become a Relayer?

1. The open-source Relayer client
2. A Bitcoin full node
2. A Polkadot account ([public/private keypair](https://wiki.polkadot.network/docs/en/learn-keys))
3. Some DOTs to provide as collateral and pay for transaction fees.

Head over to ["Running a Relayer"](/relayer/guide) for a detailed setup guide.

## Fee Model

Relayers receive a share of the bridge fees, earned from the PolkaBTC issue and redeem volume. 

In addition, in the first year (and subject to extension), Relayers will receive a subsidy from the Polkadot Treasury for correctly operating the PolkaBTC bridge (Note: this is still subject to final confirmation!).

### Pool-based Fee Distribution
Relayers earn fees based on the issued and redeemed PolkaBTC volume. To reduce variance of payouts, the PolkaBTC bride implements a **pooled fee model**.
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

## Stake

To ensure Relayers have no incentive to submit invalid Bitcoin block headers to BTC-Relay or make incorrect status updates, Relayers provide stake in DOT to the PolkaBTC bridge.

?> The more DOT stake a Relayer provides, the higher the reward share and the heavier the weight of its vote when voting on PolkaBTC status updates.

## Slashing

If Relayer fail to behave according to protocol rules, they face punishment through slashing of DOT stake.
There are 2 types of failures: 

- **Submission of an Invalid Bitcoin Block**
- **Incorrect Vote on Invalid Bitcoin Block**

Currently, slashing of Relayers is executed via an out-of-band Governance vote and sudo call. 

?> In the future, automated stake slashing based on Relayer majority vote will be implemented. 

## Service Level Agreements

Relayers provide DOT stake and have clearly defined tasks they must execute - and face punishment in case of misbehavior.
Since any user can run a Relayer, Service Level Agreements are used to reward highly active and reliable Relayers a higher fraction of the bridge fees.

### SLA Value

The SLA value is a number between `0` and `100`.
When a Relayer joins the PolkaBTC bridge, it starts with an SLA of `0`.

### SLA Actions

When Relayers execute desirable actions, their SLA increases - and decreases in case of deviation from the protocol rules.


#### Increasing the SLA

- **Submit Bitcoin Valid Block Header**: Submit a correct Bitcoin block header to BTC-Relay. The block header must be accepted by BTC-Relay as a main-chain or fork-header. 
    - *Value*: `+1`


<!--- - **Submit Invalid Bitcoin Block Header**: Submit an invalid Bitcoin block header to BTC-Relay. Invalid blocks are reported and voted upon by other Relayers. 
    - *Value*: `-100`
--->

- **Correct NoData Report / Vote**: Correctly report or vote on a NoData error. "Correct" in this case means that the majority of Relayers (based on stake) voted accordingly.
    - *Value*: `+1`

- **Correct Invalid Report / Vote**: Correctly report or vote on a Invalid error, i.e., an invalid Bitcoin block header was submitted to the BTC-Relay. "Correct" in this case means that the majority of Relayers (based on stake) voted accordingly.
    - *Value*: `+10`


- **Correct Theft Report**: Correctly report a Vault theft event by providing the necessary SPV proof to BTC-Relay. Validation of the report is performed on-chain by the BTC-Relay component. 
    - *Value*: `+1`

- **Correct Oracle Offline Report**: Correctly report a than an Oracle is offline. Validation of the report is performed on-chain by the PolkaBTC bridge oracle module. 
    - *Value*: `+1`

#### Decreasing the SLA

- **False NoData Report / Vote**: Correctly report or vote on a NoData error. "Correct" in this case means that the majority of Relayers (based on stake) voted accordingly.
    - *Value*: `-10`

- **False Invalid Report / Vote**: Correctly report or vote on a Invalid error, i.e., an invalid Bitcoin block header was submitted to the BTC-Relay. "Correct" in this case means that the majority of Relayers (based on stake) voted accordingly.
    - *Value*: resets Relayer SLA to `0`

- **Ignored Vote**: Fail to cast a vote during a PolkaBTC bridge status vote.
    - *Value*: `-10`
