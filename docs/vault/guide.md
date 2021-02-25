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
- You should have a stable Internet connection.
- Make sure that your Vault client is running for at least 8 hours per day (you can do other things on the side).

## Quickstart

<details>
<summary>
Setup the Vault client using docker-compose. Best if you want to quickly try out running the client.
</summary>

### 1. Download the docker-compose file to start the Vault client and the Bitcoin node.

```
mkdir vault && cd vault
wget https://raw.githubusercontent.com/interlay/polkabtc-clients/0.5.1/vault/docker-compose.yml
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

You can run the entire Vault client and the Bitcoin node with the following command:

```sh
docker-compose up
```

</details>

## Standard Installation

<details>
<summary>
Run Bitcoin and the Vault binary as a service on your computer or server. Best for if you are mostly interested in operating a Vault for earning PolkaBTC and participating in the protocol.
</summary>

### 1. Install a local Bitcoin node

Download and install the Bitcoin Core full-node: [https://bitcoin.org/en/full-node](https://bitcoin.org/en/full-node#what-is-a-full-node)

### 2. Start the Bitcoin testnet node

Since the vault does not require a Bitcoin node with all the data and to reduce hardware requirements, you can start Bitcoin with the following [optimizations](https://bitcoin.org/en/full-node#what-is-a-full-node):

```shell
bitcoind -testnet -server -prune=550 -par=1 -maxuploadtarget=200 -blocksonly -rpcuser=rpcuser -rpcpassword=rpcpassword
```

### 3. Install the Vault client

Create a folder for your vault and enter it:

```shell
mkdir vault && cd vault
```

?> _TODO_ Add the link to the binary
Download the vault binary:

```shell
wget https://github.com/interlay/polkabtc-clients/releases/download/0.5.1/vault
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

### 5. Start the Vault client

To start the client, you can connect to our parachain full node:

```shell
./vault \
  --bitcoin-rpc-url http://localhost:18332 \
  --bitcoin-rpc-user rpcuser \
  --bitcoin-rpc-pass rpcpassword \
  --keyfile keyfile.json \
  --keyname polkabtcvault \
  --auto-register-with-faucet-url 'http://beta.polkabtc.io/api/faucet' \
  --polka-btc-url 'wss://beta.polkabtc.io/api/parachain'
```

</details>

## Install from Source

<details>
<summary>
Build the Vault client from source. Best if you have experience compiling rust code, interested in making contributions, and see how the Vault client works under the hood.
</summary>

### Follow the instructions in the README

Go to the Vault client [README](https://github.com/interlay/polkabtc-clients/tree/master/vault).

</details>

## Usage

### Connecting the Vault to Beta

Connect to our PolkaBTC node or run your own, as descriebd [above](#_5-start-the-vault-client).

Run the vault

```sh
./vault \
  --bitcoin-rpc-url http://localhost:18332 \
  --bitcoin-rpc-user rpcuser \
  --bitcoin-rpc-pass rpcpass \
  --keyfile keyfile.json \
  --keyname polkabtcvault \
  --auto-register-with-faucet-url 'http://beta.polkabtc.io/api/faucet' \
  --polka-btc-url 'wss://beta.polkabtc.io/api/parachain'
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

## Advanced

### Key Management

### Running the Vault as a Service

### Restarting the Vault

### Making Changes to the Vault
