# FAQ

### How does interBTC maintain its peg?

The interBTC bridge ensures that for each minted interBTC: (1) there is an equivalent amount of BTC physically locked on Bitcoin, and (2) there is sufficient DOT collateral locked to economically back minted interBTC in case vaults attempt theft. As such, interBTC is a hybrid peg, with supply determined by both locked BTC and DOT.

### Who are the vaults? Can anyone become a vault?

Vaults maintain the physical backing of interBTC: they ensure BTC remains locked while interBTC exists. Anyone can become a vault. You only need: (1) a Bitcoin wallet, (2) a Polkadot wallet, and (3) to register by lock DOT collateral (secures the BTC you get to hold in custody).

### Can I always get my BTC back?

interBTC guarantees that you will either (1) get you BTC back whenever you want, or (2) you will be reimbursed in DOT at a beneficial rate (better than BTC/DOT market price).

### I am waiting 2 hours for my redeem request, why is it not fulfilled yet?

Vaults have 24 hours to complete redeem requests. The system is decentralized, i.e. we are not controlling the Vaults that are running. If Vaults are offline, i.e. the client is not running, redeem requests cannot be fulfilled.

### What happens if a Vault does not fulfill the redeem request within 24 hours?

You will have two options, both should leave you with a financial plus relative to the USD.

- **Retry**: You redeem with another vault. You will get a a payment in DOT that is slashed from the vault for the inconvenience. This will be much higher than your transaction cost so you will have a net plus.
- **Burn**: You can also burn your interBTC. In this case the financial value of your interBTC that you are burning is paid to you in DOT plus the inconvenience fee similar to the retry. In USD terms this will also be a profit for you. These DOT are also slashed from the vault.

This will also be the case in production. Since the bridge is decentralized and anyone can run a vault, we cannot enforce them to redeem. But what the system can and is doing, is reimbursing users and slashing vaults for misbehavior.

?> Testnet Note: The number of failed redeems on the testnet will be likely much higher than on the production network since (1) there are no financial repurcssions (we are using testnet DOT without real value) for vaults, and (2) the Vault clients are undergoing improvements right now and we are working to get them to be operating stable in all edge cases.


### Why does the interBTC app tell me that 0 interBTC can be issued at the moment?

There can be two reasons for this:

- **Reached maximum capacity**: If the capacity indicated on the [dashboard](https://bridge.interlay.io/dashboard) shows that the number of issued interBTC is equal to the capacity, you cannot issue more interBTC. Each interBTC must be backed by 150% worth of DOT collateral and this represents the upper limit.
- **Connection to interBTC bridge lost**: In case the connection to the bridge is lost, the UI will fallback to 0 interBTC being able to be issued.

?> Testnet Note: The number of concurrent connections to our chain instances is restricted to monitor the load. Best course of action is to retry after some time/reload the website. We are looking to increase the load.


### What does it the Error security.ParachainOracleOfflineError mean?

Oracles need to submit up-to-date prices every hour. When the oracles are offline for an hour or longer, the interBTC bridge automatically goes to a halting mode. As a security measure, you cannot issue or redeem interBTC at that point.

?> Testnet Note: We are testing all parts of the system as part of the testnet. On mainnet, we will have a number of different oracle providers that should make this error very unlikely.

### What does the Error security.ParachainNotRunning mean?

The interBTC bridge protocols can be halted for a number of reasons. Halting means that users cannot issue or redeem at those points since we require an update to the chain. Reasons to halt the chain include oracles being offline or a Bitcoin fork.

### Why is the price of interBTC different to BTC?

interBTC might trade at a slight premium, driven by supply/demand for Bitcoin on Polkadot (e.g., to cover issue fees). This is similar to diverging prices across different exchanges.

### Does interBTC require a price oracle?

interBTC guarantees that users (1) can redeem interBTC for BTC, or (2) are reimbursed in DOT at a beneficial rate in case of failure. A price oracle is needed to maintain a secure BTC/DOT collateralization rate.

### Has interBTC been audited?

interBTC is being audited by [NCC](https://www.nccgroup.com/), a top-tier security and cryptography auditor.

### Why is interBTC better than other BTC bridges?

interBTC is based on XCLAIM - a top-tier, peer-reviewed research paper. interBTC is fully permissionless and decentralized: anyone can become a vault without asking for permission and you can run your own vault. interBTC uses collateralization and cross-chain SPV proofs to guarantee that users never face financial damage.  Some projects rely on centralized parties, making them vulnerable to theft, seizure, and censorship. Other projects use techniques similar to interBTC, but are permissioned, rely on complex and non-battle-tested cryptographic protocols, or heavily rely on the price of their governance token for security of user funds.

## More questions?

Reach out on [Discord](https://discord.gg/KgCYK3MKSf) or [open an issue](https://github.com/interlay/interbtc-docs/issues).
