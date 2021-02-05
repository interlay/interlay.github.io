# Setting up a Vault

Running a Vault will allow you to hold BTC of users in custody and in return earn a return on your collateral.
To get started, follow this guide.

At the end of this document you will have:

- [x] Received testnet DOT
- [x] Started the Vault client locally
- [x] Registered your Vault on the Rococo PolkaBTC testnet
- [x] Inspected the status and activities of your Vault

## Prerequisites

- Make sure that you have a recent version of Linux or MacOS running. Windows support is not tested.
- Make sure that you have at least 2GB of RAM.
- You should have a stable Internet connection.
- Make sure that your Vault client is running for at least 8 hours per day (you can do other things on the side).


## Quickstart

<details>
<summary>
Setup the Vault client using docker-compose. Best if you want to quickly try out running the client.
</summary>
</details>

### 1. Download the docker-compose file to start the Vault client and the Bitcoin node.

?> _TODO_ Add link to docker file.

```
mkdir vault && cd vault && wget https://github.com/interlay/polkabtc-clients/tree/master/vault
```
### 2. Add your Polkadot account to use with your Vault

Add a `keyfile.json` file into that folder that contains the mnemonic of the account you want to use for the vault, e.g.:

```json
{
    "myvault": "car timber smoke zone west involve board success norm inherit door road"
}
```

### 3. Start the Vault client


You can run the entire Vault client and the Bitcoin node with the following command:

```sh
docker-compose up
```


## Standard Installation

<details>
<summary>
Run Bitcoin and the Vault binary as a service on your computer or server. Best for if you are mostly interested in operating a Vault for earning PolkaBTC and participating in the protocol.
</summary>
</details>

### 1. Install a local Bitcoin node

Download and install the Bitcoin Core full-node: [https://bitcoin.org/en/full-node](https://bitcoin.org/en/full-node#what-is-a-full-node)

### 2. Start the Bitcoin testnet node

Since the vault does not require a Bitcoin node with all the data and to reduce hardware requirements, you can start Bitcoin with the following [optimizations](https://bitcoin.org/en/full-node#what-is-a-full-node):

```sh
bitcoind -testnet -server -prune=550 -par=1 -maxuploadtarget=200 -blocksonly -rpcuser=rpcuser -rpcpassword=rpcpassword
```

### 3. Install the Vault client

Create a folder for your vault and enter it:

```sh
mkdir vault && cd vault
```

?> _TODO_ Add the link to the binary
Download the vault binary:

```sh
wget https://gitlab.com/interlay/polkabtc-clients/-/jobs/976061249/artifacts/raw/binaries/vault
```

Make the binary executable:

```sh
chmod +x vault
```

### 4. Add your Polkadot account to use with your Vault

Add a `keyfile.json` file into that folder that contains the mnemonic of the account you want to use for the vault, e.g.:

```json
{
    "myvault": "car timber smoke zone west involve board success norm inherit door road"
}
```

### 5. Start the Vault client

To start the client, you can connect to our parachain full node or run your own.

**Run your own PolkaBTC node**
```ssh
docker run --network host registry.gitlab.com/interlay/btc-parachain:dev-rococo-c33ca08b btc-parachain-parachain --wasm-execution compiled --parachain-id 21 --chain staging --port 40337 --ws-port 9948 --bootnodes /ip4/64.225.82.241/tcp/30335/p2p/12D3KooWGMxvH5Bnmzq2LQFpdYSwe1GkqvwfrnLjhjwxmyEG1Fuk --unsafe-rpc-external --unsafe-ws-external -- --execution wasm --chain rococo --port 30337
```

Start the Vault:

```sh
./vault \
  --bitcoin-rpc-url http://localhost:18332 \
  --bitcoin-rpc-user rpcuser \
  --bitcoin-rpc-pass rpcpass \
  --keyfile keyfile.json \
  --keyname myvault \
  --polka-btc-url 'ws://0.0.0.0:9948'
```

**Connect to our PolkaBTC node**

Start the Vault:

```sh
./vault \
  --bitcoin-rpc-url http://localhost:18332 \
  --bitcoin-rpc-user rpcuser \
  --bitcoin-rpc-pass rpcpass \
  --keyfile keyfile.json \
  --keyname myvault \
  --polka-btc-url 'wss://rococo.polkabtc.io/api/parachain'
```


## Install from Source

<details>
<summary>
Build the Vault client from source. Best if you have experience compiling rust code, interested in making contributions, and see how the Vault client works under the hood.
</summary>
</details>

## Follow the instructions in the README

Go to the Vault client [README](https://github.com/interlay/polkabtc-clients/tree/master/vault).


## Usage

### Connecting the Vault to Rococo
Connect to our PolkaBTC node or run your own, as descriebd [above](#_5-start-the-vault-client).

Run the vault

```sh
./vault \
  --bitcoin-rpc-url http://localhost:18332 \
  --bitcoin-rpc-user rpcuser \
  --bitcoin-rpc-pass rpcpass \
  --keyfile keyfile.json \
  --keyname myvault \
  --polka-btc-url 'wss://rococo.polkabtc.io/api/parachain'
```

### Registering your Vault
To automatically register the vault when starting it, use the `auto-register-with-collateral` or `auto-register-with-faucet-url` flags, as described in the [README](https://github.com/interlay/polkabtc-clients/tree/master/vault).

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

Go to the Vault tab and click on button next to the `Collateral:   X DOT for Y BTC` text (above the Issue Requests table). Then, follow the isntructions.

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

Go to the Vault tab and click on the button next to the `Collateral:   X DOT for Y BTC` text (above the Issue Requests table). Then, follow the isntructions.

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
Even if a vault can withdraw all of its collateral and go offline, it cannot call a function to deregister from PolkaBTC.

## Advanced

### Key Management

### Running the Vault as a Service

### Restarting the Vault

### Making Changes to the Vault
