# Setting up a Relayer

Running a Relayer will allow you submit BTC block headers and monitor the safety of the PolkaBTC system.
In return, you will earn a fee for your services.
To get started, follow this guide.

At the end of this document you will have:

- [x] Started the Relayer client locally
- [x] Registered your Relayer on the Beta PolkaBTC testnet
- [x] Submitted BTC block headers to the PolkaBTC testnet
- [x] Inspected the status and activities of your Relayer

## Prerequisites

- Make sure that you have a recent version of Linux or MacOS running. Windows support is not tested.
- Make sure that you have at least 2GB of RAM.
- Make sure you have at least 50 GB of free disk space (ideally SSD).
- You should have a stable Internet connection.
- Make sure that your Relayer client is running for at least 8 hours per day (you can do other things on the side).

## Quickstart

<details>
<summary>
Setup the Relayer client using docker-compose. Best if you want to quickly try out running the client.
</summary>

### 1. Download the docker-compose file to start the Relayer client and the Bitcoin node.

```
mkdir relayer && cd relayer
wget https://raw.githubusercontent.com/interlay/polkabtc-docs/master/scripts/staked-relayer/docker-compose.yml
```

### 2. Add your Polkadot account to use with your Relayer

Add a `keyfile.json` file into that folder that contains the mnemonic of the account you want to use for the Relayer, e.g.:

```json
{
  "polkabtcrelayer": "mango inspire guess truly stone husband double exhaust reflect wood soldier steel"
}
```

!> The mnemonic shown above is for display purposes only. DO NOT share or reuse menumonics.

You may use [subkey](https://substrate.dev/docs/en/knowledgebase/integrate/subkey) to generate this automatically:

```shell
subkey generate --output-type json | jq '{"polkabtcrelayer": .secretPhrase}' > keyfile.json
```

Please use a separate keyname and mnemonic for each client.

### 3. Start the Relayer client

(Optional) If you already have a locally running Bitcoin testnet node, only start the Relayer client:

```shell
docker-compose up staked_relayer
```

?> You may need to edit the docker-compose to point `--bitcoin-rpc-url` to `http://localhost:18332`.

You can run the entire Relayer client and the Bitcoin node with the following command:

```shell
docker-compose up
```

</details>

## Standard Installation

<details>
<summary>
Run Bitcoin and the Relayer binary as a service on your computer or server. Best for if you are mostly interested in operating a Relayer for earning PolkaBTC and participating in the protocol.
</summary>

!> This method is currently only supported for Linux.

### 1. Install a local Bitcoin node

Download and install a [Bitcoin Core full-node](https://bitcoin.org/en/full-node#what-is-a-full-node) by following the [Linux instructions](https://bitcoin.org/en/full-node#linux-instructions).

### 2. Start the Bitcoin testnet node

?> Synchronizing the BTC testnet takes about 30 GB of storage and takes a couple of hours depending on your internet connection.

The Relayer requires a Bitcoin node with only part of the data. You can start Bitcoin with the following [optimizations](https://bitcoin.org/en/full-node#what-is-a-full-node):

```shell
bitcoind -testnet -server -maxuploadtarget=200 -blocksonly -rpcuser=rpcuser -rpcpassword=rpcpassword
```

### 3. Install the Relayer client

Create a folder for your Relayer and enter it:

```shell
mkdir relayer && cd relayer
```

?> _TODO_ Add the link to the binary
Download the Relayer binary:

```shell
wget https://github.com/interlay/polkabtc-clients/releases/download/0.7.2/staked-relayer
```

Make the binary executable:

```shell
chmod +x staked-relayer
```

### 4. Add your Polkadot account to use with your Relayer

Add a `keyfile.json` file into that folder that contains the mnemonic of the account you want to use for the Relayer, e.g.:

```json
{
  "polkabtcrelayer": "mango inspire guess truly stone husband double exhaust reflect wood soldier steel"
}
```

!> The mnemonic shown above is for display purposes only. DO NOT share or reuse menumonics.

You may use [subkey](https://substrate.dev/docs/en/knowledgebase/integrate/subkey) to generate this automatically:

```shell
subkey generate --output-type json | jq '{"polkabtcrelayer": .secretPhrase}' > keyfile.json
```

Please use a separate keyname and mnemonic for each client.

### 5.A. Start the Relayer client as a systemd service

?> Some of the most common Linux systems support this approach (see [systemd](https://en.wikipedia.org/wiki/Systemd)).

```shell
wget https://raw.githubusercontent.com/interlay/polkabtc-docs/master/scripts/staked-relayer/setup
wget https://raw.githubusercontent.com/interlay/polkabtc-docs/master/scripts/staked-relayer/polkabtc-relayer.service
chmod +x ./setup && sudo ./setup
systemctl daemon-reload
systemctl start polkabtc-relayer.service
```

You can then check the status of your service by running:

```shell
journalctl --follow _SYSTEMD_UNIT=polkabtc-relayer.service
```

Or by streaming the logs to the `relayer.log` file in the current directory:

```shell
journalctl --follow _SYSTEMD_UNIT=polkabtc-relayer.service &> relayer.log
```

To stop the service, run:

```shell
systemctl stop polkabtc-relayer.service
```

### 5.B. OPTIONAL: Start the Relayer client directly

To start the client manually, follow the [instructions below](#usage).

</details>

?> Building from source requires `clang 11`. Make sure to check this via `clang -v`.

## Install from Source

<details>
<summary>
Build the Relayer client from source. Best if you have experience compiling rust code, interested in making contributions, and see how the Relayer client works under the hood.
</summary>

### 1. Install Rust

```shell
curl https://sh.rustup.rs -sSf | sh
rustup toolchain install nightly-2021-01-25
rustup default nightly-2021-01-25
```

### 2. Install a local Bitcoin node

Download and install a [Bitcoin Core full-node](https://bitcoin.org/en/full-node#what-is-a-full-node) by following the [Linux instructions](https://bitcoin.org/en/full-node#linux-instructions), [Windows instructions](https://bitcoin.org/en/full-node#windows-instructions) or [MAX OS X instructions](https://bitcoin.org/en/full-node#mac-os-x-instructions).

### 3. Start the Bitcoin testnet node

?> Synchronizing the BTC testnet takes about 30 GB of storage and takes a couple of hours depending on your internet connection.

The Relayer requires a Bitcoin node with only part of the data. You can start Bitcoin with the following [optimizations](https://bitcoin.org/en/full-node#what-is-a-full-node):

```shell
bitcoind -testnet -server -maxuploadtarget=200 -blocksonly -rpcuser=rpcuser -rpcpassword=rpcpassword
```

### 4. Build the Relayer client

?> This step will take about 45 minutes depending on your CPU.

Clone the Relayer code, checkout release `0.7.2`, and build the client:

```shell
git clone git@github.com:interlay/polkabtc-clients.git
cd polkabtc-clients
git checkout 0.7.2
cargo build -p staked-relayer
```

### 5. Add your Polkadot account to use with your Relayer

Add a `keyfile.json` file into that folder that contains the mnemonic of the account you want to use for the Relayer, e.g.:

```json
{
  "polkabtcrelayer": "mango inspire guess truly stone husband double exhaust reflect wood soldier steel"
}
```

!> The mnemonic shown above is for display purposes only. DO NOT share or reuse menumonics.

You may use [subkey](https://substrate.dev/docs/en/knowledgebase/integrate/subkey) to generate this automatically:

```shell
subkey generate --output-type json | jq '{"polkabtcrelayer": .secretPhrase}' > keyfile.json
```

Please use a separate keyname and mnemonic for each client.

### 6. Start the Relayer client

To start the client, you can connect to our parachain full node:

```shell
RUST_LOG=info cargo run -p staked-relayer -- \
  --bitcoin-rpc-url http://localhost:18332 \
  --bitcoin-rpc-user rpcuser \
  --bitcoin-rpc-pass rpcpassword \
  --keyfile keyfile.json \
  --keyname polkabtcrelayer \
  --btc-parachain-url 'wss://beta.polkabtc.io/api/parachain' \
  --auto-fund-with-faucet-url 'https://beta.polkabtc.io/api/faucet'
```

### For a local development setup, check the README

Go to the Relayer client [README](https://github.com/interlay/polkabtc-clients/tree/master/staked-relayer).

</details>

## Upgrading

We will announce on public channels when a new release is made available for the Relayer client. The changelog and binaries will be published on the [release page](https://github.com/interlay/polkabtc-clients/releases). Depending on the method of installation;

<!-- QUICKSTART -->
<details>
<summary>
Quickstart
</summary>

### 1. Stop the containers

```shell
docker-compose down
```

### 2. Re-download the script

```shell
rm docker-compose.yaml
wget https://raw.githubusercontent.com/interlay/polkabtc-docs/master/scripts/staked-relayer/docker-compose.yml
docker-compose up
```

</details>

<!-- STANDARD -->
<details>
<summary>
Standard
</summary>

### 1. Stop the service

```shell
systemctl stop polkabtc-relayer.service
```

OR terminate the process with `Ctrl+C`.

### 2. Re-download the binary and setup script

```shell
wget https://github.com/interlay/polkabtc-clients/releases/download/0.7.2/staked-relayer-0.7.2-x86_64-linux-gnu
mv staked-relayer-0.7.2-x86_64-linux-gnu staked-relayer
wget https://raw.githubusercontent.com/interlay/polkabtc-docs/master/scripts/staked-relayer/setup
chmod +x ./setup && sudo ./setup
systemctl start polkabtc-relayer.service
```

</details>

<!-- SOURCE -->
<details>
<summary>
Source
</summary>

Terminate the process using `Ctrl+C` and follow the instructions above to re-compile.

</details>

## Usage

### Connecting the Relayer to Beta

Connect to our PolkaBTC node or run your own then start the Relayer:

```shell
RUST_LOG=info ./staked-relayer \
  --bitcoin-rpc-url http://localhost:18332 \
  --bitcoin-rpc-user rpcuser \
  --bitcoin-rpc-pass rpcpassword \
  --keyfile keyfile.json \
  --keyname polkabtcrelayer \
  --btc-parachain-url 'wss://beta.polkabtc.io/api/parachain' \
  --auto-fund-with-faucet-url 'https://beta.polkabtc.io/api/faucet'
```

Logging can be configured using the [`RUST_LOG`](https://docs.rs/env_logger/0.8.3/env_logger/#enabling-logging) environment variable.
By default, the Relayer will log at `info` or above but you may, for example, configure `debug` logs for increased verbosity.

### Registering your Relayer

The default behaviour on Beta is **automatic registration** using Interlay's DOT faucet as set in the `auto-register-with-faucet-url` arg. Another option for registering is the `auto-register-with-stake` flag, as described in the [README](https://github.com/interlay/polkabtc-clients/tree/master/staked-relayer).

You can also register your Relayer through the web UI. Go to the "Relayer" tab and click on the "Register (Lock DOT)" button, following the instructions.

Moreover, you can interact with the Relayer pallet directly using [polkabtc-js](https://github.com/interlay/polkabtc-js).

```js
import { createPolkabtcAPI } from "@interlay/polkabtc";

const polkaBTC = await createPolkabtcAPI("ws://127.0.0.1:9944", "testnet");
polkaBTC.setAccount(KEYRING);

// 100 DOT denominated in Planck
const stakeInPlanck = 1000000000000;
await polkaBTC.stakedRelayer.register(stakeInPlanck);
```

### Submitting Bitcoin Blockheaders

### Earning Fees

See the Fee Model described in the Overview section.

### Monitoring the PolkaBTC System

### Voting on the System Status

**Web UI**

Go to the Relayer tab and click on the "Vote" button. Follow the instructions.

**Polkabtc-js library**

You can use [polkabtc-js](https://github.com/interlay/polkabtc-js) to vote on a status update.

```js
import { createPolkabtcAPI } from "@interlay/polkabtc";

const polkaBTC = await createPolkabtcAPI("ws://127.0.0.1:9944", "testnet");
polkaBTC.setAccount(KEYRING);

const statusUpdateId = 21;
const approve = true;
await polkaBTC.stakedRelayer.voteOnStatusUpdate(statusUpdateId, approve);
```

### Leaving PolkaBTC

**Web UI**

Go to the Relayer tab and click on the "Deregister" button.

**Polkabtc-js library**

You can use [polkabtc-js](https://github.com/interlay/polkabtc-js) to deregister.

```js
import { createPolkabtcAPI } from "@interlay/polkabtc";

const polkaBTC = await createPolkabtcAPI("ws://127.0.0.1:9944", "testnet");
polkaBTC.setAccount(KEYRING);

await polkaBTC.stakedRelayer.deregister();
```

<!--
## Advanced

### Key Management

### Running the Relayer as a Service

### Restarting the Relayer

### Making Changes to the Relayer

-->

## Dashboard

You can monitor the operation of your Relayer on the Relayer dashboard by adding the key to the [polkadot{.js} extension](https://polkadot.js.org/extension/).

Once the Relayer is up and running, a "Relayer" tab will appear in the topbar of the app at [beta.polkabtc.io](https://beta.polkabtc.io/) (or you can access directly at [beta.polkabtc.io/relayer](https://beta.polkabtc.io/relayer)).
