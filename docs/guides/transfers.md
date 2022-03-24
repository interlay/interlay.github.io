# Transfer Guide

?> You need to keep a very small amount of KINT (0.000,000,001 KINT or 1000 pico KINT) in your account as existential deposit.

?> You can only transfer tokens that have unlocked! `Available for transfer` = `free` - `frozen`

## Using the App

<!-- tabs:start -->

#### **Kintsugi**
[kintsugi.interlay.io/transfer](https://kintsugi.interlay.io/transfer)


#### **Interlay**

Coming soon

#### **Testnet**

[testnet.interlay.io/transfer](https://testnet.interlay.io/transfer)

<!-- tabs:end -->

### Transfer to another address

1. Go to the Transfer page on the app. Make sure you allow polkadot.js to connect to the website.
2. Select the token you would like to transfer, e.g., KSM, KBTC, or KINT.
3. Your available balance is shown on top of the amount input field.
4. Enter the address of the recipient. This can be in any supported Polkadot address format and will automatically be converted to the Kintsugi address format for the transfer.
5. Click "Transfer" and sign the extrinsic to make a transaction.

### Cross chain transfers

?> At present we support transferring KSM between Kusama and Kintsugi

#### Kusama -> Kintsugi

1. Go to the Transfer page on the app. Make sure you allow polkadot.js to connect to the website.
2. Select the Cross Chain Transfer tab.
3. Make sure you select the correct account from the account selector at the top of the page. This is the account from which the transfer will be made.
4. Your available balance on the relay chain (Kusama) is shown above the amount field. This wil be different from the parachain balance shown at the top of the page.
5. Set the "from" chain value to Kusama and the "to" chain value to Kintsugi. You should only need to set one field, as the other field will be changed automatically.
6. Select an account from the destination select field. You can only transfer to accounts that are in your wallet.If you want to transfer using the same account, make sure that the destination account matches the account selector at the to of the page.
7. Click "Transfer" and sign the extrinsic to make a transaction.
8. After the transfer has been made, if you select the account/wallet that has received the KSM from the account selector then the top right KSM balance should be updated to include the amount of KSM transferred.

#### Kintsugi -> Kusama

1. Go to the Transfer page on the app. Make sure you allow polkadot.js to connect to the website.
2. Select the Cross Chain Transfer tab.
3. Make sure you select the correct account from the account selector at the top of the page. This is the account from which the transfer will be made.
4. Your available balance on the parachain (Kintsugi) is shown above the amount field. This wil be the same as the KSM balance shown at the top of the page.
5. Set the "from" chain value to Kintsugi and the "to" chain value to Kusama. You should only need to set one field, as the other field will be changed automatically.
6. Make sure you are transferring enough funds to cover the transfer fees and maintain the existential balance. You should see an error message below the amount field if the amount you are transferring is too low.
7. Select an account from the destination select field. You can only transfer to accounts that are in your wallet. If you want to transfer using the same account, make sure that the destination account matches the account selector at the to of the page.
8. Click "Transfer" and sign the extrinsic to make a transaction.
8. After the transfer has been made, your available KSM balance at the top of the page should decrease by the amount that has been transferred. If you want to check your balance on the relay chain you should do this on Polkadot, but you can also set the "from" chain value to Kusama (as if you were making a transfer in the other direction) to see the updated relay chain balance above the amount field. This may not update immediately, and you may need to refresh the page to see the current balance.

## Polkadot.js Developer Tab (Advanced)

#### Transfer KINT in Developer > Extrinsics > Tokens

?> Important: At the end of the transfer, write down / store the block hash of your transaction! (see [Step 3 below](kintsugi/guides?id=_3-transfer-kint-in-developer-gt-extrinsics-gt-tokens)). **Do not close the browser / tab before you do this!** Otherwise you will need to manually find your transaction in Polkadot.js.

1. To transfer KINT, select "Extrinsics" in the "Developer".

2. In the dropdown, select the "tokens" pallet.

3. Select the `transfer()` function. If you want to transfer all available tokens, you can use `transferAll()` - but be careful!

?> Important: You need to use `transfer()`, not `forceTransfer()`. Otherwise you will get a `BadOrigin` error.

4. Enter the source account.

5. Enter the destination account.

6. Select "KINT" in the "Token" dropdown.

7. Enter the amount **in pico KINT (1 KINT = 1,000,000,000,000 pico KINT)**.

8. Press "Sign Transaction". In the opened modal, enter your account password, and then click "Sign and Submit".


You will see a green success message after 10-20 seconds in the top right if the transfer was successful.



?> Reminder: 1 KINT = 1,000,000,000,000 pico KINT (12 zeroes).

![Transfer](../_assets/img/kintsugi/transfer/transfer-step-1.png)