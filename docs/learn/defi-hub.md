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

## DeFi Hub Math

### Liquidity Protocol Interest Rate Model
At launch, the Interlay liquidity protocol will feature an interest
model inspired by Compound v2 - a model that is well tested in practice.
Improvements and optimizations are expected in future versions of the
protocol.

#### Utilization Rate
The utilization rate displays which percentage of the total supply is
currently borrowed and is a central parameter in determining the supply
rate.

$$UtilizationRate = \frac{TotalAmountBorrowed}{TotalCash + TotalAmountBorrowed-TotalReserves}$$
where $\mathit{TotalCash}$ is the amount of supply that is currently not
lend out, $\mathit{TotalAmountBorrowed}$ is the total outstanding debt,
$\mathit{TotalReserves}$ is the amount of unharvested reserves which
accrued in the pool.

#### Internal Exchange Rate
When a supplier adds tokens to the lending pool, they get credited
qToken based on the initial exchange rate. Since the qTokens accrue
interest as the TotalAmountBorrowed continually increases, the amount of
tokens they will receive at redemption will change based on the internal
exchange rate. This can be represented as:
$$InternalExchangeRate= \frac{TotalCash+TotalAmountBorrowed-TotalReserves}{TotalSupply}$$
where $\mathit{TotalCash}$ is the unborrowed supply and
$\mathit{TotalSupply}$ is the total available supply in the pool.

#### Borrowing Rate
The function below describes the borrowing rate depending on the demand
and supply for that token, represented as the utilization rate U.
$$r_{borrow} = \frac{BaseRate + U*(JumpRate - BaseRate)}{U_{target}} \vert U \leq target$$
$$r_{borrow} = \frac{JumpRate + (U - U_{target})*(FullRate-JumpRate)}{1-U_{target}} \vert U >target$$
where $\mathit{BaseRate}$ is the intercept (when utilization is zero),
$\mathit{JumpRate}$ is the borrow rate when $U = U_{\mathit{target}}$
and $\mathit{FullRate}$ corresponds to the rate when $U = 100\%$.

#### Supply Rate
The relationship between the supply rate and the borrow rate can then be
described as below. Note that the supply rate supply rate is reduced by
the fees that are attributable to the protocol.
$$SupplyRate = \frac{\mathit{BorrowRate} * \mathit{TotalAmountBorrowed}}{\mathit{TotalSupply}}*(1-\mathit{DAOFee})$$
where $\mathit{DAOFee}$ is the percentage fee that is collected by the
protocol.

### AMM Curves

#### Non-stable Pools
Exchange prices on the DEX are determined by the constant function
market maker. For non-stable pools, prices are determined via a constant
product function in the form of
$$K = x * y$$
where K is a constant, x and y are the supplies of tokens X and Y,
respectively.

#### Stable Pools
For stable pools, the DEX determines the exchange price of an asset
using the stable swap invariant first proposed by Curve
$$An^n \sum{x_i} + D = ADn^n + \frac{D^{n+1}}{n^n \prod{x_i}}$$
where $\mathit{A}$ is the amplification coefficient that determines the
liquidity concentration towards the middle of the curve, $\mathit{D}$ is
the constant (determined as the product of the amount of tokens and is
comparable to the constant K in the constant product function),
$\mathit{n}$ is the number of coins in the pool and $\mathit{x_i}$ is
the respective token.
When a trade is being executed on such a pool, the above equation must
hold. This requires to find a solution for either $\mathit{D}$ or
$\mathit{x}$, when all other variables are known, via iterative
convergence.
