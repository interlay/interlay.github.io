# Installing the Vault Client

Running a Vault will allow you to hold BTC of users in custody and in return earn a return on your collateral.

To install the Vault client, follow this guide.

## Checklist

- [x] Generate Sr25519 key(s) for the parachain ([LINK](/vault/installation?id=keyfile))
- [x] Transfer collateral from the relay chain ([LINK](/guides/transfers?id=cross-chain-transfers))
- [x] Start and sync a Bitcoin full-node ([LINK](/vault/installation?id=_2-start-the-bitcoin-node))
- [x] Start the Vault client and make sure it is registered ([LINK](/vault/installation?id=_5-start-the-vault-client))
- [x] Ensure the Vault can accept issue, redeem and replace requests ([LINK](/vault/guide?id=accepting-issue-and-redeem-requests))
- [x] Verify backups are in place (Bitcoin wallet and Substrate keys) ([LINK](https://bitcoin.org/en/secure-your-wallet))
- [x] Subscribe to critical updates for continued operation ([LINK](https://discord.gg/invite/interlay))

## Prerequisites

- Recent version of Linux or MacOS. Windows support is not tested.
- At least 2 GB of RAM and a good CPU - exact requirements not yet benchmarked.
- Free disk space (ideally SSD):
  - at least **40 GB** for the Bitcoin testnet, *or*
  - at least **400 GB** for the Bitcoin mainnet.
- You should have a stable internet connection.
- Have at least 1 KINT/INTR to pay for initial transaction fees.
- Have a minimum amount of collateral assets, see [requirements](/vault/overview?id=minimum).

The Vault client should have consistent up-time, running for at least 8 hours per day.

### Keyfile

!> Ensure that you have a backup of the substrate key in a safe place.

Create a `keyfile.json` file that contains the mnemonic of the substrate account you want to use for the Vault, e.g.:

```json
{
  "0x0e5aabe5ff862d66bcba0912bf1b3d4364df0eeec0a8137704e2c16259486a71": "pizza fan clock economy angry rich enhance reveal repair figure disagree loan"
}
```

!> The mnemonic shown above is for display purposes only. DO NOT share or reuse mnemonics.

You may use [subkey](https://substrate.dev/docs/en/knowledgebase/integrate/subkey) to generate this automatically:

```shell
subkey generate --output-type json | jq '{(.accountId): .secretPhrase}' > keyfile.json
```

Please use a separate keyname and mnemonic for each client. This name determines which wallet to load on the Bitcoin full node.
If the Vault spends funds from another wallet this may be marked as theft.

### Funding

The account used to register **MUST** be endowed with collateral (at launch this is KSM for Kintsugi and DOT for Interlay) and the parachain's native token for transaction fees - KINT for Kintsugi and INTR for Interlay.

Please follow [this guide](guides/transfers?id=cross-chain-transfers) for transferring assets between chains.

Please also check the [minimum collateral requirements](/vault/overview?id=minimum) for Vaults.

## Quickstart Installation

Setup the Vault client using docker-compose. This guide is only for the testnet, please follow the [Standard Installation](vault/installation?id=standard-installation) for Kintsugi.

### 1. Install docker and docker-compose

Make sure [docker](https://docs.docker.com/engine/install/) and [docker-compose](https://docs.docker.com/compose/install/) are installed in your system.

### 2. Download the docker-compose file

```shell
mkdir vault && cd vault
wget https://raw.githubusercontent.com/interlay/interbtc-docs/master/scripts/vault/docker-compose.yml
```

### 3. Start the docker-compose process

(Optional) If you already have a locally running Bitcoin testnet node, only start the Vault client:

```shell
docker-compose up vault -d
```

?> You may need to edit the docker-compose to point `--bitcoin-rpc-url` to `http://localhost:18332`.

You can run the entire Vault client and the Bitcoin node with the following command:

```shell
docker-compose up -d
```

You can optionally view the running docker containers with command `docker-compose ps` or check the logs to see
what the containers are doing with `docker-compose logs -f vault` and `docker-compose logs -f bitcoind`.
Please take into account it can take a few hours for the bitcoin-core to sync for the first time.

## Standard Installation

Run Bitcoin and the Vault binary as a service on your computer or server. Follow this guide if you are interested in operating a Vault for earning and participating in the protocol.

!> This method is currently only supported for Linux.

### 1. Install a Bitcoin node

Download and install a [Bitcoin Core full-node](https://bitcoin.org/en/full-node#what-is-a-full-node) by following the [Linux instructions](https://bitcoin.org/en/full-node#linux-instructions).

!> Remember to backup the wallet in the [data directory](https://en.bitcoin.it/wiki/Data_directory) to preserve keys held by your Vault. Please check [this guide](https://bitcoin.org/en/secure-your-wallet) for more security best-practices.

Please note the following default ports for incoming TCP and JSON-RPC connections:

|         | P2P   | RPC   |
|---------|-------|-------|
| Regtest | 18444 | 18443 |
| Testnet | 18333 | 18332 |
| Mainnet | 8333  | 8332  |

### 2. Start the Bitcoin node

?> Synchronizing the BTC testnet requires 40GB of storage and the BTC mainnet requires 400GB. Depending on your internet connection, the download time may take anything from hours to days.

Since the Vault does not require a Bitcoin node with all the data and to reduce hardware requirements, you can start Bitcoin with the following [optimizations](https://bitcoin.org/en/full-node#what-is-a-full-node):

<!-- tabs:start -->

#### **Testnet**

```shell
bitcoind -testnet -server -rpcuser=rpcuser -rpcpassword=rpcpassword -fallbackfee=0.0002
```

#### **Kintsugi**

```shell
bitcoind -server -rpcuser=rpcuser -rpcpassword=rpcpassword -fallbackfee=0.0002
```

<!-- tabs:end -->

!> The fallback fee argument is crucial. Without it, your vault may fail to make payments in certain circumstances, which it will be punished for.

Please also note that the Vault may require additional funds to cover Bitcoin transaction fees as specified [here](/vault/guide?id=bitcoin-fees).

### 3. Install a pre-built binary

Download the asset from GitHub:

<!-- tabs:start -->

#### **Testnet**

```shell
wget -O vault https://github.com/interlay/interbtc-clients/releases/download/1.9.2/vault-parachain-metadata-testnet
```

#### **Kintsugi**

```shell
wget -O vault https://github.com/interlay/interbtc-clients/releases/download/1.9.2/vault-parachain-metadata-kintsugi
```

<!-- tabs:end -->

Make the binary executable:

```shell
chmod +x vault
```

### 4. [Optional] Install from source

Build the Vault client from source, this is necessary if we do not host builds compatible with your architecture.
Please also check the [README](https://github.com/interlay/interbtc-clients/tree/master/vault) for development instructions.

?> Building from source requires `clang 11`. Make sure to check this via `clang -v`.

#### 4.1 Install Rust

```shell
curl https://sh.rustup.rs -sSf | sh
```

#### 4.2 Build the Vault client

?> This step will take some time depending on your CPU.

Clone the Vault code, checkout the release and build the client:

```shell
git clone git@github.com:interlay/interbtc-clients.git
cd interbtc-clients
```

<!-- tabs:start -->

#### **Testnet**

```shell
git checkout 1.9.2
cargo build --bin vault --features parachain-metadata-testnet
```

#### **Kintsugi**

```shell
git checkout 1.9.2
cargo build --bin vault --features parachain-metadata-kintsugi
```

<!-- tabs:end -->

### 5. Start the Vault client

Move the vault binary into your `$PATH`.

To start the client, you can connect to our parachain full node:

<!-- tabs:start -->

#### **Testnet**

To request funds from the faucet:

```shell
vault \
  --bitcoin-rpc-url http://localhost:18332 \
  --bitcoin-rpc-user rpcuser \
  --bitcoin-rpc-pass rpcpassword \
  --keyfile keyfile.json \
  --keyname 0x0e5aabe5ff862d66bcba0912bf1b3d4364df0eeec0a8137704e2c16259486a71 \
  --collateral-currency-id=KSM \
  --auto-register-with-faucet-url 'https://api-testnet.interlay.io/faucet' \
  --btc-parachain-url 'wss://api-testnet.interlay.io:443/parachain'
```

#### **Kintsugi**

To register with 3 KSM (3000000000000 Planck):

```shell
vault \
  --bitcoin-rpc-url http://localhost:8332 \
  --bitcoin-rpc-user rpcuser \
  --bitcoin-rpc-pass rpcpassword \
  --keyfile keyfile.json \
  --keyname 0x0e5aabe5ff862d66bcba0912bf1b3d4364df0eeec0a8137704e2c16259486a71 \
  --collateral-currency-id=KSM \
  --auto-register-with-collateral 3000000000000 \
  --btc-parachain-url 'wss://api-kusama.interlay.io:443/parachain'
```

<!-- tabs:end -->

Logging can be configured using the [`RUST_LOG`](https://docs.rs/env_logger/0.8.3/env_logger/#enabling-logging) environment variable.
By default, the Vault will log at `info` or above but you may, for example, configure `debug` logs for increased verbosity.

On startup, the Vault will automatically create or load the Bitcoin wallet using the keyname specified above and import additional keys generated from issue requests.

### 6. [Optional] Start the Vault client as a systemd service

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
