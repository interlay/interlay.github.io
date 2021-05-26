# Collators

Collators are responsible for collecting parachain transactions and producing state-transition proofs. For more details checkout the [Polkadot Wiki](https://wiki.polkadot.network/docs/en/learn-collator).

Please note that in our beta testnet phase, we are actually running an independent chain based on Proof-of-Authority (PoA). This is for better control of the network whilst we collect user feedback and fix bugs. Therefore nodes are not actually collating blocks as they will be on Rococo and eventually Kusama / Polkadot. Please checkout [Polkadot's roadmap](https://polkadot.network/launch-parachains/) for futher details on when to expect live parachains.

Running a local full-node / Collator will vastly improve the reliablity of your Vault or Relayer client. Once synced, you may set `--btc-parachain-url=ws://localhost:9944` to point your client to this node.
