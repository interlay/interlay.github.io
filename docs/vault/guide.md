# Setting up a Vault

Running a Vault will allow you to hold BTC of users in custody and in return earn a return on your collateral.
To get started, follow this guide.

At the end of this document you will have:

- [x] Received testnet DOT
- [x] Started the Vault client locally
- [x] Registered your Vault on the Beta PolkaBTC testnet
- [x] Inspected the status and activities of your Vault

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
wget https://raw.githubusercontent.com/interlay/polkabtc-docs/master/scripts/vault/docker-compose.yml
```

### 2. Add your Polkadot account to use with your Vault

Add a `keyfile.json` file into that folder that contains the mnemonic of the account you want to use for the vault, e.g.:

```json
{
  "polkabtcvault": "mango inspire guess truly stone husband double exhaust reflect wood soldier steel"
}
```

!> DO NOT use the mnemonic above when running your vault. This publicly available mnemonic can be used by anyone and represents the credentials of a Polkadot account. Any funds deposited at this address will in all likelihood be lost.

You may use [subkey](https://substrate.dev/docs/en/knowledgebase/integrate/subkey) to generate this automatically:

```shell
subkey generate --output-type json | jq '{"polkabtcvault": .secretPhrase}' > keyfile.json
```

### 3. Start the Vault client

(Optional) If you already have a locally running Bitcoin testnet node, only start the vault client:

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
Run Bitcoin and the Vault binary as a service on your computer or server. Best for if you are mostly interested in operating a Vault for earning PolkaBTC and participating in the protocol.
</summary>

!> This method is currently only supported for Linux.

### 1. Install a local Bitcoin node

Download and install the Bitcoin Core full-node: [https://bitcoin.org/en/full-node](https://bitcoin.org/en/full-node#what-is-a-full-node)

### 2. Start the Bitcoin testnet node

?> Synchronizing the BTC testnet takes about 30 GB of storage and takes a couple of hours depending on your internet connection.

Since the vault does not require a Bitcoin node with all the data and to reduce hardware requirements, you can start Bitcoin with the following [optimizations](https://bitcoin.org/en/full-node#what-is-a-full-node):

```shell
bitcoind -testnet -server -par=1 -maxuploadtarget=200 -blocksonly -rpcuser=rpcuser -rpcpassword=rpcpassword
```

### 3. Install the Vault client

Create a folder for your vault and enter it:

```shell
mkdir vault && cd vault
```

Download the vault binary:

```shell
wget https://github.com/interlay/polkabtc-clients/releases/download/0.5.7/vault
```

Make the binary executable:

```shell
chmod +x vault
```

### 4. Add your Polkadot account to use with your Vault

Add a `keyfile.json` file into that folder that contains the mnemonic of the account you want to use for the vault, e.g.:

```json
{
  "polkabtcvault": "mango inspire guess truly stone husband double exhaust reflect wood soldier steel"
}
```

!> DO NOT use the mnemonic above when running your vault. This publicly available mnemonic can be used by anyone and represents the credentials of a Polkadot account. Any funds deposited at this address will in all likelihood be lost.

You may use [subkey](https://substrate.dev/docs/en/knowledgebase/integrate/subkey) to generate this automatically:

```shell
subkey generate --output-type json | jq '{"polkabtcvault": .secretPhrase}' > keyfile.json
```

### 5.A. Start the Vault client as a systemd service

?> Some of the most common Linux systems support this approach (see [systemd](https://en.wikipedia.org/wiki/Systemd)).

```shell
wget https://raw.githubusercontent.com/interlay/polkabtc-docs/master/scripts/vault/setup && wget https://raw.githubusercontent.com/interlay/polkabtc-docs/master/scripts/vault/polkabtc-vault.service
chmod +x ./setup && sudo ./setup
systemctl daemon-reload
systemctl start polkabtc-vault.service
```

You can then check the status of your service by running:

```shell
systemctl status polkabtc-vault.service
```

Or by streaming the logs to the `vault.log` file in the current directory:

```shell
journalctl --follow _SYSTEMD_UNIT=polkabtc-vault.service &> vault.log
```

To stop the service, run:

```shell
systemctl stop polkabtc-vault.service
```

### 5.B. OPTIONAL: Start the Vault client directly

?> The client will not restart on network outages or after rebooting the system using this approach.

To start the client, you can connect to our parachain full node:

```shell
RUST_LOG=info ./vault \
  --bitcoin-rpc-url http://localhost:18332 \
  --bitcoin-rpc-user rpcuser \
  --bitcoin-rpc-pass rpcpassword \
  --keyfile keyfile.json \
  --keyname polkabtcvault \
  --auto-register-with-faucet-url 'https://beta.polkabtc.io/api/faucet' \
  --polka-btc-url 'wss://beta.polkabtc.io/api/parachain' \
  --network=testnet
```

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
rustup toolchain install nightly-2021-01-25
rustup default nightly-2021-01-25
```

### 2. Install a local Bitcoin node

Download and install the Bitcoin Core full-node: [https://bitcoin.org/en/full-node](https://bitcoin.org/en/full-node#what-is-a-full-node)

### 3. Start the Bitcoin testnet node

?> Synchronizing the BTC testnet takes about 30 GB of storage and takes a couple of hours depending on your internet connection.

Since the vault does not require a Bitcoin node with all the data and to reduce hardware requirements, you can start Bitcoin with the following [optimizations](https://bitcoin.org/en/full-node#what-is-a-full-node):

```shell
bitcoind -testnet -server -par=1 -maxuploadtarget=200 -blocksonly -rpcuser=rpcuser -rpcpassword=rpcpassword
```

### 4. Build the Vault client

?> This step will take about 45 minutes depending on your CPU.

Clone the Vault code, checkout release `0.5.7`, and build the client:

```shell
git clone git@github.com:interlay/polkabtc-clients.git
cd polkabtc-clients
git checkout 0.5.7
cargo build -p vault
```

### 5. Add your Polkadot account to use with your Vault

?> You can execute this step in parallel to step 4.

Add a `keyfile.json` file into that folder that contains the mnemonic of the account you want to use for the vault, e.g.:

```json
{
  "polkabtcvault": "mango inspire guess truly stone husband double exhaust reflect wood soldier steel"
}
```

!> DO NOT use the mnemonic above when running your vault. This publicly available mnemonic can be used by anyone and represents the credentials of a Polkadot account. Any funds deposited at this address will in all likelihood be lost.

You may use [subkey](https://substrate.dev/docs/en/knowledgebase/integrate/subkey) to generate this automatically:

```shell
subkey generate --output-type json | jq '{"polkabtcvault": .secretPhrase}' > keyfile.json
```

### 6. Start the Vault client

To start the client, you can connect to our parachain full node:

```shell
RUST_LOG=info cargo run -p vault -- \
  --bitcoin-rpc-url http://localhost:18332 \
  --bitcoin-rpc-user rpcuser \
  --bitcoin-rpc-pass rpcpassword \
  --keyfile keyfile.json \
  --keyname polkabtcvault \
  --auto-register-with-faucet-url 'https://beta.polkabtc.io/api/faucet' \
  --polka-btc-url 'wss://beta.polkabtc.io/api/parachain' \
  --network=testnet
```

### For a local development setup, check the README

Go to the Vault client [README](https://github.com/interlay/polkabtc-clients/tree/master/vault).

</details>

## Usage

### Connecting the Vault to Beta

Connect to our PolkaBTC node or run your own, as described [above](#_5-start-the-vault-client).

Run the vault

```shell
RUST_LOG=info ./vault \
  --bitcoin-rpc-url http://localhost:18332 \
  --bitcoin-rpc-user rpcuser \
  --bitcoin-rpc-pass rpcpassword \
  --keyfile keyfile.json \
  --keyname polkabtcvault \
  --polka-btc-url 'wss://beta.polkabtc.io/api/parachain' \
  --auto-register-with-faucet-url 'https://beta.polkabtc.io/api/faucet' \
  --network=testnet
```

### Registering your Vault

The default behaviour on Beta is automatic registration using Interlay's DOT faucet. This happens through the `auto-register-with-faucet-url`. Another option for registering is the `auto-register-with-collateral` flag, as described in the [README](https://github.com/interlay/polkabtc-clients/tree/master/vault).

You can also register your vault through our web UI, going to the "Vault" tab, clicking the `Register` button and completing the steps.

Moreover, you can interact with the Vault client directly using [polkabtc-js](https://github.com/interlay/polkabtc-js).

```js
import { VaultClient } from "@interlay/polkabtc";
const vaultClient = new VaultClient(VAULT_CLIENT_URL);

// 100 DOT denominated in Planck
const collateralInPlanck = "1000000000000";
await vaultClient.registerVault(collateralInPlanck);
```

### Increasing Collateral

**Web UI**

Go to the Vault tab and click on button next to the `Collateral: X DOT for Y BTC` text (above the Issue Requests table). Then, follow the isntructions.

**Polkabtc-js library**

You can interact with the Vault directly client using [polkabtc-js](https://github.com/interlay/polkabtc-js).

```js
import { VaultClient } from "@interlay/polkabtc";
const vaultClient = new VaultClient(VAULT_CLIENT_URL);

// 100 DOT denominated in Planck
const additionalCollateralInPlanck = "1000000000000";
await vaultClient.lockAdditionalCollateral(additionalCollateralInPlanck);
```

### Withdrawing Collateral

**Web UI**

Go to the Vault tab and click on the button next to the `Collateral: X DOT for Y BTC` text (above the Issue Requests table). Then, follow the isntructions.

**Polkabtc-js library**

You can interact with the Vault directly client using [polkabtc-js](https://github.com/interlay/polkabtc-js).

```js
import { VaultClient } from "@interlay/polkabtc";
const vaultClient = new VaultClient(VAULT_CLIENT_URL);

// 100 DOT denominated in Planck
const collateralToWithdrawInPlanck = "1000000000000";
await vaultClient.withdrawCollateral(collateralToWithdrawInPlanck);
```

### Earning Fees

See the Fee Model described in the Overview section.

### Accepting Issue and Redeem Requests

Issue and Redeem requests are processed automatically at the moment, signing transactions on the Bitcoin and Polkadot networks using the mnemonic/account credentials you provide to the client when running it.

### Leaving PolkaBTC

The process to leave PolkaBTC depends on whether or not your Vault client holds BTC in custody.

If you Vault has _no BTC in custody_, you can withdraw all your DOT collateral at any time and leave the system. It is safe to stop the Vault client without risking being penalized. You will not participate in any issue or redeem requests once you have removed your DOT collateral.

If your Vault clients holds at least _some BTC in custody_, you have two options to leave the system. Both options require that the BTC that you have in custody is moved. Option A, leaving through _replace_, requires you to request being replaced by another vault. You can request to be replaced through the [Vault dashboard](https://beta.polkabtc.io/vault). Option B, leaving through _redeem_ requires you to wait for a user to redeem the entire amount of BTC that the Vault has in custody. Only after you have 0 BTC, can the Vault client withdraw its entire collateral.

<!-- ## Advanced

### Key Management

### Running the Vault as a Service

### Restarting the Vault

### Making Changes to the Vault -->

## Dashboard

You can monitor the operation of your Vault on the Vault dashboard.
The Vault will expose an API on the default port `3030`. If you are running the Vault on a remote server, you might need to port-forward.

Once the Vault is up and running, a "Vault" tab will appear in the topbar of the app at [beta.polkabtc.io](https://beta.polkabtc.io/) (or you can access directly at [beta.polkabtc.io/vault](https://beta.polkabtc.io/vault)).
