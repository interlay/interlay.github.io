# Setting up a Bot

The following instructions have been tested on Linux.

## Quickstart

<details>
<summary>
Setup the Bot using docker-compose. Best if you want to quickly try it out.
</summary>

```shell
git clone https://github.com/interlay/bridge-bot
cd bridge-bot
yarn install
source .env.local
docker-compose up

# In a different terminal:
yarn live
```
</details>

## Standard Installation

<details>
<summary>
Run Bitcoin and the Bot as a service on your computer or server. Best if you intend to load test the live system (and, in the future, arbitrage).
</summary>

?> Some of the most common Linux systems support this approach (see [systemd](https://en.wikipedia.org/wiki/Systemd)).

### 1. Install a local Bitcoin node

Download and install a [Bitcoin Core full-node](https://bitcoin.org/en/full-node#what-is-a-full-node) by following the [Linux instructions](https://bitcoin.org/en/full-node#linux-instructions).

### 2. Start the Bitcoin testnet node

?> Synchronizing the BTC testnet takes about 30 GB of storage and takes a couple of hours depending on your internet connection.

The Relayer requires a Bitcoin node with only part of the data. You can start Bitcoin with the following [optimizations](https://bitcoin.org/en/full-node#what-is-a-full-node):

```shell
bitcoind -testnet -server -maxuploadtarget=200 -blocksonly -rpcuser=rpcuser -rpcpassword=rpcpassword
```


### 3. Install and start the Bot

Ensure that the current directory has a correctly configured `.env.testnet` file, using [this template](https://github.com/interlay/bridge-bot/blob/master/.env.testnet). Pay particular attention to `POLKABTC_BOT_ACCOUNT` (the Substrate mnemonic) and `BITCOIN_RPC_WALLET` (the name of the wallet to use from your Bitcoin node) - these should be dedicated (unique) to just the Bot.

```shell
wget https://raw.githubusercontent.com/interlay/polkabtc-docs/master/scripts/bridge-bot/setup
wget https://raw.githubusercontent.com/interlay/polkabtc-docs/master/scripts/bridge-bot/bridge-bot.service
chmod +x ./setup && sudo ./setup
systemctl daemon-reload
systemctl start bridge-bot.service
```

You can then check the status of your service by running:

```shell
journalctl --follow _SYSTEMD_UNIT=bridge-bot.service
```

Or by streaming the logs to the `bridge-bot.log` file in the current directory:

```shell
journalctl --follow _SYSTEMD_UNIT=polkabtc-bridge-bot &> bridge-bot.log
```

To stop the service, run:

```shell
systemctl stop bridge-bot.service
```

</details>