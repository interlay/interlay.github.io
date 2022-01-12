# Interlay Bug Bounty Program

<a class="docs-button util-w100" href="mailto:security@interlay.io">
  Submit Bug Report
</a>

## Help us secure Interlay and Kintsugi

We are opening the Interlay Bug Bounty (the ”Program”) to incentivize responsible bug disclosure.

We are limiting the scope of the Program to critical, high, and medium severity bugs in the parachain, parchain clients and the parachain typescript library (see Bounty Scope below).We are offering a reward of up to $50,000. Happy hunting!

## Bounty Scope

The bug bounty will be applicable for the following repositories, sources and sites:

- Parachain - https://github.com/interlay/interbtc
- Parachain clients (excluding the `faucet` crate) - https://github.com/interlay/interbtc-clients
- Parachain typescript library - https://github.com/interlay/interbtc-api

## Vulnerabilities Classification

**Critical** An issue that might cause immediate loss of > 10% of the funds, or permanent impairment of the protocol state.

**High** An issue that might cause immediate loss of <10% of the funds, or severely damage the protocol state.

**Medium** An issue that might theoretically cause minimal loss of funds (<10%), damage the protocol state, or cause severe user dissatisfaction.

**Low / Note** An issue that might cause user dissatisfaction or minimal failure.

## Exclusions

The following are not within the scope of the Program:

- Bugs in third party software
- Already reported bugs
- Already discovered bugs
- Denial of service
- Spamming
- Social engineering (including phishing) of team members or contractors
- Any physical attempts against Interlay property or data centers

## Rewards

The size of the bounty will vary depending on the severity of the issue discovered.

Decisions on the eligibility and size of a reward are guided by the rules above, but are, in the end, determined at the sole discretion of Interlay.

- **Critical:** up to $50,000
- **High:** up to $25,000
- **Medium:** up to $10,000

## Disclosure

Any vulnerability or bug discovered must be reported only to the following email: security@interlay.io.

Please also encrypt your bug report using our [pgp key](https://interlay.io/pgp-key.txt).

The vulnerability must not be disclosed publicly or to any other person, entity or email address before Interlay has been notified, has fixed the issue, and has granted permission for public disclosure. In addition, disclosure must be made within 24 hours following discovery of the vulnerability.

In addition to severity, other variables are also considered when Interlay evaluates the eligibility and size of a bounty, including (but not limited to):

- **Quality of description.** Higher rewards are paid for clear, well-written submissions.
- **Quality of reproducibility.** Please include test code, scripts and detailed instructions. The easier it is for us to reproduce and verify the vulnerability, the higher the reward.
- **Quality of fix, if included.** Higher rewards are paid for submissions with clear description of how to fix the issue.

Anyone who reports a unique, previously-unreported vulnerability that results in a change to the code or a configuration change and who keeps such vulnerability confidential until it has been resolved by our engineers will be recognized publicly for their contribution if they so choose.
