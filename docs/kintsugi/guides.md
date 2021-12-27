# Kintsugi Guides

At launch, Kintsugi Network can be accessed via [Polkadot.js Apps](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Fapi-kusama.interlay.io%2Fparachain#/explorer).

This is a very powerful tool, but it is not easy to use. Please follow the guides below.

?> Explorer and wallet support is WIP and will be added soon.


## Claiming KINT Crowdloan Airdrop

Follow this guide to claim KINT. This guide applies the following contributors:

* Contributions via https://kintsugi.interlay.io/, please use the account that you have used for the contribution
* Contributions via https://polkadot.js.org/apps/#/, please use the account that you have used for the contribution
* Contributions via the Bifrost SALP (https://bifrost.app/vcrowdloan?tab=ksm), please use the account that you have used for the contribution on the Bifrost app

Other contributions, e.g., via exchanges will airdrop KINT tokens through other means. If you have contributed via any other means than the ones listed above, please contact the provider of the contribution for the KINT airdrop.

#### 1. Go to the KINT claim website at [ https://kintsugi.interlay.io/](https://kintsugi.interlay.io/)

Scroll down to the claim form. If you have contributed to the Kintsugi crowdloan, you will see your estimated KINT airdrop together with your KSM locked.

You MUST read and accept the terms and conditions to qualify for the airdrop.

![Claim KINT](../_assets/img/kintsugi/claim_kint_1.png)

#### 2. Sign the Terms and Conditions by clicking the "Claim KINT" button

You will be asked to sign the terms and conditions via the polkadot.js extension.

?> There are **no** transaction fees for signing the T&Cs.

![Sign TCs](../_assets/img/kintsugi/claim_kint_2.png)

#### 3. Successfully claimed KINT

It may take up to 48 hours to see and access your KINT in your wallet. Thank you for participating.

![Wait](../_assets/img/kintsugi/claim_kint_3.png)


## Update Metadata in Polkadot.js  (Repetitive!)

!> Currently, you need to update the Metadata each time you open Polkadot.js in a new tab or browser. This issue is being worked on.


#### 1. Go to [ https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Fapi-kusama.interlay.io%2Fparachain#/explorer](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Fapi-kusama.interlay.io%2Fparachain#/explorer)

Make sure you check that "Hosted by Kinstugi Labs" is selected in the explorer sidebar.

![Select hoster](../_assets/img/kintsugi/check-balance/1_select-kintsugi-chain.png)

#### 2. Wait for Metadata to Update

Go to the "Settings" > "Metadata" tab. 

There are two cases:

(A) If you see the "Update metadata" button, click it and confirm if the modal that will open.

(B) If you do not see any button, wait for 1-2 seconds. You will see the QR code changes.

?> In most cases, there is no "update" button -  **the QR code will change automatically**. This means the metadata was updated.

![Update metadata](../_assets/img/kintsugi/check-balance/2_update-metadata.png)


## Checking KINT Balance

Withing ~48 hours of accepting the T&Cs and submitting the claim form, you should receive airdropped tokens in you account. Follow these steps to check your balance.

!> Polkadot.js Accounts tab will **not show your KINT balance**. Kintsugi has a generic multi-currency pallet, which allows adding any token easily. However, Polkadot.js does not support this - yet. **Follow the instructions below to view your KINT balance via Polkadot.js Developer tab**

#### 1. [ Update Metadata](kintsugi/guides?id=update-metadata-in-polkadotjs-repetitive)

To be sure everything works, please do this step first!

#### 2. View Balance in Developer > Chain state > Token

In the "Developer" tab select "Chain state".

![Go to chain state in Developer tab](../_assets/img/kintsugi/check-balance/3_chainstate.png)

Then, in the dropdown, select the "token" pallet.

![](../_assets/img/kintsugi/check-balance/4_select-tokens.png)

Select your Account in the top-level dropdown, and select "KINT" in the currency selector dropdown (see image).

Then click "+" in the top right of the form.

![Enter account and select KINT currency](../_assets/img/kintsugi/check-balance/5_enter-form.png)


You will now see your KINT balance as follows:

![View balance](../_assets/img/kintsugi/check-balance/6_view_balance.png)

?> Amounts are shown in **pico KINT** - the smallest currency unit on Kintsugi. To get to the actual KINT **divide by 1,000,000,000,000** (remove 12 decimal points). This is a Polkadot.js feature.

- **free** shows your **total KINT balance**
- **frozen** shows how much of your KINT are **still vesting**


## Transfer KINT

?> Important: At the end of the transfer, write down / store the block hash of your transaction! (see [Step 3 below](kintsugi/guides?id=_3-transfer-kint-in-developer-gt-extrinsics-gt-tokens)). **Do not close the browser / tab before you do this!** Otherwise you will need to manually find your transaction in Polkadot.js. 


#### 1. [ Update Metadata](kintsugi/guides?id=update-metadata-in-polkadotjs-repetitive)

To be sure everything works, please do this step first!

#### 2. Transfer KINT in Developer > Extrinsics > Tokens

1. To transfer KINT, select "Extrsinsics" in the "Developer".

2. In the dropdown, select the "tokens" pallet.

3. Enter the source account.
   
4. Enter the destination account.

5. Select "KINT" in the "Token" dropdown.

6. Enter the amount **in pico KINT (1 KINT = 1,000,000,000,000 pico KINT)**.
7. Press "Sign Transaction". In the opened modal, enter your account password, and then click "Sign and Submit".


You will see a green success message after 10-20 seconds in the top right if the transfer was successful.



?> Reminder: 1 KINT = 1,000,000,000,000 pico KINT (12 zeroes).

![Transfer](../_assets/img/kintsugi/transfer/transfer-step-1.png)


!> Important: write down / save the block hash of your transaction!

#### 3.View and Save Transaction Details


**Option 1: RPC Call Log**

1. Go to Developer > RPC Calls.
2. In the `author` endpoint (selected by default) you can see a list of events for your accounts.
3. **Copy and save the contents of the latest `author.submitAndWatchExtrinsic` text box on your PC (recommended)**. Alternatively, you can copy and save the block hash shown in `status > InBlock` at the bottom of the text box

![Save TX Info - 1](../_assets/img/kintsugi/transfer/save-tx-info.png)

The data in the box will look something like this:

```
{
  dispatchInfo: {
    weight: 594,000,000
    class: Normal
    paysFee: Yes
  }
  events: [
    {
      phase: {
        ApplyExtrinsic: 2
      }
      event: {
        method: Transfer
        section: tokens
        index: 0x0702
        data: [
          {
            Token: KINT
          }
          a3df.......FU
          a3ct.......tg
          100,000,000
        ]
      }
      topics: []
    }
    {
      phase: {
        ApplyExtrinsic: 2
      }
      event: {
        method: ExtrinsicSuccess
        section: system
        index: 0x0000
        data: [
          {
            weight: 594,000,000
            class: Normal
            paysFee: Yes
          }
        ]
      }
      topics: []
    }
  ]
  status: {
    InBlock: 0x73d7af.......
  }
}
```

It shows the list of events, including the `Transfer` you just made - here you can also see the sender and receiver accounts, and the amount of KINT sent (in pico KINT).

At the bottom of the box you will find the `status` and `InBlock` fields - this shows you the block hash of the block in which your transfer was included. 

?> Use the block hash to search for the block in the [Netork tab](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Fapi-kusama.interlay.io%2Fparachain#/explorer) of Polkadot.js - for example, if you need help with your transaction.


**Option 2: Network Tab (manual search)**

1. Go to the [Netork tab](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Fapi-kusama.interlay.io%2Fparachain#/explorer) of Polkadot.js
2. Under `recent events` check the latest `tokens.Transfer` events to find your account (no search function available).+
3. **Save and click on the block number** next to your `tokens.Transfer` event. Note: this shows (a) the block number and (b) the index )(= position) of your transaction in that block (e.g.  index 0 means it is the first transaction).

![Save TX Info - 2](../_assets/img/kintsugi/transfer/find-tx-network-tab.png)


## Find a Transfer Transaction

1. Go to the Kintsugi [Network tab](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Fapi-kusama.interlay.io%2Fparachain#/explorer) in Polkadot.js.
2. Enter the (A) block hash or (B) the block number of your transaction ([see above](/kintsugi/guides?id=_3view-and-save-transaction-details) - you need to save this when you make a transfer!).
3. You will see all events in that block. Check the `tokens.Transfer` events to find your transaction (no search function available yet).

![Check block](../_assets/img/kintsugi/find-tx/check-block.png)
