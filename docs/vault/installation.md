# Installing the Vault Client

Running a Vault will allow you to hold BTC of users in custody and in return earn a return on your collateral.
To install the Vault client, follow this guide.

At the end of this document you will have:

- [x] Started the Vault client locally
- [x] Registered your Vault on the interBTC testnet

## Prerequisites

- Make sure that you have a recent version of Linux or MacOS running. Windows support is not tested.
- Make sure that you have at least 2GB of RAM.
- Make sure you have at least 50 GB of free disk space (ideally SSD).
- You should have a stable Internet connection.
- Make sure that your Vault client is running for at least 8 hours per day (you can do other things on the side).

## Quickstart

<details>
<summary>
Setup the Vault client using docker-compose. Best if you want to quickly try out running the client.
</summary>

### 1. Download the docker-compose file to start the Vault client and the Bitcoin node

```shell
mkdir vault && cd vault
wget https://raw.githubusercontent.com/interlay/interbtc-docs/master/scripts/vault/docker-compose.yml
```

### 2. Add your Polkadot account to use with your Vault

Add a `keyfile.json` file into that folder that contains the mnemonic of the account you want to use for the Vault, e.g.:

```json
{
  "interbtcvault": "mango inspire guess truly stone husband double exhaust reflect wood soldier steel"
}
```

!> The mnemonic shown above is for display purposes only. DO NOT share or reuse menumonics.

You may use [subkey](https://substrate.dev/docs/en/knowledgebase/integrate/subkey) to generate this automatically:

```shell
subkey generate --output-type json | jq '{"interbtcvault": .secretPhrase}' > keyfile.json
```

Please use a separate keyname and mnemonic for each client. This name determines which wallet to load on the Bitcoin full node.
If the Vault spends funds from another wallet this may be marked as theft.

### 3. Start the Vault client

(Optional) If you already have a locally running Bitcoin testnet node, only start the Vault client:

```shell
docker-compose up vault
```

?> You may need to edit the docker-compose to point `--bitcoin-rpc-url` to `http://localhost:18332`.

You can run the entire Vault client and the Bitcoin node with the following command:

```shell
docker-compose up
```

</details>

## Standard Installation

<details>
<summary>
Run Bitcoin and the Vault binary as a service on your computer or server. Best for if you are mostly interested in operating a Vault for earning interBTC and participating in the protocol.
</summary>

!> This method is currently only supported for Linux.

### 1. Install a local Bitcoin node

Download and install a [Bitcoin Core full-node](https://bitcoin.org/en/full-node#what-is-a-full-node) by following the [Linux instructions](https://bitcoin.org/en/full-node#linux-instructions).

!> Remember to backup the wallet in the [data directory](https://en.bitcoin.it/wiki/Data_directory) to preserve keys held by your Vault.

### 2. Start the Bitcoin testnet node

?> Synchronizing the BTC testnet takes about 30 GB of storage and takes a couple of hours depending on your internet connection.

Since the Vault does not require a Bitcoin node with all the data and to reduce hardware requirements, you can start Bitcoin with the following [optimizations](https://bitcoin.org/en/full-node#what-is-a-full-node):

```shell
bitcoind -testnet -server -par=1 -maxuploadtarget=200 -blocksonly -rpcuser=rpcuser -rpcpassword=rpcpassword -fallbackfee=0.0002
```

!> The fallback fee argument is crucial. Without it, your vault may fail to make payments in certain circumstances, which it will be punished for.

### 3. Install the Vault client

Create a folder for your Vault and enter it:

```shell
mkdir vault && cd vault
```

Download the vault binary:

```shell
wget https://github.com/interlay/interbtc-clients/releases/download/1.0.2/vault
```

Make the binary executable:

```shell
chmod +x vault
```

### 4. Add your Polkadot account to use with your Vault

Add a `keyfile.json` file into that folder that contains the mnemonic of the account you want to use for the Vault, e.g.:

```json
{
  "interbtcvault": "mango inspire guess truly stone husband double exhaust reflect wood soldier steel"
}
```

!> The mnemonic shown above is for display purposes only. DO NOT share or reuse menumonics.

You may use [subkey](https://substrate.dev/docs/en/knowledgebase/integrate/subkey) to generate this automatically:

```shell
subkey generate --output-type json | jq '{"interbtcvault": .secretPhrase}' > keyfile.json
```

Please use a separate keyname and mnemonic for each client. This name determines which wallet to load on the Bitcoin full node.
If the Vault spends funds from another wallet this may be marked as theft.

### 5.A. Start the Vault client as a systemd service

?> Some of the most common Linux systems support this approach (see [systemd](https://en.wikipedia.org/wiki/Systemd)).

```shell
wget https://raw.githubusercontent.com/interlay/interbtc-docs/master/scripts/vault/setup
wget https://raw.githubusercontent.com/interlay/interbtc-docs/master/scripts/vault/interbtc-vault.service
chmod +x ./setup && sudo ./setup
sudo systemctl daemon-reload
sudo systemctl start interbtc-vault.service
```

You can then check the status of your service by running:

```shell
journalctl --follow _SYSTEMD_UNIT=interbtc-vault.service
```

Or by streaming the logs to the `vault.log` file in the current directory:

```shell
journalctl --follow _SYSTEMD_UNIT=interbtc-vault.service &> vault.log
```

To stop the service, run:

```shell
sudo systemctl stop interbtc-vault.service
```

### 5.B. OPTIONAL: Start the Vault client directly

To start the client manually, follow the [instructions below](#_6-start-the-vault-client).

</details>

## Install from Source

<details>
<summary>
Build the Vault client from source. Best if you have experience compiling rust code, interested in making contributions, and see how the Vault client works under the hood.
</summary>

?> Building from source requires `clang 11`. Make sure to check this via `clang -v`.

### 1. Install Rust

```shell
curl https://sh.rustup.rs -sSf | sh
```

### 2. Install a local Bitcoin node

Download and install a [Bitcoin Core full-node](https://bitcoin.org/en/full-node#what-is-a-full-node) by following the [Linux instructions](https://bitcoin.org/en/full-node#linux-instructions), [Windows instructions](https://bitcoin.org/en/full-node#windows-instructions) or [Mac OS X instructions](https://bitcoin.org/en/full-node#mac-os-x-instructions).

!> Remember to backup the wallet in the [data directory](https://en.bitcoin.it/wiki/Data_directory) to preserve keys held by your Vault.

### 3. Start the Bitcoin testnet node

?> Synchronizing the BTC testnet takes about 30 GB of storage and takes a couple of hours depending on your internet connection.

Since the Vault does not require a Bitcoin node with all the data and to reduce hardware requirements, you can start Bitcoin with the following [optimizations](https://bitcoin.org/en/full-node#what-is-a-full-node):

```shell
bitcoind -testnet -server -par=1 -maxuploadtarget=200 -blocksonly -rpcuser=rpcuser -rpcpassword=rpcpassword -fallbackfee=0.0002
```

!> The fallback fee argument is crucial. Without it, your vault may fail to make payments in certain circumstances, which it will be punished for.

### 4. Build the Vault client

?> This step will take about 45 minutes depending on your CPU.

Clone the Vault code, checkout release `1.0.2`, and build the client:

```shell
git clone git@github.com:interlay/interbtc-clients.git
cd interbtc-clients
git checkout 1.0.2
cargo build -p vault
```

### 5. Add your Polkadot account to use with your Vault

?> You can execute this step in parallel to step 4.

Add a `keyfile.json` file into that folder that contains the mnemonic of the account you want to use for the Vault, e.g.:

```json
{
  "interbtcvault": "mango inspire guess truly stone husband double exhaust reflect wood soldier steel"
}
```

!> The mnemonic shown above is for display purposes only. DO NOT share or reuse menumonics.

You may use [subkey](https://substrate.dev/docs/en/knowledgebase/integrate/subkey) to generate this automatically:

```shell
subkey generate --output-type json | jq '{"interbtcvault": .secretPhrase}' > keyfile.json
```

Please use a separate keyname and mnemonic for each client. This name determines which wallet to load on the Bitcoin full node.
If the Vault spends funds from another wallet this may be marked as theft.

### 6. Start the Vault client

To start the client, you can connect to our parachain full node:

```shell
RUST_LOG=info cargo run -p vault -- \
  --bitcoin-rpc-url http://localhost:18332 \
  --bitcoin-rpc-user rpcuser \
  --bitcoin-rpc-pass rpcpassword \
  --keyfile keyfile.json \
  --keyname interbtcvault \
  --auto-register-with-faucet-url 'https://api.interlay.io/faucet' \
  --telemetry-url 'https://api.interlay.io/telemetry' \
  --btc-parachain-url 'wss://api.interlay.io/parachain' \
  --network=testnet \
  --currency-id=dot
```

Logging can be configured using the [`RUST_LOG`](https://docs.rs/env_logger/0.8.3/env_logger/#enabling-logging) environment variable.
By default, the Vault will log at `info` or above but you may, for example, configure `debug` logs for increased verbosity.

On startup, the Vault will automatically create or load the Bitcoin wallet using the keyname specified above and import additional keys generated from issue requests.

### For a local development setup, check the README

Go to the Vault client [README](https://github.com/interlay/interbtc-clients/tree/master/vault).

</details>
