# Setting up a Collator and Full-Node

Running a collator or full-node will allow you to sync and verify the integrity of the interBTC bridge.
To get started, follow this guide.

At the end of this document you will have:

- [x] Connected to a network
- [x] Synced a local full-node
- [x] Registered as a collator

?> Please note that there are limited slots for registering as a collator.

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
  interlayhq/interbtc:1.20.0 \
  --base-path=/data \
  --chain=kintsugi \
  --execution=wasm \
  --wasm-execution=compiled \
  --unsafe-ws-external \
  --rpc-methods=unsafe \
  --pruning=archive \
  --state-cache-size=0 \
  -- \
  --rpc-cors=all \
  --no-telemetry \
  --execution=wasm \
  --wasm-execution=compiled \
  --database=rocksdb \
  --pruning=1000 \
  --state-cache-size=0
```

#### **Interlay**

```shell
docker run \
  --network host \
  --volume ${PWD}/data:/data \
  interlayhq/interbtc:1.19.1 \
  interbtc-parachain \
  --base-path=/data \
  --chain=interlay \
  --execution=wasm \
  --wasm-execution=compiled \
  --unsafe-ws-external \
  --rpc-methods=unsafe \
  --pruning=archive \
  --state-cache-size=0 \
  -- \
  --rpc-cors=all \
  --no-telemetry \
  --execution=wasm \
  --wasm-execution=compiled \
  --database=RocksDb \
  --pruning=1000 \
  --state-cache-size=0
```

<!-- tabs:end -->

## Standard Installation

### 1. Install a pre-built binary

Download the pre-built binary:

<!-- tabs:start -->

#### **Kintsugi**

```shell
wget https://github.com/interlay/interbtc/releases/download/1.20.0/interbtc-parachain
chmod +x interbtc-parachain
```

#### **Interlay**

```shell
wget https://github.com/interlay/interbtc/releases/download/1.20.0/interbtc-parachain
chmod +x interbtc-parachain
```

<!-- tabs:end -->

### 2. [Optional] Install from source

Build the parachain from source, this is necessary if we do not host builds compatible with your architecture.

#### 2.1 Install Rust

We typically aim to support the latest version of nightly, check the [README](https://github.com/interlay/interbtc/blob/master/README.md) for the most up-to-date build instructions.

```shell
curl https://sh.rustup.rs -sSf | sh
```

#### 2.2 Build the node

?> This step will take some time depending on your hardware.

Clone the parachain code, checkout the appropriate release and build the node:

```shell
git clone git@github.com:interlay/interbtc.git
cd interbtc
```

<!-- tabs:start -->

#### **Kintsugi**

```shell
git checkout 1.20.0
cargo build --release
```

#### **Interlay**

```shell
git checkout 1.19.1
cargo build --release
```

<!-- tabs:end -->

### 3. Run the node

Move the binary into your `$PATH` and run the parachain full-node:

<!-- tabs:start -->

#### **Kintsugi**

```shell
interbtc-parachain \
  --base-path=${PWD}/data \
  --chain=kintsugi \
  --execution=wasm \
  --wasm-execution=compiled \
  --unsafe-ws-external \
  --rpc-methods=unsafe \
  --pruning=archive \
  --state-cache-size=0 \
  -- \
  --rpc-cors=all \
  --no-telemetry \
  --execution=wasm \
  --wasm-execution=compiled \
  --database=rocksdb \
  --pruning=1000 \
  --state-cache-size=0
```

#### **Interlay**

```shell
interbtc-parachain \
  --base-path=${PWD}/data \
  --chain=interlay \
  --execution=wasm \
  --wasm-execution=compiled \
  --unsafe-ws-external \
  --rpc-methods=unsafe \
  --pruning=archive \
  --state-cache-size=0 \
  -- \
  --rpc-cors=all \
  --no-telemetry \
  --execution=wasm \
  --wasm-execution=compiled \
  --database=RocksDb \
  --pruning=1000 \
  --state-cache-size=0
```

<!-- tabs:end -->

## Registering

To contribute to the decentralization of the network, users may register their full-node as a collator candidate.

### 1. Stake vote-escrowed tokens

See: [Staking](/collator/overview?id=staking)

Note the account that you used to stake.

### 2. Generate a session key

Connect to your [local node](https://polkadot.js.org/apps/?rpc=ws%3A%2F%2F127.0.0.1%3A9944#/rpc) and use the `author_rotateKeys` RPC request to create new keys in your node's keystore:

![Rotate Keys](../_assets/img/collator/rotate-keys.png)

### 3. Submit the session key

Use the `setKeys` extrinsic ([example](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Fapi-kusama.interlay.io%2Fparachain#/extrinsics/decode/0x520044d46c1f308c4d0bb487bc548e210188859ac934a9863f64b47d0cbea08d2e6300)) to associate your collator node with the controller account (that you used to stake):

![Set Keys](../_assets/img/collator/set-keys.png)

In this example we used the URI `//Alice` which is a well-known testing account ONLY - do not use this in production. Generate a secure (sr25519) secret seed using a tool such as `subkey` or `polkadot-js` instead.

### 4. Register the collator

Submit the `registerAsCandidate` extrinsic ([example](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Fapi-kusama.interlay.io%2Fparachain#/extrinsics/decode/0x5103)) with the staked account to start collating:

![Register](../_assets/img/collator/register.png)

## Upgrading

In most cases, breaking changes to on-chain logic will be handled via the [forkless runtime upgrades](https://substrate.dev/docs/en/knowledgebase/runtime/upgrades#forkless-runtime-upgrades). However, this will not cover updates to the RPC tooling which will require that you terminate, download and restart your node.
