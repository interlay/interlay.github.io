
# FAQ

### Who can become a Relayer?

Anyone, without restrictions. 

### How many Relayers can there be?

As many as want to participate. 

### What is the minimum number of required Relayers?

One honest and online Relayer is enough. The more Relayers, however, the more robust the bridge becomes. 

### How often must a Relayer come online?

Relayers are expected to be online 24/7 but at least every 10 minutes (average block interval).

### Do Relayers earn fees?

Yes! Relayers earn fees in PolkaBTC and DOT. 

### What happens if a Relayer crashes?

The [SLA score](/relayer/overview?id=service-level-agreements) of the Relayer will stall and the Relayer will earn less fees. 

### Can a Relayer steal BTC?

No. Relayers have no access to users' BTC.

### What if all Relayers go offline?

While unlikely, if all Relayers go offline the PolkaBTC bridge will pause operation.

## Troubleshooting

### No available targets are compatible with this triple.

The secp256k1 elliptic-curve dependency used for generating Vault addresses requires a newer version of [Clang](https://clang.llvm.org/).
Please download the latest available version for your distribution or check the minimum supported version in the build instructions.