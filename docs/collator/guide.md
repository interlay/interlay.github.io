# Setting up a Collator and Full-Node

Running a collator or full-node will allow you to sync and verify the integrity of the interBTC bridge.
To get started, follow this guide.

At the end of this document you will have:

- [x] Connected to a network
- [x] Synced a local full-node

?> Please note that we have not yet opened running collators to the community. Currently, it is only possible to run a full-node. We will communicate updates to this on our discord.

The following instructions have been tested on Linux.

## Prerequisites

Checkout the standard hardware requirements on the [Polkadot Wiki](https://wiki.polkadot.network/docs/en/maintain-guides-how-to-validate-polkadot#requirements).

Create a local directory to store persistent data:

```shell
mkdir -p data
```

### Optional: Snapshots

Syncing the embedded relay chain can take a number of days. [Polkashots](https://polkashots.io/) hosts database snapshots of Kusama and Polkadot which can be downloaded in minutes. Follow the relevant guide and extract the snapshot to the `data` directory.

## Quickstart

Map the directory into a local volume used by the docker container.

<!-- tabs:start -->

#### **Kintsugi**

```shell
docker run \
  --network host \
  --volume ${PWD}/data:/data \
  interlayhq/interbtc:interbtc-parachain-1-8-1 \
  interbtc-parachain \
  --base-path=/data \
  --chain=kintsugi \
  --execution=wasm \
  --wasm-execution=compiled \
  --unsafe-ws-external \
  --rpc-methods=unsafe \
  --pruning=archive \
  -- \
  --rpc-cors=all \
  --no-telemetry \
  --execution=wasm \
  --wasm-execution=compiled \
  --database=RocksDb \
  --unsafe-pruning \
  --pruning=1000
```

#### **Testnet**

```shell
docker run \
  --network host \
  --volume ${PWD}/data:/data \
  interlayhq/interbtc:interbtc-standalone-1-8-1 \
  interbtc-standalone \
  --base-path=/data \
  --chain=testnet \
  --execution=wasm \
  --wasm-execution=compiled \
  --unsafe-ws-external \
  --rpc-methods=unsafe
```

<!-- tabs:end -->

## Standard Installation

Download the pre-built binary and map the directory to the local `base-path`.

<!-- tabs:start -->

#### **Kintsugi**

```shell
wget https://github.com/interlay/interbtc/releases/download/1.9.4/interbtc-parachain
chmod +x interbtc-parachain
./interbtc-parachain \
  --base-path=${PWD}/data \
  --chain=kintsugi \
  --execution=wasm \
  --wasm-execution=compiled \
  --unsafe-ws-external \
  --rpc-methods=unsafe \
  --pruning=archive \
  -- \
  --rpc-cors=all \
  --no-telemetry \
  --execution=wasm \
  --wasm-execution=compiled \
  --database=RocksDb \
  --unsafe-pruning \
  --pruning=1000
```

#### **Testnet**

```shell
wget https://github.com/interlay/interbtc/releases/download/1.9.4/interbtc-standalone
chmod +x interbtc-standalone
./interbtc-standalone \
  --base-path=${PWD}/data \
  --chain=testnet \
  --execution=wasm \
  --wasm-execution=compiled \
  --unsafe-ws-external \
  --rpc-methods=unsafe
```

<!-- tabs:end -->

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

Clone the interBTC bridge code, checkout the appropriate release and build the node:

```shell
git clone git@github.com:interlay/interbtc.git
cd interbtc
```

### 3. Run the node

<!-- tabs:start -->

#### **Kintsugi**

```shell
git checkout 1.9.4
cargo build --release

./target/release/interbtc-parachain \
  --base-path=${PWD}/data \
  --chain=kintsugi \
  --execution=wasm \
  --wasm-execution=compiled \
  --unsafe-ws-external \
  --rpc-methods=unsafe \
  --pruning=archive \
  -- \
  --rpc-cors=all \
  --no-telemetry \
  --execution=wasm \
  --wasm-execution=compiled \
  --database=RocksDb \
  --unsafe-pruning \
  --pruning=1000
```

#### **Testnet**

```shell
git checkout 1.9.4
cargo build --release

./target/release/interbtc-standalone \
  --base-path=${PWD}/data \
  --chain=testnet \
  --execution=wasm \
  --wasm-execution=compiled \
  --unsafe-ws-external \
  --rpc-methods=unsafe
```

<!-- tabs:end -->

## Upgrading

In most cases, breaking changes to on-chain logic will be handled via the [forkless runtime upgrades](https://substrate.dev/docs/en/knowledgebase/runtime/upgrades#forkless-runtime-upgrades). However, this will not cover updates to the RPC tooling which will require that you terminate, download and restart your node.
