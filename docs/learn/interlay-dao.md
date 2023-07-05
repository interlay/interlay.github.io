# Governance: Liquid, Optimistic, Stake-to-Vote

The Interlay network is governed by the DAO of INTR holders. Governance is liquid, optimistic and stake-to-vote.

## Voting Rights

### Who can vote?

Any INTR holder can participate in governance. To do so, you need to lock INTR.

?> Both **vested** and **unvested** INTR can be used for voting (and stake-to-vote).

### What can be voted upon?

Community governance can propose and vote on essentially any possible modification to the system.
Specifically:

- **Runtime upgrades** (code changes for bugfixes, substrate/library updates, new features, ...)
- **Parameter upgrades** (collateral whitelisting, liquidation thresholds, accepted oracles, ...)
- **Treasury spending**

?> While many DeFi protocols bet on "gradual decentralization", Interlay focuses **on decentralization from day 1**. This entails a higher risk during the early days of the network, as the community learns to organize - but this risk ultimately pays off in the mid and long run.

## Liquid & Optimistic Governance

To promote a more active governance process and avoid the “lazy voter” problem, Interlay implements “optimistic governance”. This means:

- **No Council**, only public proposals from community
- Community can elect a **Technical Committee** to fast-track proposals
- Referenda are require a **Super-Majority** vote

## Stake-to-Vote

To vote on governance proposals, users must lock INTR with the Interlay parachain (escrow)- minting vINTR, a non-transferable token representing each user's voting power at any given point in time.

### Hard facts

- The longer INTR are locked, the more vINTR are minted.
- Lock periods:
  - **Minimum: 1 week**
  - **Maximum: 192 weeks**
- As time progresses, the vINTR balance **decreases linearly with time** (per block)
- INTR can be withdrawn at the end of the lock period (all at once)
- You can extend the lock period at any time (up to the maximum lock period)

?> The idea is simple: **the longer INTR are locked**, the **more voting power** a voter has, since the voter has a **longer-term stake in the health and success** of Interlay.

### Staking Rewards

Users who lock INTR to mint vINTR and participate in governance receive staking rewards in INTR. You can check the up-to-date staking parameters in the [staking app](https://app.interlay.io/staking).

The staking rewards are distributed on a per-block basis, proportional to the initial vINTR voting balance on deposit. This encourages users to lockup as much as possible for as long as possible, no more rewards are earned after withdrawal.

?> Outlook: Additional criteria may be added by community governance, e.g. voting participation.

## Proposals and Referenda

All proposals are public and undergo a referenda (= voting process), as visualized below:

<img src="../_assets/img/kintsugi/governance.jpeg" alt="INTR Governance" width="400"/>

### Process

1) Any vINTR token holder can submit a public proposal. This requires a deposit which "reserves" vINTR tokens (i.e., restricts vINTR available for voting on ongoing referenda or seconding other proposals. ).
2) Other vINTR holders can [second](https://wiki.polkadot.network/docs/maintain-guides-democracy#seconding-a-proposal) that proposal, specifying how much vINTR they want to reserve.

?> Reserving vINTR has no impact on the lock duration. This is merely a way to limit the number of proposals you can make in parallel. Example: you have 300 vINTR and second a proposal with 20 vINTR. You now use 280 vINTR to second other proposals (or make new proposals yourself), while 20 are reserved until the proposal becomes a referendum.

3) Once every ``7 days`` (= `LaunchPeriod`) the proposal with the highest vINTR backing becomes a referenda (i.e., goes to vote).
4) All vINTR reserved for this proposal are released (i.e., are now available to be used for voting on the new referendum or to second other proposals).
5) vINTR holders vote on the referenda.
6) After the voting period, votes are counted .
7) If the vote passed, the proposal is executed.

## Technical Committee

Interlay also exhibits a Technical Committee (TC) of developer teams, voted on by the community governance.

The **only function of the TC is fast-tracking proposals**: a fast-tracked instantly goes to vote, even if other proposals have more vINTR backing. This fast-tracking mechanism is used to ensure the system can be upgraded quickly in case of critical bugfixes.

!> The TC has **no veto power**! The TC can only fast-track proposals - but **can never interfere with governance votes**.

At launch, the Interlay Labs team, as initial code contributors, will represent the Technical Committee to ensure bug fixes can be fast-tracked if necessary. The community can elect another TC or increase the number of TC seats anytime through a proposal.

## Voting Power Distribution

Interlay launched 100% decentralized from day 1:

- No admin keys,
- No single entity / group has more than 50% voting power,
- Any INTR holder can participate in governance.

?> Recommended reading for understanding the governance and voting power dynamics: [INTR token economy whitepaper](https://raw.githubusercontent.com/interlay/whitepapers/master/Interlay_Token_Economy.pdf).

### Restricting Team and Investor Voting Power

Since both vested and unvested tokens can participate in voting, the Interlay team and investors introduce additional restrictions on their own voting power during the first 4 years after network launch. Specifically: while the community can use 100% of their vested and unvested INTR to vote, team and investors can only use a portion of their INTR (which is subject to lockup and vesting) to participate in governance - increasing linearly over 4 years.
