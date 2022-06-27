# Installing the Vault Client

Running a Vault will allow you to hold BTC of users in custody and in return earn a return on your collateral.
Since Vault clients take funds into custody, this guide assumes that you have operated a Linux server before and understand how to securely store hot wallets.

To install the Vault client, follow this guide.

## Checklist

- [x] Generate Sr25519 key(s) for the parachain ([LINK](/vault/installation?id=keyfile))
- [x] Transfer collateral from the relay chain ([LINK](/guides/transfers?id=cross-chain-transfers))
- [x] Start and sync a Bitcoin full-node ([LINK](/vault/installation?id=_2-start-the-bitcoin-node))
- [x] Start the Vault client and make sure it is registered ([LINK](/vault/installation?id=_5-start-the-vault-client))
- [x] Ensure the Vault can accept issue, redeem and replace requests ([LINK](/vault/guide?id=accepting-issue-and-redeem-requests))
- [x] Verify backups are in place (Bitcoin wallet and Substrate keys) ([LINK](https://bitcoin.org/en/secure-your-wallet))
- [x] Subscribe to critical updates for continued operation ([LINK](https://discord.gg/invite/interlay))
- [x] Monitor the Vault client for any unusual operation ([LINK](/vault/guide?id=monitoring))
- [x] Understand the Do's and Dont's ([LINK](/vault/installation?id=dos-and-donts))

## Do's and Dont's

Operating a Vault client at the current stage involves running a fully automated client on a server.
This client has access to hot wallets for both Bitcoin and the Interlay/Kintsugi networks.
The setup below is - on purpose - technically challenging to minimize the chance that Vaults and users lose funds due to operation errors.

?> In the future, there will be an option to split custody of funds such that the Vault client is only acting as a relayer of transactions but does not have access to users funds. This aims to solve two issues: First, it should be a lot easier to open a Vault to back IBTC/KBTC form just the Dapp. Second, it opens up the possibility to have infrastructure providers and LP providers work together together to operate Vaults with the maintance from experience infrastructure providers and the capital of LP providers.

When operating a Vault client ensure the following:

1. **Do not operate two or more Vault clients with the same keyname/account at the same time.** The Vault stands the risk to execute redeem transactions twice which will be flagged as theft. The Vault will in turn lose all its collateral.
2. **Do not allow any third party access to the server operating the Vault client.** If anyone is able to access the Bitcoin or Interlay/Kintsugi wallets, the third-party is able to extract all funds. Specifically, make sure that the [RPC ports for the Bitcoin full node are not accessible via the internet](/vault/installation?id=_1-install-a-bitcoin-node).
3. **Do backup Bitcoin and Interlay/Kintsugi keys.** If the keys are not backed-up and the server operating the Vault client loses this data, the Vault stands the risk of losing all funds. There are notes for backing up the [Substrate key](/vault/installation?id=keyfile) and for backing up the [Bitcoin wallet linked in the installation below](/vault/installation?id=_1-install-a-bitcoin-node).
4. **Do monitor the Vault for potential failures.** This includes three parts: (1) keeping the collateralization level above the liquidation threshold, (2) fulfilling redeem requests on time, (3) ensuring that you have enough BTC in the Vault's wallet to fulfill redeem requests. Make sure to check out the [monitoring guides to find out how to achieve this](/vault/guide?id=monitoring).

Multiple vaults _can_ share the same bitcoin node, but only if they are each started with a unique `--keyname` argument, regardless of the network the vault runs on. That is, vaults need to use unique `--keyname` arguments even if one is running on Kintsugi while the other is running on Interlay.

## Prerequisites

- Recent version of Linux or MacOS. Windows support is not tested.
- At least 2 GB of RAM and a good CPU - exact requirements not yet benchmarked.
- Free disk space (ideally SSD):
  - at least **40 GB** for the Bitcoin testnet, *or*
  - at least **500 GB** for the Bitcoin mainnet.
- You should have a stable internet connection.
- Have at least 1 KINT/INTR to pay for initial transaction fees.
- Have a minimum amount of collateral assets, see [requirements](/vault/overview?id=minimum).

### Uptime

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

The account used to register must be endowed with collateral (at launch this is KSM for Kintsugi and DOT for Interlay) and the parachain's native token for transaction fees - KINT for Kintsugi and INTR for Interlay.

Please follow [this guide](guides/transfers?id=cross-chain-transfers) for transferring assets between chains.

Please also check the [minimum collateral requirements](/vault/overview?id=minimum) for Vaults.

## Standard Installation

Run Bitcoin and the Vault binary as a service on your computer or server. Follow this guide if you are interested in operating a Vault for earning and participating in the protocol.

?> This method is currently only supported for Linux.

### 1. Install a Bitcoin node

Download and install version 22 of the [Bitcoin Core full-node](https://bitcoin.org/en/full-node#what-is-a-full-node) by following the [Linux instructions](https://bitcoin.org/en/full-node#linux-instructions).

!> Remember to backup the wallet in the [data directory](https://en.bitcoin.it/wiki/Data_directory) to preserve keys held by your Vault. Please check [this guide](https://bitcoin.org/en/secure-your-wallet) for more security best-practices.

!> The newest supported Bitcoin Core version is 22. Version 23 uses descriptor wallets by default, which we not currently support.

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
bitcoind -testnet -server -rpcuser=rpcuser -rpcpassword=rpcpassword -walletrbf=1
```

#### **Kintsugi**

```shell
bitcoind -server -rpcuser=<INSERT_CUSTOM_USERNAME> -rpcpassword=<INSERT_YOUR_PASSWORD> -walletrbf=1
```

#### **Interlay**

```shell
bitcoind -server -rpcuser=<INSERT_CUSTOM_USERNAME> -rpcpassword=<INSERT_YOUR_PASSWORD> -walletrbf=1
```

<!-- tabs:end -->

#### Verifying Installation

Once your bitcoin node is running, you can use `nmap -p 8332 127.0.0.1` to verify that the RPC port is open.

#### Important Notes

!> Make sure that your Bitcoin RPC port is not available from the internet. You can check this by running e.g., `nmap -p 8332 your.server.ip.address` from another computer. If you expose your Bitcoin RPC port to the internet, you stand at risk of losing all funds including all collateral and BTC locked with the Vault.
You should also use a custom RPC username and password instead of the default `rpcuser` and `rpcpassword`. Just make sure to use the same username and password combination when you start your Vault and your Bitcoin node. That bitcoin username and password combination should be different than your OS login credentials.

?> Please also note that the Vault may require additional funds to cover Bitcoin transaction fees as specified [here](/vault/guide?id=bitcoin-fees).

### 3. Install a pre-built binary

Download the asset from GitHub:

<!-- tabs:start -->

#### **Testnet-Kintsugi**

```shell
wget -O vault https://github.com/interlay/interbtc-clients/releases/download/1.13.0/vault-parachain-metadata-kintsugi-testnet
```

#### **Testnet-Interlay**

```shell
wget -O vault https://github.com/interlay/interbtc-clients/releases/download/1.13.0/vault-parachain-metadata-interlay-testnet
```

#### **Kintsugi**

```shell
wget -O vault https://github.com/interlay/interbtc-clients/releases/download/1.14.0/vault-parachain-metadata-kintsugi
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

#### **Testnet-Kintsugi**

```shell
git checkout 1.13.0
cargo build --bin vault --features parachain-metadata-kintsugi-testnet
```

#### **Testnet-Interlay**

```shell
git checkout 1.13.0
cargo build --bin vault --features parachain-metadata-interlay-testnet
```

#### **Kintsugi**

```shell
git checkout 1.14.0
cargo build --bin vault --features parachain-metadata-kintsugi
```

<!-- tabs:end -->

### 5. Start the Vault client

Move the vault binary into your `$PATH`.

To start the client, you can connect to our parachain full node:

<!-- tabs:start -->

#### **Testnet-Kintsugi**

To request funds from the faucet:

```shell
vault \
  --bitcoin-rpc-url http://localhost:18332 \
  --bitcoin-rpc-user rpcuser \
  --bitcoin-rpc-pass rpcpassword \
  --keyfile keyfile.json \
  --keyname <INSERT_YOUR_KEYNAME, example: 0x0e5aabe5ff862d66bcba0912bf1b3d4364df0eeec0a8137704e2c16259486a71> \
  --faucet-url 'https://api-dev-kintsugi.interlay.io/faucet' \
  --auto-register=KSM=faucet \
  --btc-parachain-url 'wss://api-dev-kintsugi.interlay.io:443/parachain'
```

#### **Testnet-Interlay**

To request funds from the faucet:

```shell
vault \
  --bitcoin-rpc-url http://localhost:18332 \
  --bitcoin-rpc-user rpcuser \
  --bitcoin-rpc-pass rpcpassword \
  --keyfile keyfile.json \
  --keyname <INSERT_YOUR_KEYNAME, example: 0x0e5aabe5ff862d66bcba0912bf1b3d4364df0eeec0a8137704e2c16259486a71> \
  --faucet-url 'https://staging.interlay-dev.interlay.io/faucet' \
  --auto-register=DOT=faucet \
  --auto-register=INTR=faucet \
  --btc-parachain-url 'wss://staging.interlay-dev.interlay.io:443/parachain'
```

#### **Kintsugi**

To register with 3 KSM (3000000000000 Planck):

```shell
vault \
  --bitcoin-rpc-url http://localhost:8332 \
  --bitcoin-rpc-user <INSERT_CUSTOM_USERNAME> \
  --bitcoin-rpc-pass <INSERT_YOUR_PASSWORD> \
  --keyfile keyfile.json \
  --keyname <INSERT_YOUR_KEYNAME, example: 0x0e5aabe5ff862d66bcba0912bf1b3d4364df0eeec0a8137704e2c16259486a71> \
  --auto-register=KSM=3000000000000 \
  --btc-parachain-url 'wss://api-kusama.interlay.io:443/parachain'
```

<!-- tabs:end -->

Logging can be configured using the [`RUST_LOG`](https://docs.rs/env_logger/0.8.3/env_logger/#enabling-logging) environment variable.
By default, the Vault will log at `info` or above but you may, for example, configure `debug` logs for increased verbosity.

On startup, the Vault will automatically create or load the Bitcoin wallet using the keyname specified above and import additional keys generated from issue requests.

### 6. [Optional] Start the Vault client as a systemd service

?> Some of the most common Linux systems support this approach (see [systemd](https://en.wikipedia.org/wiki/Systemd)).

<!-- tabs:start -->

#### **Testnet-Kintsugi**

Download the systemd service file and a small helper script to install the service.

```shell
wget https://raw.githubusercontent.com/interlay/interbtc-docs/master/scripts/vault/setup
wget https://raw.githubusercontent.com/interlay/interbtc-docs/master/scripts/vault/testnet-vault.service
```

?> Please adjust the systemd service file to insert your substrate key into the arguments similar to step 5 above with your favorite text editor. Vim is only used as an example here.

```shell
vim testnet-vault.service
```

Install the service and start it.

```shell
chmod +x ./setup && sudo ./setup testnet
sudo systemctl daemon-reload
sudo systemctl start testnet-vault.service
```

You can also automatically start the Vault client on system reboot with:

```shell
sudo systemctl enable testnet-vault.service
```

You can then check the status of your service by running:

```shell
journalctl --follow _SYSTEMD_UNIT=testnet-vault.service
```

Or by streaming the logs to the `vault.log` file in the current directory:

```shell
journalctl --follow _SYSTEMD_UNIT=testnet-vault.service &> vault.log
```

To stop the service, run:

```shell
sudo systemctl stop testnet-vault.service
```

#### **Testnet-Interlay**

Download the systemd service file and a small helper script to install the service.

```shell
wget https://raw.githubusercontent.com/interlay/interbtc-docs/master/scripts/vault/setup
wget https://raw.githubusercontent.com/interlay/interbtc-docs/master/scripts/vault/testnet-interlay-vault.service
```

?> Please adjust the systemd service file to insert your substrate key into the arguments similar to step 5 above with your favorite text editor. Vim is only used as an example here.

```shell
vim testnet-vault.service
```

Install the service and start it.

```shell
chmod +x ./setup && sudo ./setup testnet-interlay
sudo systemctl daemon-reload
sudo systemctl start testnet-interlay-vault.service
```

You can also automatically start the Vault client on system reboot with:

```shell
sudo systemctl enable testnet-interlay-vault.service
```

You can then check the status of your service by running:

```shell
journalctl --follow _SYSTEMD_UNIT=testnet-interlay-vault.service
```

Or by streaming the logs to the `vault.log` file in the current directory:

```shell
journalctl --follow _SYSTEMD_UNIT=testnet-interlay-vault.service &> vault.log
```

To stop the service, run:

```shell
sudo systemctl stop testnet-interlay-vault.service
```

#### **Kintsugi**

Download the systemd service file and a small helper script to install the service.

```shell
wget https://raw.githubusercontent.com/interlay/interbtc-docs/master/scripts/vault/setup
wget https://raw.githubusercontent.com/interlay/interbtc-docs/master/scripts/vault/kintsugi-vault.service
```

?> Please adjust the systemd service file to insert your substrate key, the Bitcoin RPC username and password, and the initial amount of collateral you want to register the Vault with similar to step 5 above. Vim is only used as an example here.

```shell
vim kintsugi-vault.service
```

Install the service and start it.

```shell
chmod +x ./setup && sudo ./setup kintsugi
sudo systemctl daemon-reload
sudo systemctl start kintsugi-vault.service
```

You can also automatically start the Vault client on system reboot with:

```shell
sudo systemctl enable kintsugi-vault.service
```

You can then check the status of your service by running:

```shell
journalctl --follow _SYSTEMD_UNIT=kintsugi-vault.service
```

Or by streaming the logs to the `vault.log` file in the current directory:

```shell
journalctl --follow _SYSTEMD_UNIT=kintsugi-vault.service &> vault.log
```

To stop the service, run:

```shell
sudo systemctl stop kintsugi-vault.service
```

#### **Interlay**

Coming soon.

<!-- tabs:end -->
