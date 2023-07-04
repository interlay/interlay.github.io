# Custom Fees Guide

Our dapp no longer limits the user to using solely the Interlay (and Kintsugi) native token (INTR/KINT) to pay transaction fees. Any asset listed in the AMM pools can now be used to pay for transaction fees.

Under the hood, the selected fee token (e.g., DOT) is swapped into the native token (e.g., INTR) to cover fees.

You can now find a select option on every transaction form across the dapp.

![Custom Fees Select](../_assets/img/guide/custom-fees-select.png)

When you click inside the "Fee token" selector, a modal with the available options for pays fees is shown. This modal shows your balance of each token (not the fees you would pay in the token).

![Custom Fees Modal](../_assets/img/guide/custom-fees-select-modal.png)

When you select the desired token to cover for fees, an estimation is ran only when you fulfil the form.

![Custom Fees Estimate](../_assets/img/guide/custom-fees-estimate.png)

After the form is fulfilled, a fee estimaste will run. For you to execute the transaction, the necessary funds to cover tge transaction fee needs to be available. If not, make sure you make more funds available or choose another token from the available set.

![Custom Fees Error](../_assets/img/guide/custom-fees-error.png)
