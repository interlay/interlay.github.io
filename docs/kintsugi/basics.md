## Address Format

Kintsugi uses the Substrate SS58 address format as [described here](https://wiki.polkadot.network/docs/learn-accounts).

- Polkadot addresses **always start with** the number **1**.
- Kusama addresses always start with a capital letter like **C, D, F, G, H, J**.
- Generic Substrate addresses **always start with** the number **5**.
- Kintsugi addresses **always start with** the value **a3**.
- Interlay addresses **always start with** the value **wd**.

## Transaction Fees

Resources such as computation and storage are limited, transaction fees are used to prevent users from over-consuming them. As in [Polkadot](https://wiki.polkadot.network/docs/learn-transaction-fees), Kintsugi uses a weight-based fee model that charges the user prior to execution. These fees are paid in $KINT based on the complexity of the interaction.

There are three parameters to consider in the calculation of a transaction fee:

- The per-byte fee (also known as the "length fee").
- The weight fee.
- A tip (optional).

In the following snippet we have included some rough code to estimate the inclusion fee for an encoded extrinsic.

```rust
fn compute_fee<T: pallet_transaction_payment::Config>(encoded_xt: Bytes) -> Balance {
    let unchecked_xt: Block::Extrinsic = Decode::decode(&mut &*encoded_xt).unwrap();
    let dispatch_info = <Extrinsic as GetDispatchInfo>::get_dispatch_info(&unchecked_xt);

    let base_fee = T::ExtrinsicBaseWeight::get();
    let len_fee = T::TransactionByteFee::get() * encoded_xt.len();
    
    let unadjusted_weight_fee = min(dispatch_info.weight, T::BlockWeights::get().max_block);
    let multiplier = pallet_transaction_payment::Pallet::<T>::next_fee_multiplier();
    let adjusted_weight_fee = multiplier * unadjusted_weight_fee;
    
    let inclusion_fee = base_fee + len_fee + adjusted_weight_fee;
    return inclusion_fee;
}
```

It is also possible to use the `payment.queryFeeDetails(encoded_xt)` RPC to calculate this dynamically.

Weights are 1:1 with $KINT which means that an extrinsic marked with `#[pallet::weight(100000000)]` will cost at least 0.0001 $KINT in addition to the base and length fees.

Tips are optional and can be added atop the inclusion fee to increase priority.

### Fee Estimates

The following are rough fee estimates measured on the `standalone` node:

- **Kintsugi**
  - **tokens.transfer**: 0.00073 KINT
  - **vaultRegistry.registerVault**: 0.0015 KINT
  - **issue.requestIssue**: 0.0014 KINT

?> Real transactions will be charged on other factors including network congestion.