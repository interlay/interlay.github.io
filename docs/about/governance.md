# Governance

On-chain governance is useful for controlling system parameters, authorizing trusted oracles and upgrading the core protocols. The interBTC parachain adopts Polkadot's architecture which allows for a **Council** and **Technical Committee** to propose referenda which are voted on by INTR holders. During the initial phase of launch Interlay will still be afforded access via the [Sudo](https://crates.parity.io/pallet_sudo/index.html) pallet, but this will relinquished over time.

The **Council** is elected by INTR holders up to a maximum capacity. Proposals are made to alter the state of the chain in some way - for example, a runtime upgrade may resolve a bug in the Bitcoin validation logic or the issue period may be extended. These proposals are then converted into referenda which are voted upon and executed if accepted. The **Technical Committee** is elected by the **Council** and may propose emergency referenda with a smaller quorum.
