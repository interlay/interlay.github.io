# Overview

In 2016, the [Polkadot whitepaper](https://polkadot.network/PolkaDotPaper.pdf) identified secure interoperability with Bitcoin as a critical and also challenging feature. In January 2020, with the launch of Polkadot on the horizon, the [Web3 Foundation](https://web3.foundation/in) commissioned [Interlay](https://www.interlay.io/) to design a trustless bridge from Bitcoin to Polkadot based on [XCLAIM](https://www.xclaim.io/) — a carefully designed, open and trustless system that guarantees the security of user’s funds.

The BTC-Parachain allows users to mint 1:1 Bitcoin-backed assets onto Polkadot - interBTC - and use these across a wide range of applications, including decentralized exchanges, stablecoins, and lending protocols.

Funded by a [Web3 Foundation grant](https://web3.foundation/grants/), the BTC-Parachain is implemented in [Rust](https://www.rust-lang.org/) using [Parity](https://www.parity.io/)'s [Substrate framework](https://substrate.dev/).

### Helpful Links

- [Polkadot's blog post explaining interBTC](https://polkadot.network/bitcoin-is-coming-to-polkadot/)

- [BTC Parachain specification](https://interlay.gitlab.io/polkabtc-spec/)

- [BTC Parachain open-source code](https://github.com/interlay/btc-parachain)

- [XCLAIM peer-reviewed paper](https://eprint.iacr.org/2018/643.pdf)

- [Interlay homepage](https://www.interlay.io/)

## Security Guarantees: Trustless and Fully Decentralized

What makes the BTC-Parachain unique is the strict dedication to being trustless and decentralized:

- **Trustless**. The bridge has no central authority. Right from the start, the BTC-Parachain will be run by a decentralized network of individuals, community members, and companies.
- **Decentralized**. In the spirit of permissionless systems like Bitcoin, anyone can participate in operating the bridge: contrary to other approaches, you do not need permission or any additional token to become a maintainer and start earning fees.

As a holder of interBTC, you have the following guarantee:

?> You can always redeem interBTC for BTC, or be reimbursed in the collateral currency at a beneficial rate.

In the case that a vault misbehaves, you will be reimbursed from the Vault’s collateral and will make a very profitable trade between BTC and DOT. At launch, collateral will be put down in DOT. In the mid/long run, this may be extended to stablecoins or token-sets to improve stability.

Summarizing, to trust the bridge, you only need to:

- Trust that Bitcoin is secure. Meaning: trust that Bitcoin blocks are final after X confirmations. The bridge will recommend a minimum of 6 confirmations, though users and apps are encouraged to set higher thresholds.
- Trust that Polkadot is secure. This assumption is made by all applications running on top of Polkadot.

## Design: The XCLAIM Framework

### Cryptocurrency-Backed Assets

At the core, XCLAIM - the framework underlying the BTC-Parachain - introduces the concept of cryptocurrency-back assets. Assets are locked on Bitcoin and unlocked on Polkadot in form of 1:1 BTC backed-assets (interBTC). interBTC can be used just like any native asset within the Polkadot ecosystem, meaning: they can be easily transferred and integrated into other Parachains and applications.

![CbA](https://cdn-images-1.medium.com/max/3200/0*7K1rmj7j0Cya0eB_)

### Issue, Trade, Redeem

[XCLAIM](https://xclaim.io) consists of three main protocols, which also resemble the life-cycle of interBTC:

- **Issue**: Users create interBTC on the BTC-Parachain by locking BTC with Vaults — non-trusted and collateralized intermediaries on Bitcoin (see below).

- **Transfer**: Users transfer interBTC to other users or migrate to other Parachains within the Polkadot ecosystem, integrating with stablecoins, decentralized exchanges, lending protocols etc.

- **Redeem**: Users burn interBTC on the BTC-Parachain to receive the *equivalent* amount of BTC from Vaults on Bitcoin.

interBTC can remain on Polkadot indefinitely (no expiry date) and can be redeemed at any point in time. Users who obtain interBTC on Polkadot do not need a BTC wallet, until they decide to redeem the tokens for BTC (if at all).

### Secure, Open, Efficient.

XCLAIM guarantees users can redeem interBTC tokens for the corresponding amount of BTC or be reimbursed in the DOT at any point in time. To summarize, XCLAIM is:

- **Financially Secure**: intermediaries pledge collateral and cryptographically prove correct behavior. Any attempt of theft is automatically punished, while users are reimbursed.

- **Dynamic and Permissionless**: any user can become their own intermediary — simply, anytime, and without asking for permission. No need to rely on someone else, or any special hardware.

- **Censorship Resistant**: By design, Vaults have no influence over the Issue process. That is, no Vault can prevent a user from minting or obtaining interBTC.

- **Fast and Efficient**: XCLAIM is on average 95% faster than using classic HTLC atomic swaps with Bitcoin.

### Participating in the BTC Parachain

XCLAIM’s design has an emphasis on being open and permissionless. As such, any user can take up multiple roles at the same — but also leave the system whenever they wish. As such, to participate in the BTC Parachain, you can choose from:

**Users:** there are two types of user on the BTC Parachain:

- **Liquidity Providers** lock BTC with Vaults to mint 1:1 backed *interBTC* on the Parachain. Requirement: (1) Bitcoin wallet and (2) Polkadot wallet.

- **End-Users** obtain interBTC from liquidity providers on Polkadot and use interBTC for payments and with applications. Requirements: Polkadot wallet

Both can redeem owned interBTC for BTC at any time (requires BTC wallet).

**Vaults:** collateralized intermediaries who hold BTC locked on Bitcoin. Any user can become a Vault by simply locking DOT collateral. The only requirements are (1) a Bitcoin wallet, (2) a Polkadot wallet and (3) some DOTs.

**Relayers** make sure the BTC Parachain is up to date with the state of Bitcoin by submitting block headers to BTC-Relay, the [Parachain’s Bitcoin SPV client](https://medium.com/interlay/interlay-releases-codebase-for-btc-relay-on-polkadot-b37502ce88e3). Requirements: (1) Bitcoin full node, (2) Polkadot wallet, (3) some DOTs.

**Collators** participate in the BTC Parachain’s NPoS consensus, as per Polkadot consensus rules. Requirement: (1) Parachain full node and (2) DOTs.

### BTC Parachain Components

All roles are coordinated through the **Parachain Execution Environment**, which encodes the functionality to issue, transfer and redeem interBTC, and enforce correct behavior of Vaults. Thereby, the Parachain implements a multi-stage collateralization scheme, to protect against exchange rate fluctuations. The Parachain also verifies the correct execution of payments on Bitcoin via BTC-Relay, a Substrate Bitcoin SPV client. To transfer interBTC to other Parachains, an integration with Polkadot’s Cross-Chain Message Passing (XCMP) will be provided.

![The BTC-Parachain is Polkadot’s trustless gateway for Bitcoin.](https://cdn-images-1.medium.com/max/3200/0*v1lfJ1ZK75luh16s)*The BTC-Parachain is Polkadot’s trustless gateway for Bitcoin.*

<div id="step-by-step"></div>

## From BTC to interBTC, Step by Step

XCLAIM exhibits two core protocols, Issue and Redeem, outlined below.

### Issue interBTC

A user (liquidity provider) mints new interBTC.

1. A Vault locks DOT as collateral with the interBTC bridge (the Interlay BTC Parachain).

1. A user creates an issue request with a collateralized Vault of his choosing. This reserves the Vault’s DOT collateral.

1. The user then sends BTC to the Vault.

1. The user proofs to the interBTC bridge that it send the BTC to the vault (using a transaction inclusion proof against the BTC Relay).

1. Upon successful verification of the proof, the user mints interBTC and receives the tokens to his or her account balance.

![High-level interBTC Issue process](https://cdn-images-1.medium.com/max/3200/0*3OIDfIffZskXZmi7)*High-level interBTC Issue process*

### Redeem interBTC for BTC

A user redeems interBTC for the equivalent amount of BTC or receives DOT as reimbursement.

1. To request a redeem, a user locks interBTC with the interBTC bridge (Interlay BTC Parachain).

1. The Parachain instructs a Vault to execute the redeem.

1. The Vault transfers the correct amount of BTC to the user.

1. To unlock the DOT collateral, the Vault submits a transaction inclusion proof to BTC-Relay.

1. If the proof is correct, the Parachain releases the Vault’s DOTs.

1. If no valid proof is provided on time, the Parachain slashes the Vault’s DOTs and reimburses the user at a beneficial exchange rate.

In practice, a less strict approach will be taken, where Vaults remain unpunished for being offline (not stealing!) as long as they maintain an overall satisfactory SLA.

![High-level interBTC Redeem process](https://cdn-images-1.medium.com/max/3200/0*GeYgUaeduwBxfgfN)*High-level interBTC Redeem process*
