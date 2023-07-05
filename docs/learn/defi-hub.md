# BTC DeFi Hub

Interlay v2 features a set of financial tools, offering Bitcoin users
decentralized access to trading, borrowing, lending and other
primitives.

## Liquidity Protocol

Interlay v2 introduces support for borrowing and lending of iBTC and
other assets through a pool-based liquidity protocol, based on the
design of [Compound v2](https://docs.compound.finance/v2/).

### Lending Pools

Assets supplied by lenders into a lending pool are represented by a
fungible "qToken\" balance. Subject to the supply of the pool exceeding
the borrowed amount, qTokens can be redeemed for the underlying assets.
As the protocol accrues interest, subject to borrowing demand, the
amount of the underlying asset redeemable by each qToken increases.
Thereby, generated interest is distributed among lenders of each pool on
a pro-rata basis.

To borrow assets, users must deposit qTokens as collateral. Borrowing
contracts are open-ended while rates follow the models encoded in the
protocol. Each loan must be backed by collateral at a loan-to-value
(LTV) ration below 1.0 to ensure that borrowers have an economic
incentive to repay their loans. The interest accrued by a loan, payable
in the underling asset, continuously increases the LTV ratio.

Each asset that can be supplied into lending pools must be whitelisted
by Interlay network governance. Assets (qToken representations) that can
be used as collateral for borrowing require a separate vote. This is to
ensure high quality of assets and proper risk management.

Subject to proper risk assessment by community governance, qTokens may
also be used as Vault collateral in the BTC bridge. This allows Vaults
to lend out their bridge collateral as an additional revenue stream,
significantly improving the capital efficiency of the collateralized
bridge model.

### Liquidations

If the LTV ratio of a position exceeds the borrowing capacity, as
configured by network governance on a per-asset basis, all or part of
the outstanding loan may be liquidated. During a liquidation, an
arbitrageur repays (parts of) the outstanding loan in return for the
borrower's qToken collateral at the current market price minus a
*liquidation discount*. This process can be executed by any user and
repeated until a healthy LTV ratio is restored.

### Interest Rate Model

The Interlay liquidity protocol utilizes an interest rate model to
balance lending supply and borrowing demand, and incentivize liquidity.
High demand for an asset leads to a decline in liquidity of that asset.
The protocol reacts by increasing interest rates, which makes borrowing
more expensive and incentivizes supply (and vice-versa).

## Decentralized Exchange 

To unlock easy access to trading for BTC holders, Interlay v2 introduces
a decentralized exchange (DEX). The DEX serves as capital source for
liquidations on in the liquidity protocol. Further, the combination of
lending / borrowing with trading transactions unlocks a variety of
financial products for Bitcoin, including leverage and hedging. In the
first iteration, the goal of the DEX is to create deep iBTC liquidity,
pairing all major listed assets with iBTC: trades between any two assets
should be able to be routed via iBTC.

The DEX supports the following automated market maker (AMM) functions:

1.  Constant product AMM($XY=K$), following the [Uniswap v2
    design](https://uniswap.org/whitepaper.pdf), which allows pairing iBTC with any other
    crypto asset.

2.  [Curve StableSwap AMM](https://berkeley-defi.github.io/assets/material/StableSwap.pdf) for low-slippage swaps
    between assets which are expected to trade at the same value, e.g.,
    iBTC and wBTC.

Liquidity positions in the DEX are represented through "LP-tokens\",
which can be transferred and potentially traded themselves. Subject to
proper risk assessment by community governance, LP-tokens may also be
used as Vault collateral in the BTC bridge.
