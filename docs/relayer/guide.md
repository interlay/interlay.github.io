# Setting up a Relayer

Running a Relayer will allow you submit BTC block headers and monitor the safety of the PolkaBTC system.
In return, you will earn a fee for your services.
To get started, follow this guide.

At the end of this document you will have:

- [x] Started the Relayer client locally
- [x] Registered your Relayer on the Rococo PolkaBTC testnet
- [x] Submitted BTC block headers to the PolkaBTC testnet
- [x] Inspected the status and activities of your Relayer

The completion of this guide will take you approximately one to two hours.

## Quickstart

### Bitcoin

Download the Bitcoin Core full-node: [https://bitcoin.org/en/full-node](https://bitcoin.org/en/full-node#what-is-a-full-node)

Start the Bitcoin Core node in testnet mode, keeping in mind the [hardware requirements](https://bitcoin.org/en/full-node#minimum-requirements).

```sh
bitcoind -testnet -server
```

### Relayer

Download the relayer binary: [INSERT LINK]

```sh
wget https://gitlab.com/interlay/polkabtc-clients/-/jobs/976061249/artifacts/raw/binaries/relayer
```

Make the binary executable:

```sh
chmod +x relayer
```

Start the relayer:

```sh
./relayer --keyfile --keyname vault
```


## Building from source

You can install the prerequisites for the relayer client as well as building the client from source by following the instructions in the vault repository:

- Relayer: https://github.com/interlay/polkabtc-clients/tree/master/relayer

## Usage

## Advanced
