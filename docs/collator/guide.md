# Setting up a Collator

Running a Collator will allow you to sync and verify the integrity of the BTC-Parachain.
To get started, follow this guide.

At the end of this document you will have:

- [x] Connected to the beta testnet
- [x] Synced a local full-node

?> Please note that in this testnet phase, consensus validation is run via Aura.

The following instructions have been tested on Linux.

## Prerequisites

Checkout the standard hardware requirements on the [Polkadot Wiki](https://wiki.polkadot.network/docs/en/maintain-guides-how-to-validate-polkadot#requirements).
Prior to Kusama, we will review the exact requirements for a Collator node.

Download the genesis chain spec into a separate directory to also store other chain files:

```shell
wget https://raw.githubusercontent.com/interlay/polkabtc-docs/master/scripts/beta.json
mkdir btc-parachain
mv beta.json btc-parachain
```

## Quickstart

Map the directory into a local volume used by the docker container:

```shell
docker run \
  --network host \
  --volume ${PWD}/btc-parachain:/btc-parachain \
  registry.gitlab.com/interlay/btc-parachain/standalone:0.7.5 \
  btc-parachain \
  --base-path=/btc-parachain \
  --chain=/btc-parachain/beta.json \
  --unsafe-ws-external \
  --rpc-methods=Unsafe
```

## Standard Installation

Download the pre-built binary and map the directory to the local `base-path`:

```shell
wget https://github.com/interlay/btc-parachain/releases/download/0.7.5/btc-parachain-standalone
chmod +x btc-parachain-standalone
./btc-parachain-standalone \
  --base-path=${PWD}/btc-parachain \
  --chain=${PWD}/btc-parachain/beta.json \
  --unsafe-ws-external \
  --rpc-methods=Unsafe
```

## Install from Source

?> Building from source requires `clang 11`. Make sure to check this via `clang -v`.

### 1. Install Rust

We typically aim to support the latest version of nightly, check the README for the most up-to-date build instructions.

```shell
curl https://sh.rustup.rs -sSf | sh
rustup toolchain install nightly
rustup default nightly
rustup target add wasm32-unknown-unknown --toolchain nightly
```

### 2. Compile the node

?> This step will take some time depending on your hardware.

Clone the BTC-Parachain code, checkout release `0.7.5`, and build the node:

```shell
git clone git@github.com:interlay/btc-parachain.git
cd btc-parachain
git checkout 0.7.5
cargo build --release
```

### 3. Run the node

```shell
./target/release/btc-parachain \
  --base-path=${PWD}/btc-parachain \
  --chain=${PWD}/btc-parachain/beta.json \
  --unsafe-ws-external \
  --rpc-methods=Unsafe
```

## Upgrading

In most cases, breaking changes to on-chain logic will be handled via the [forkless runtime upgrades](https://substrate.dev/docs/en/knowledgebase/runtime/upgrades#forkless-runtime-upgrades). However, this will not cover updates to the RPC tooling which will require that you terminate, download and restart your node.
