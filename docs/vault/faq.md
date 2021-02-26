# FAQ

### Who can become a Vault?

The PolkaBTC bridge is fully decentralized: anyone can run a Vault.

### How many Vaults can there be?

As many as want to participate.

### What is the minimum number of required Vaults?

One honest and online Vault is enough to allow users to move BTC to Polkadot. A user can even be his/her own Vault. The more Vaults, however, the better.

### Do Vaults earn fees?

Yes! Vaults earn fees in PolkaBTC and DOT.

### How often must Vaults come online?

Vaults **should remain online** to check for issue and, most importantly, redeem requests.
Currently, Vaults have ``24 hours`` to execute a redeem request.
As such, a Vault must come online **at least once every ``24 hours``**.

We recommend Vaults remain online to avoid failing redeem requests in times of high network load on Bitcoin.

### What are collateral requirements?

Vaults must lock up DOT collateral worth ``150%`` of the locked BTC value.

### What happens if a Vault fails to redeem BTC?

The Vault will be slashed based on its SLA and the user can (i) chose to retry with another Vault or (ii) claim the Vault's collateral.

### What happens if a vault steals?

The Vault's collateral, up to ``150%`` of the stolen BTC value at the current exchange rate, is slashed and the PolkaBTC bridge initiates a [**Burn Event**](/overview?id=burn-event-restoring-a-11-physical-peg).
See [here](/vault/overview?id=collateral) for more details.

### What happens if a vault is undercollateralized?

The Vault must re-balance its collateral. If it fails to do so, the PolkaBTC bridge has a multi-stage collateral scheme in place. See [here](/vault/overview?id=over-collateralization) for more details.

### How do Vaults manage Bitcoin keys?

Vaults are responsible for managing their own Bitcoin private keys.
Each Vault must submit a Bitcoin public key (the "master" key) when registering with the PolkaBTC bridge.

The Vault client uses a separate wallet file, specified upon start-up, which is imported into the Vault's local Bitcoin full node (Bitcoin Core wallet).

### How are deposit addresses generated?

Each time a user requests to issue new PolkaBTC, the PolkaBTC bridge uses the master key of the selected Vault to derive Bitcoin addresses (controlled by this Vault) via an on [on-chain key derivation scheme](https://interlay.gitlab.io/polkabtc-spec/security_performance/security-analysis.html). This address is used by the user for the BTC deposit.
