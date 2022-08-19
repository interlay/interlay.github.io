# Governance: Liquid, Optimistic, Stake-to-Vote

Kintsugi's governance model is inspired by [Polkadot’s governance structure](https://wiki.polkadot.network/docs/learn-governance), yet introduces two major modifications.

1. Liquid, optimistic governance
2. Stake-to-vote.

## Voting Rights

### Who can vote?

Any KINT holder can participate in governance. To do so, you need to lock KINT - see [stake-to-vote](kintsugi/governance?id=stake-to-vote) below.

Both **vested** and **unvested** KINT can be used for voting (and stake-to-vote)!

### What can be voted upon?

Community governance can propose and vote on essentially any possible modification to the system.
Specifically:

- **Runtime upgrades** (code changes for bugfixes, substrate/library updates, new features, ...)
- **Parameter upgrades** (collateral whitelisting, liquidation thresholds, accepted oracles, ...)
- **Treasury spending**

?> While many DeFi protocols bet on "gradual decentralization", Interlay focuses **on decentralization from day 1**. This entails a higher risk during the early days of the network, as the community learns to organize - but this risk ultimately pays off in the mid and long run. Kintsugi hence serves as an experiment, betting on the strength of the community.

## Liquid & Optimistic Governance

To promote a more active governance process and avoid the “lazy voter” problem, Kintsugi implements “optimistic governance”. This means:

- **No Council**, only public proposals from community
- Community can elect a **Technical Committee** to fast-track proposals
- Referenda are **Super-Majority Against (Negative Turnout Bias)** by default

### Super-Majority Against (Negative Turnout Bias)

An important distinction is the negative turnout bias (Super-Majority Against​) voting threshold. This is best summarized in the [Polkadot wiki](https://wiki.polkadot.network/docs/learn-governance):

> A heavy super-majority of nay votes is required to reject at low turnouts, but as turnout increases towards 100%, it becomes a simple majority-carries vote.

The outcome of a vote is determined by the following formula:

![Negative turnout bias formula](../_assets/img/kintsugi/negative-turnout-bias.png)

where

- `against` are the votes against a proposal ("nay")
- `approve` are the votes in favor of a proposal ("aye")
- `electorate` is the total voting power of the network
- `turnout` is the vote turnout (`against + approve`)

Below, we visualize the percentage of `against` (in relation to the `electorate`) votes necessary to fail a proposal:

![Plot: Minimum number of AGAINST votes to fail a vote, for given APPROVE votes](../_assets/img/kintsugi/nay-votes-to-fail-vote.png)

As we can see, at low turnouts, the number of `against` votes must be significantly higher than the `approve` votes. As we get closer to 100% turnout we get closer to 50% `against` versus 50% `approve`.

?> The idea behind this is that proposals which the community do not deem impactful (e.g. minor parameter changes, small treasury proposals, etc.) will tend to pass, unless strongly opposed. If a disputed vote starts gaining higher turnouts, we move towards a traditional simple majority vote.

## Stake-to-Vote

To vote on governance proposals, users must lock KINT with the Kintsugi parachain (escrow)- minting vKINT, a non-transferable token representing each user's voting power at any given point in time.

### Hard facts

- The longer KINT are locked, the more vKINT are minted.
- Lock periods:
  - **Minimum: 1 week**
  - **Maximum: 96 weeks** (2x the maximum parachain lease period on Kusama).
- As time progresses, the vKINT balance **decreases linearly with time** (per block)
- KINT can be withdrawn at the end of the lock period (all at once)
- You can extend the lock period at any time (up to the maximum lock period)

?> For performance reasons, the end dates of locks are rounded down - i.e. KINT are locked in weekly intervals.

?> The idea is simple: **the longer KINT are locked**, the **more voting power** a voter has, since the voter has a **longer-term stake in the health and success** of Kintsugi.

### Staking Rewards

Users who lock KINT to mint vKINT and participate in governance receive staking rewards in KINT:

- Years 1-4: **5% of the initial 4 year supply of KINT**
- Year 5+: **6.7% of the annual inflation**

The staking rewards are distributed on a per-block basis, proportional to the initial vKINT voting balance on deposit (using interBTC's [scalable reward distribution mechanism](https://spec.interlay.io/economics/fees.html#excursion-scalable-reward-distribution)). This encourages users to lockup as much as possible for as long as possible, no more rewards are earned after withdrawal.

?> Outlook: In the mid-term, additional criteria may be added to receiving staking rewards, if so decided by Governance. For example, accounts with very active vote participation may be rewarded higher rewards.

## Proposals and Referenda

The process of proposals and voting is very similar to [Polkadot’s governance structure](https://wiki.polkadot.network/docs/learn-governance) - with the difference that all proposals are public and undergo a referenda (= voting process), as visualized below:

<img src="../_assets/img/kintsugi/governance.jpeg" alt="KINT Governance" width="400"/>

### Process

1) Any vKINT token holder can submit a public proposal. This requires a deposit which "reserves" vKINT tokens (i.e., restricts vKINT available for voting on ongoing referenda or seconding other proposals. ).
2) Other vKINT holders can [second](https://wiki.polkadot.network/docs/maintain-guides-democracy#seconding-a-proposal) that proposal, specifying how much vKINT they want to reserve.

?> Reserving vKINT has no impact on the lock duration. This is merely a way to limit the number of proposals you can make in parallel. Example: you have 10 vKINT and second a proposal with 2 vKINT. You now use 8 vKINT to second other proposals (or make new proposals yourself), while 2 are reserved until the proposal becomes a referendum.

3) Once every ``7 days`` (= `LaunchPeriod`) the proposal with the highest vKINT backing becomes a referenda (i.e., goes to vote).
4) All vKINT reserved for this proposal are released (i.e., are now available to be used for voting on the new referendum or to second other proposals).
5) vKINT holders vote on the referenda.
6) After the voting period of ``7 days``, votes are counted (see [optimistic governance](kintsugi/governance?id=optimistic-governance) above)
7) If the vote passed, the proposal is executed.

## Technical Committee

Kintsugi also exhibits a Technical Committee (TC) of developer teams, voted on by the community governance.

The **only function of the TC is fast-tracking proposals**: a fast-tracked instantly goes to vote, even if other proposals have more vKINT backing. This fast-tracking mechanism is used to ensure the system can be upgraded quickly in case of critical bugfixes.

!> The TC has **no veto power**! The TC can only fast-track proposals - but **can never interfere with governance votes**.

At launch, the Interlay team, as main code contributors, will represent the Technical Committee to ensure bug fixes can be fast-tracked if necessary. The community can elect another TC or increase the number of TC seats anytime through a proposal.

## Voting Power Distribution

Kintsugi will launch 100% decentralized from day 1:

- No admin keys,
- No single entity / group has more than 50% voting power,
- Any KINT holder can participate in governance.

?> Recommended reading for understanding the governance and voting power dynamics: [KINT token economy whitepaper released by Kintsugi Labs](https://raw.githubusercontent.com/interlay/whitepapers/master/Kintsugi_Token_Economy.pdf).

### Restricting Team and Investor Voting Power
Since both vested and unvested tokens can participate in voting, the Interlay team and investors introduce additional restrictions on their own voting power during the first 2 years after network launch. Specifically: while the community can use 100% of their vested and unvested KINT to vote, team and investors can only use a portion of their KINT (which is subject to lockup and vesting) to participate in governance - increasing linearly over 2 years.

### Estimates: First 2 Years
The estimated voting power distribution of Kintsugi over the first 2 years is hence as follows*:

![Kintsugi voting power distribution over first 2 years](../_assets/img/kintsugi/voting-power.png)

\* This excludes the 10% Foundation Reserve and the remaining 21% of the On-chain Treasury which are excluded from voting. The LP rewards are estimated - and may be higher during the first two years.
