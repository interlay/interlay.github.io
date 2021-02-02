# Setting up a Vault

Running a Vault will allow you to hold BTC of users in custody and in return earn a return on your collateral.
To get started, follow this guide.

At the end of this document you will have:

- [x] Received testnet DOT
- [x] Started the Vault client locally
- [x] Registered your Vault on the Rococo PolkaBTC testnet
- [x] Inspected the status and activities of your Vault

The completion of this guide will take you approximately one to two hours.

## Quickstart

?> _TODO_ Add a single command to start everything the vault needs.

You can run the entire Vault client and the Bitcoin node with the following command:

```sh
docker-compose up
```

## Detailed Instructions

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

Download the vault binary: [INSERT LINK]

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


## Building from source

You can install the prerequisites for the vault client as well as building the client from source by following the instructions in the vault repository:

- Vault: https://github.com/interlay/polkabtc-clients/tree/master/vault

## Usage

## Advanced
