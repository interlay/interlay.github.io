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

Start your Bitcoin node:

```sh
bitcoind -testnet -server -rpcuser=rpcuser -rpcpassword=rpcpassword
```

Start the vault:

```sh
./vault \
  --bitcoin-rpc-url http://localhost:18332 \
  --bitcoin-rpc-user rpcuser \
  --bitcoin-rpc-pass rpcpass \
  --keyfile keyfile.json \
  --keyname myvault \
  --polka-btc-url 'wss://rococo.polkabtc.io/api/parachain'
```


## Detailed Instructions

### Bitcoin

Download the Bitcoin Core full-node: [https://bitcoin.org/en/full-node](https://bitcoin.org/en/full-node#what-is-a-full-node)

Start the Bitcoin Core node in testnet mode, keeping in mind the [hardware requirements](https://bitcoin.org/en/full-node#minimum-requirements).

```sh
bitcoind -testnet -server -rpcuser=rpcuser -rpcpassword=rpcpassword
```

If you want to reduce the hardware requirements, you can also start Bitcoin with the following [optimizations](https://bitcoin.org/en/full-node#what-is-a-full-node):

```sh
bitcoind -testnet -server -prune=550 -par=1 -maxuploadtarget=200 -blocksonly -rpcuser=rpcuser -rpcpassword=rpcpassword
```

### Vault

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

Add a `keyfile.json` file into that folder that contains the mnemonic of the account you want to use for the vault, e.g.:

```json
{
    "myvault": "car timber smoke zone west involve board success norm inherit door road"
}
```


Start the vault keeping in mind that you have set the Bitcoin environment variables:

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
