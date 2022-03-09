# Token Vesting


## Crowdloan Vesting 

### Vesting Schedule

Tokens received via the crowdloan airdrop are subject to vesting.
Your vested tokens are shown as "locked" or "frozen" (depending on the wallet/platform).

<!-- tabs:start -->

#### **Kintsugi**

Vesting stared on 13 October (when Kintsugi won a parachain). 

Unlocking happens **per ~block~ week** (see below).


?> **Kintsugi Vesting Issue (as of 9 March 2022)**. 70% of the KINT tokens should vest for a total of 48 weeks. However, the current vesting schedule is longer. The root cause for this issue stems from the fact that parachains should produce a block every 12 seconds, but in reality block production is slower (22 seconds at the moment). This problem affects other Kusama parachains as well.
**Solution**: once block times stabilize (upcoming Kusama upgrade looks promising) the vesting schedules of all participants will be updated to reflect the correct unlocked KINT amount.

You can calculate based on this formula:

`my_tokens x 0.3 + (tokens x 0.7 x days_since_13oct2021 / 336)`

where `my_tokens` is the total amount of KINT you received in the airdrop and `days_since_13oct2021` is the number of days that passed since 13 October 2021 when vesting started.

#### **Interlay**

Coming soon

<!-- tabs:end -->

## Claim Unlocked Tokens

To receive your tokens into your account once they have unlocked, you need to make a "claim" transaction on the parachain.

### Via App

Coming soon.


### Via Polkadot.js Developers Tab

To claim the latest unlocked tokens, do the following:

1. Go to "Developers" > "Extrinsics"
2. Select "vesting" in the extrinsic dropdown
3. Select the "claim()" function
4. Submit Transaction

If you had tokens that have unlocked, you will see them in your balance.
