# Setting up a Relayer

Running a Relayer will allow you submit BTC block headers and monitor the safety of the PolkaBTC system.
In return, you will earn a fee for your services.
To get started, follow this guide.

At the end of this document you will have:

- [x] Started the Relayer client locally
- [x] Registered your Relayer on the Rococo PolkaBTC testnet
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

?> _TODO_ Add link to docker file.

```
mkdir relayer && cd relayer && wget https://github.com/interlay/polkabtc-clients/tree/master/relayer
```
### 2. Add your Polkadot account to use with your Relayer

Add a `keyfile.json` file into that folder that contains the mnemonic of the account you want to use for the relayer, e.g.:

```json
{
    "myrelayer": "car timber smoke zone west involve board success norm inherit door road"
}
```

### 3. Start the Relayer client

?> _TODO_ Add a single command to start everything the relayer needs.

You can run the entire Relayer client and the Bitcoin node with the following command:

```sh
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

```sh
mkdir relayer && cd relayer
```

?> _TODO_ Add the link to the binary
Download the relayer binary:

```sh
wget https://gitlab.com/interlay/polkabtc-clients/-/jobs/976061249/artifacts/raw/binaries/relayer
```

Make the binary executable:

```sh
chmod +x relayer
```

### 4. Add your Polkadot account to use with your Relayer

Add a `keyfile.json` file into that folder that contains the mnemonic of the account you want to use for the relayer, e.g.:

```json
{
    "myrelayer": "car timber smoke zone west involve board success norm inherit door road"
}
```

### 5. Start the Relayer client

Start the Relayer:

```sh
./relayer \
  --bitcoin-rpc-url http://localhost:18332 \
  --bitcoin-rpc-user rpcuser \
  --bitcoin-rpc-pass rpcpass \
  --keyfile keyfile.json \
  --keyname myrelayer \
  --polka-btc-url 'wss://rococo.polkabtc.io/api/parachain'
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

### Connecting the Relayer to Rococo

### Registering your Relayer

### Submitting Bitcoin Blockheaders

### Earning Fees

### Monitoring the PolkaBTC System

### Voting on the System Status

### Leaving PolkaBTC

## Advanced

### Key Management

### Running the Relayer as a Service

### Restarting the Relayer

### Making Changes to the Relayer
