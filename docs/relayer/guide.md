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
wget https://raw.githubusercontent.com/interlay/polkabtc-clients/0.5.0/staked-relayer/docker-compose.yml
```

### 2. Add your Polkadot account to use with your Relayer

Add a `keyfile.json` file into that folder that contains the mnemonic of the account you want to use for the relayer, e.g.:

```json
{
  "polkabtcrelayer": "mango inspire guess truly stone husband double exhaust reflect wood soldier steel"
}
```

!> DO NOT use the mnemonic above when running your vault. This publicly available mnemonic can be used by anyone and represents the credentials of a Polkadot account. Any funds deposited at this address will in all likelihood be lost.

You may use [subkey](https://substrate.dev/docs/en/knowledgebase/integrate/subkey) to generate this automatically:

```shell
subkey generate --output-type json | jq '{"polkabtcrelayer": .secretPhrase}' > keyfile.json
```

### 3. Start the Relayer client

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

### 1. Install a local Bitcoin node

Download and install the Bitcoin Core full-node: [https://bitcoin.org/en/full-node](https://bitcoin.org/en/full-node#what-is-a-full-node)

### 2. Start the Bitcoin testnet node

The Relayer requires a Bitcoin node with only part of the data. You can start Bitcoin with the following [optimizations](https://bitcoin.org/en/full-node#what-is-a-full-node):

```sh
bitcoind -testnet -server -maxuploadtarget=200 -blocksonly -rpcuser=rpcuser -rpcpassword=rpcpassword
```

### 3. Install the Relayer client

Create a folder for your relayer and enter it:

```shell
mkdir relayer && cd relayer
```

?> _TODO_ Add the link to the binary
Download the relayer binary:

```shell
wget https://gitlab.com/interlay/polkabtc-clients/-/jobs/976061249/artifacts/raw/binaries/relayer
```

Make the binary executable:

```shell
chmod +x relayer
```

### 4. Add your Polkadot account to use with your Relayer

Add a `keyfile.json` file into that folder that contains the mnemonic of the account you want to use for the relayer, e.g.:

```json
{
  "polkabtcrelayer": "mango inspire guess truly stone husband double exhaust reflect wood soldier steel"
}
```

!> DO NOT use the mnemonic above when running your vault. This publicly available mnemonic can be used by anyone and represents the credentials of a Polkadot account. Any funds deposited at this address will in all likelihood be lost.

You may use [subkey](https://substrate.dev/docs/en/knowledgebase/integrate/subkey) to generate this automatically:

```shell
subkey generate --output-type json | jq '{"polkabtcrelayer": .secretPhrase}' > keyfile.json
```

### 5. Start the Relayer client

To start the client, you can connect to our parachain full node:

```shell
./staked-relayer \
  --bitcoin-rpc-url http://localhost:18332 \
  --bitcoin-rpc-user rpcuser \
  --bitcoin-rpc-pass rpcpassword \
  --keyfile keyfile.json \
  --keyname polkabtcrelayer \
  --polka-btc-url 'wss://beta.polkabtc.io/api/parachain'
```

</details>

## Install from Source

<details>
<summary>
Build the Relayer client from source. Best if you have experience compiling rust code, interested in making contributions, and see how the Relayer client works under the hood.
</summary>

### Follow the instructions in the README

Go to the Relayer client [README](https://github.com/interlay/polkabtc-clients/tree/master/staked-relayer).

</details>

## Usage

### Connecting the Relayer to Beta

Connect to our PolkaBTC node or run your own, as described [above](#_5-optional-run-your-own-polkabtc-node).

Run the vault

```shell
./staked-relayer \
  --bitcoin-rpc-url http://localhost:18332 \
  --bitcoin-rpc-user rpcuser \
  --bitcoin-rpc-pass rpcpass \
  --keyfile keyfile.json \
  --keyname polkabtcrelayer \
  --polka-btc-url 'wss://beta.polkabtc.io/api/parachain' \
  --auto-register-with-faucet-url 'http://beta.polkabtc.io/api/faucet'
```

### Registering your Relayer

The default behaviour on Beta is automatic registration using Interlay's DOT faucet. This happens through the `auto-register-with-faucet-url`. Another option for registering is the `auto-register-with-collateral` flag, as described in the [README](https://github.com/interlay/polkabtc-clients/tree/master/vault).

You can also register your relayer through the web UI. Go to the "Relayer" tab and click on the "Register (Lock DOT)" button, following the instructions.

Moreover, you can interact with the Staked Relayer client directly using [polkabtc-js](https://github.com/interlay/polkabtc-js).

```js
import { StakedRelayerClient } from "@interlay/polkabtc";
const stakedRelayerClient = new StakedRelayerClient(VAULT_CLIENT_URL);

// 100 DOT denominated in Planck
const stakeInPlanck = 1000000000000;
await stakedRelayerClient.registerStakedRelayer(stakeInPlanck);
```

### Submitting Bitcoin Blockheaders

### Earning Fees

See the Fee Model described in the Overview section.

### Monitoring the PolkaBTC System

### Voting on the System Status

**Web UI**

Go to the Relayer tab and click on the "Vote" button. Follow the instructions.

**Polkabtc-js library**

You can interact with the Vault directly client using [polkabtc-js](https://github.com/interlay/polkabtc-js).

```js
import { VaultClient } from "@interlay/polkabtc";
const vaultClient = new VaultClient(VAULT_CLIENT_URL);

const statusUpdateId = 21;
const approve = true;
await vaultClient.voteOnStatusUpdate(statusUpdateId, approve);
```

### Leaving PolkaBTC

**Web UI**

Go to the Relayer tab and click on the "Deregister" button.

**Polkabtc-js library**

You can interact with the Vault directly client using [polkabtc-js](https://github.com/interlay/polkabtc-js).

```js
import { VaultClient } from "@interlay/polkabtc";
const vaultClient = new VaultClient(VAULT_CLIENT_URL);

await vaultClient.deregisterStakedRelayer();
```

## Advanced

### Key Management

### Running the Relayer as a Service

### Restarting the Relayer

### Making Changes to the Relayer
