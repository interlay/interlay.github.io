# Vaults

Vaults are the heart of the PolkaBTC bridge. They are responsible for maintaining the physical 1:1 peg between BTC and PolkaBTC.
Vaults receive BTC for safekeeping from users and ensure BTC remains locked while PolkaBTC exists

Vaults are **non-trusted** and **collateralized** and **any user can become a Vault** by providing DOT collateral. This means: as a user, you can freely choose any Vault you like or be your own Vault. You don’t have to trust anyone else if you want to be extra cautious.

The correct behavior of Vaults is enforced by the PolkaBTC bridge parachain. Specifically, Vaults must prove correct behavior to the BTC-Relay component - a Bitcoin SPV client implemented directly on top of Polkadot. If a Vault tries to steal BTC, this will be automatically detected and the Vault will lose its collateral - and users will be reimbursed using this collateral (at a beneficial rate).


### What do Vaults do?

1. **Provide DOT Collateral** and upload their Bitcoin public key to the PolkaBTC bridge. The amount of collateral provided determines how much BTC the Vault can accept for safekeeping / how many PolkaBTC this Vault can secure.
2. **Issue**: Vaults receive BTC from users for safekeeping. This locks the Vault's DOT collateral until BTC is redeemed again. 
2. **Redeem**: Vaults monitor the PolkaBTC bridge for redeem requests. When a user requests to redeem PolkaBTC, Vaults release BTC to the user and prove that they behaved correctly to the PolkaBTC bridge (via the BTC-Relay). Only if this proof is correct, the Vault's collateral is unlocked again.




### What do I need to become a Vault?

1. The open-source Vault client
2. A Bitcoin account ([public/private keypair](https://en.bitcoin.it/wiki/Private_key))
3. A Polkadot account ([public/private keypair](https://wiki.polkadot.network/docs/en/learn-keys))
4. Some DOTs to provide as collateral and pay for transaction fees.

Head over to ["Running a Vault"](/vault/guide) for a detailed setup guide.

## Fee Model

Vaults earn fees on issue and redeem, based on the PolkaBTC volume. 

In addition, in the first year (and subject to extension) , Vaults will receive a subsidy from the Polkadot Treasury for correctly operating the PolkaBTC bridge and providing DOT collateral (Note: this is still subject to final confirmation!).
### Pool-based Fee Distribution
Vaults earn fees based on the issues and redeemed PolkaBTC volume. To reduce variance of payouts, the PolkaBTC bride implements a **pooled fee model**. 

Each time a user issues or redeems PolkaBTC, they pay the following fees to a **global fee pool**:

- **Issue Fee**: `0.5%` of the Issue volume, paid in *PolkaBTC*
- **Redeem**: `0.5%` of the redeem volume, paid in *PolkaBTC*

From this fee pool, ``77%`` is distributed among all active Vaults based on the following two factors:

- 90% based on the Vault's **BTC in custody** ( = issued PolkaBTC) in proportion to the total locked BTC ( = issued PolkaBtc) across all Vaults
- 10% based on the Vault's **locked DOT collateral** in proportion to the total locked DOT collateral across all Vaults

Specifically, each Vault's fee is calculated according to the following formula:

    vault_fee = 
    pool * 0.9 (vault_locked_btc / total_locked_btc) 
    + pool * 0.1 * (vault_locked_dot / total_locked_dot)_

The Vault fee is paid each time an Issue or Redeem request is executed. 

### Collateral Subsidy

Initially, the Issue and Redeem fees will likely not suffice to offer a competitive APY compared to most other DeFi protocols, including the staking rewards on the Polkadot Relay Chain. 
To this end, Vault will receive a subsidy from the Polkadot Treasury. 

The exact size of the subsidy is to be determined by the Polkadot Council when parachains launch.
The aim is to offer Vaults an APY similar to that of the Relay Chain staking rewards.


## Collateral and Punishment

To ensure Vaults have no incentive to steal user's BTC, Vaults provide collateral in DOT to the PolkaBTC bridge. 
To mitigate exchange rate fluctuations, the PolkaBTC bridge employs *over-collateralization* and a *multi-level collateral balancing* scheme.
### Over-collateralization

Vaults must over-collateralize their BTC holdings by ``150%`` with DOT collateral.

This means, the amount of BTC a Vault can accept for safekeeping is calculated by:

    max_vault_btc = vault_dot_collateral / (1.5 * btc_dot_exchange_rate) 

### Collateral Re-balancing

To protect against short and long term exchange rate fluctuations, Vaults are **instructed to keep their collaterlization rate up to date**.
This can be achieved in 2 ways:

- **PolkaBTC Redeem**: if users redeem with the Vault, the collateralization ration increases. The Vault can also maintain a PolkaBTC reserve and execute self-redeems for quick rebalancing
- **Increase Collateral**: alternatively, the Vault can also add more collateral to the system.

#### Thresholds and Balancing Mechanisms
The PolkaBTC bridge introduces multiple thresholds with different actions to ensure Vaults never drop below 100% collateralization:

- **Secure Collateral**: ``150%``
- **Premium Redeem**: ``135%``
- **Vault Auction**: ``120%``
- **Vault Liquidation**: ``110%`` 


### Punishment and Service Level Agreements



## FAQ
### Who can become a Vault?
The PolkaBTC bridge is fully decentralized: anyone can run a Vault. 

- How often must Vaults come online?
- What are collateral requirements?
- What happens if a Vault fails to redeem BTC?
- What happens if a vault steals?
- What happens if a vault is undercollateralized?
- How do Vaults manage Bitcoin keys?
- How are deposit addresses generated?

