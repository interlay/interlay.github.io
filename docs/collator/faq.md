# FAQ

### Who can become a Collator?

The interBTC bridge is fully decentralized: anyone can run a Collator.

### Do Collators earn fees?

Collators earn transaction fees made by users for preparing blocks.

## Troubleshooting

### Bootnode with peer id `xxx` is on a different chain

The genesis chain spec is invalid, please download the file specified in the instructions.

### Storage root must match that calculated

The state cache in Substrate has several bugs, nodes [are recommended](https://github.com/paritytech/substrate/issues/9697#issuecomment-982501753) to run with `--state-cache-size=0` set. This will no longer be required after [this change](https://github.com/paritytech/substrate/pull/11407) has been released.
