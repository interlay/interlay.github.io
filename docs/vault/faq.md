# FAQ

### Who can become a Vault?

The PolkaBTC bridge is fully decentralized: anyone can run a Vault.

### How many Vaults can there be?

As many as want to participate.

### What is the minimum number of required Vaults?

One honest and online Vault is enough to allow users to move BTC to Polkadot. A user can even be his/her own Vault. The more Vaults, however, the better.

### Can I run multiple Vaults?

Definitely! Please only use one unique key per client to avoid race conditions on signing. Additionally, if a Vault is started using the same key name
as another that was already running it will load the same Bitcoin wallet and may attempt to transfer "locked" funds. Please review the response to Vault
theft below.

?> Please note that only one Vault's SLA score will count toward the testnet challenges.

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

!> Never transfer funds manually from the Bitcoin wallet as it may be considered theft.

### How are deposit addresses generated?

Each time a user requests to issue new PolkaBTC, the PolkaBTC bridge uses the master key of the selected Vault to derive Bitcoin addresses (controlled by this Vault) via an on [on-chain key derivation scheme](https://interlay.gitlab.io/polkabtc-spec/security_performance/security-analysis.html). This address is used by the user for the BTC deposit.

## Troubleshooting

### Vault has stolen BTC

Funds have unexpectedly been sent from an address registered to your Vault. This may happen if you have transferred
BTC from your wallet manually or if you have started another Vault using the same keyname.

### Failed to obtain public key

On startup, the Vault will check that your Bitcoin full node still has access to the key it registered with.
If this is not the case, something may have happened to your [wallet](https://en.bitcoin.it/wiki/Wallet) file.

### Fee estimation failed. Fallbackfee is disabled.

Sometimes it is not possible for Bitcoin to estimate the transaction fees for redeem requests.
To avoid this happening, set a sensible default on startup such as `-fallbackfee=0.0002`.

### No available targets are compatible with this triple.

The secp256k1 elliptic-curve dependency used for generating Vault addresses requires a newer version of [Clang](https://clang.llvm.org/).
Please download the latest available version for your distribution or check the minimum supported version in the build instructions.