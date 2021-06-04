# InterBTC

## Financially Trustless Bitcoin on Polkadot

At the core, interBTC leverages the concept of cryptocurrency-back assets introduced in the XCLAIM protocol. Assets are locked on Bitcoin and unlocked on Polkadot in form of 1:1 BTC backed-assets (interBTC). interBTC can be used just like any native asset within the Polkadot ecosystem, meaning: they can be easily transferred and integrated into other Parachains and applications.

![Cryptocurrency-backed Assets](_assets/img/CbA.png)

### Issue, Trade, and Redeem

The life-cycle of interBTC follows the three main protocols:

- **Issue:** Users create interBTC on the BTC-Parachain by locking BTC with Vaults — non-trusted and collateralized intermediaries on Bitcoin.
- **Transfer:** Users transfer interBTC to other users or migrate to other Parachains within the Polkadot ecosystem, integrating with stablecoins, decentralized exchanges, lending protocols etc.
- **Redeem:** Users burn interBTC on the BTC-Parachain to receive the equivalent amount of BTC from Vaults on Bitcoin.

interBTC can remain on Polkadot indefinitely (no expiry date) and can be redeemed at any point in time. Users who obtain interBTC on Polkadot do not need a BTC wallet, until they decide to redeem the tokens for BTC (if at all).

### Secure, Open, Efficient.

XCLAIM guarantees users can redeem interBTC tokens for the corresponding amount of BTC or be reimbursed in the DOT at any point in time. To summarize, XCLAIM is:

- **Financially Secure:** Vaults (intermediaries) pledge collateral and cryptographically prove correct behavior. Any attempt of theft is automatically punished and users are reimbursed.

- **Dynamic and Permissionless:** any user can become a Vault — simply, anytime, and without asking for permission. No need to rely on someone else, or any special hardware. You can even run your own Vault for issuing interBTC.

- **Censorship Resistant:** By design, Vaults have no influence over the Issue process. That is, no Vault can prevent a user from minting or obtaining interBTC.

- **Fast and Efficient:** XCLAIM is on average 95% faster than using classic HTLC atomic swaps with Bitcoin.

## Guides

###### [1. Get a detailed overview of interBTC](start/overview.md)

###### [2. Issue your first interBTC](start/issue.md)

###### [3. Redeem interBTC for BTC](start/redeem.md)

###### [4. Understand how a Vault works](vault/overview.md)

###### [5. Operate your own Vault](vault/guide.md)

###### [6. Understand how a Relayer works](relayer/overview.md)

###### [7. Operate your own Relayer](relayer/guide.md)

###### [8. Build your own Dapps with interBTC](developers/integration.md)

## Contributions

interBTC is an open-source project. We welcome contributions to the code and documentation. Feel free to checkout our [GitHub](https://github.com/interlay) or [Discord](https://discord.gg/KgCYK3MKSf).
