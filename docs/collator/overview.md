# Collators

Collators are responsible for collecting parachain transactions and producing state-transition proofs. For more details checkout the [Polkadot Wiki](https://wiki.polkadot.network/docs/en/learn-collator). We use the [`collator-selection` pallet](https://github.com/paritytech/cumulus/blob/master/pallets/collator-selection/src/lib.rs) developed by Parity to allow candidates to register on a first-come-first-serve basis. Each candidate must deposit a `CandidacyBond` to register. We allow up to the `DesiredCandidates` count to be registered but candidates may be removed after the configured `KickThreshold` if no block is produced.

Running a local collator node will vastly improve the reliability of your Vault client. Once your collator is synced, in your vault client's config you may set `--btc-parachain-url=ws://localhost:9944` to point your vault client to your local collator node.
