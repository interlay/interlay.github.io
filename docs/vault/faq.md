# FAQ

### Who can become a Vault?

The interBTC bridge is fully decentralized: anyone can run a Vault.

### How many Vaults can there be?

As many as want to participate.

### What is the minimum number of required Vaults?

One honest and online Vault is enough to allow users to move BTC to Polkadot. A user can even be their own Vault. The more Vaults, however, the better.

### Can I run multiple Vaults?

Definitely! Please only use one unique key per client to avoid race conditions on signing. Additionally, if a Vault is started using the same key name as another that was already running it will load the same Bitcoin wallet and may attempt to transfer "locked" funds. If this happens the Vault may not have enough funds to back locked KBTC/IBTC.

### Do Vaults earn fees?

Yes! Vaults earn fees in KBTC/IBTC and KINT/INTR.

### How often must Vaults come online?

Vaults **should remain online** to check for issue and, most importantly, redeem requests.
Currently, Vaults have ``24 hours`` to execute a redeem request.
As such, a Vault must come online **at least once every ``24 hours``**.

We recommend Vaults remain online to avoid failing redeem requests in times of high network load on Bitcoin.

### What are collateral requirements?

Vaults must lock up collateral equivalent to the locked BTC value based on the [network thresholds](/vault/overview?id=collateral-thresholds).

### What happens if a Vault fails to redeem BTC?

The Vault will be slashed a punishment fee and the user can (i) chose to retry with another Vault or (ii) claim the Vault's collateral.

### What happens if a Vault steals?

If a Vault fails to fulfill a redeem request on time, it incurs a punishment fee (paid to the redeeming user) and can be liquidated, at the choice of the redeeming user. Vaults can freely move around BTC (e.g. between hot and cold storage) they have received, as long as they execute redeem requests on time.  

An earlier version of the protocol had a "theft reporting" feature which prevented Vaults from moving BTC, unless there was a redeem/replace request. Theft reporting was removed [here](https://github.com/interlay/interbtc/pull/677) in accordance with [this post-mortem](https://medium.com/interlay/kintsugi-released-urgent-security-patches-aebf969ee087) since it does not add to the economic security of the bridge and with the purpose of improving Vault flexibility. The proposals to enact this change were successfully executed on Kintsugi and Interlay.

### What happens if a vault is undercollateralized?

The Vault must re-balance its collateral. If it fails to do so, the interBTC bridge has a multi-stage collateral scheme in place. See [here](/vault/overview?id=over-collateralization) for more details.

### How do Vaults manage Bitcoin keys?

Vaults utilise the local Bitcoin Core full-node for key management.

Each Vault must submit a Bitcoin public key (the "master" key) when registering with the interBTC bridge.
When the Vault (re)starts it will check that this key is still held by the Bitcoin wallet.

!> Never transfer funds manually from the Bitcoin wallet.

### How are deposit addresses generated?

Each time a user requests to issue new interBTC, the interBTC bridge uses the master key of the selected Vault to derive Bitcoin addresses (controlled by this Vault) via an on [on-chain key derivation scheme](https://interlay.gitlab.io/interbtc-spec/security_performance/security-analysis.html). This address is used by the user for the BTC deposit.

### What if all Relayers go offline?

While unlikely, if all Relayers go offline the interBTC bridge will pause operation.

## Troubleshooting

### Insufficient funds

The Vault does not have sufficient BTC to process outstanding requests, this is likely due to the overpayment of Bitcoin fees relative to what was allocated by the parachain - when the Vault spends from more UTXOs it will cost more. You **MUST** transfer additional BTC to cover fees as specified [here](/vault/guide?id=bitcoin-fees). We plan to assign treasury funds to reimburse Vault operators as well as improve the on-chain estimations.

### Failed to obtain public key

On startup, the Vault will check that your Bitcoin full node still has access to the key it registered with.
If this is not the case, something may have happened to your [wallet](https://en.bitcoin.it/wiki/Wallet) file.

### Fee estimation failed. Fallbackfee is disabled.

Sometimes it is not possible for Bitcoin to estimate the transaction fees for redeem requests.
To avoid this happening, set a sensible default on startup such as `-fallbackfee=0.0002`.

### No available targets are compatible with this triple.

The secp256k1 elliptic-curve dependency used for generating Vault addresses requires a newer version of [Clang](https://clang.llvm.org/). Please download the latest available version for your distribution or check the minimum supported version in the build instructions.
