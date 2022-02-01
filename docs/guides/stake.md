# Stake

Staking allows two things in the Kintsugi and Interlay networks:
- Participate in governance by obtaining vKINT tokens form staking KINT or vINTR tokens from staking INTR
- Earning staking rewards by locking KINT or INTR

Following this guide, you will learn:

- [x] Staking your KINT/INTR tokens
- [x] Extending your lock time to increase your vKINT/vINTR stake
- [x] Withdrawing staking rewards
- [x] Withdrawing staked KINT/INTR tokens

### Short Primer on Stake-to-Vote

When staking KINT or INTR to receive vKINT or vINTR, the amount of vKINT or vINTR received depends on two factors:

- The amount of KINT/INTR staked. The higher the amount of staked KINT/INTR, the more vKINT/vINTR is generated.
- The period for which KINT/INTR is staked. The longer KINT/INTR are staked, the more vKINT/vINTR is generated.

The amount of staked KINT/INTR is equal to the amount of received vKINT/vINTR at the time of staking if the maximum staking period is chosen. For example, in Kintsugi staking 10 KINT for 96 weeks results in 10 vKINT at the time of staking. If 10 KINT are staked for 48 weeks, 5 vKINT are generated. vKINT and vINTR decrease linearly overtime.

For reference, please check the maximum staking periods for [Kintsugi](kintsugi/governance#hard-facts) and [Interlay](interlay/governance#hard-facts).

## Stake KINT or INTR

You can lock KINT/INTR to receive vKINT/vINTR. If you plan to participate in governance, please check the [minimum staking requirements per network](guides/governance#required-tokens).

### App

Will be added soon.

### Polkadot.js (Advanced)

?> This is a low-level interface intended for advanced users.

Make sure you connect to the correct parachain in [polkadot.js.org/apps](https://polkadot.js.org/apps).

#### 1. Lock KINT/INTR

- Go to Developer -> Extrinsics -> escrow -> createLock
- Enter the `amount` of tokens you would like to lock. For KINT, convert by 1 KINT = 1 * 10^12 planck KINT. For example, if you want to lock 50 KINT, you would enter 50,000,000,000,000 planck KINT.
- Enter the time the KINT should be locked for. KINT will not be accessible before the lock expires. The `unlockHeight` is specified in numbers of blocks and rounds down to the closest week. One week is equal to 50,400 blocks. For example, if the chain is currently at block height 100,000 and you wish to lock tokens for 10 weeks, you would enter current block height + 10 weeks * 50,400 blocks = 100,000 + 10 * 50,400 = 604,000.
- Submit the extrinsic to lock the tokens.

![Lock KINT](../_assets/img/guide/governance-stake-to-vote-1.png)

#### 2. Check locked KINT/INTR

- Go to Developer -> Chain Sate -> escrow -> locked
- The `amount` is the KINT locked in planck KINT
- The `end` is the block number at which all KINT can be withdrawn

![Check KINT](../_assets/img/guide/governance-stake-to-vote-2.png)

#### 3. Check vKINT/vINTR balance

The vKINT and vINTR balances decrease linearly over time.
To calculate the current balance, we use the following [formula](https://spec.interlay.io/spec/escrow.html#point):

`balance = bias - (slope * (now - height))`

- `now`: Go to Explorer and note down the current block height. This is the now value.
- `bias`, `height`, and `slope`: Go to Developer - Chain State -> escrow -> userPointEpoch and note down the current epoch. Then go to Developer - Chain State -> escrow -> userPointHistory and insert your account as well as the epoch from the previous step. This will give the current `bias` and `slope` as well as the `height` (see `ts`) in which tokens were locked.

In the example in the images, we have 500 KINT locked for four weeks. This results in:

19,424,086,457,961 - (103,339,947 * (64,100 - 64,037)) = 19,417,576,041,300 planck vKINT = 19.4 vKINT

![Check vKINT](../_assets/img/guide/governance-stake-to-vote-3.png)

## Extend Staking Lock

### App

Will be added soon.

## Withdraw Staking Rewards

### App

Will be added soon.

## Withdraw Staked KINT or INTR

### App

Will be added soon.
