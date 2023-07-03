# Installing the Vault Client

For Vault operators:
</br>
<a class="docs-button util-w100" href="https://forms.gle/ndrcxfF7qnnbDH4i8">
  iBTC Vault operator survey
</a>

Running a Vault will allow you to hold BTC of users in custody and in return earn a return on your collateral.
Since Vault clients take funds into custody, this guide assumes that you have operated a Linux server before and understand how to securely store hot wallets.

To install the Vault client, follow this guide.

## Checklist

- [x] Understand the Do's and Dont's ([LINK](/vault/installation?id=dos-and-donts))
- [x] Generate Sr25519 key(s) for the parachain ([LINK](/vault/installation?id=keyfile))
- [x] Transfer collateral from the relay chain ([LINK](/guides/transfers?id=cross-chain-transfers))
- [x] Start and sync a Bitcoin full-node ([LINK](/vault/installation?id=_2-start-the-bitcoin-node))
- [x] Start the Vault client and make sure it is registered ([LINK](/vault/installation?id=_5-start-the-vault-client))
- [x] Ensure the Vault can accept issue, redeem and replace requests ([LINK](/vault/guide?id=accepting-issue-and-redeem-requests))
- [x] Verify backups are in place (Bitcoin wallet and Substrate keys) ([LINK](https://bitcoin.org/en/secure-your-wallet))

## Post Installation Steps

- [x] Subscribe to critical updates for continued operation ([LINK](https://discord.gg/invite/interlay))
- [x] Head over to the operating guide to manage your Vault ([LINK](vault/guide))

## Do's and Dont's

Operating a Vault client at the current stage involves running a fully automated client on a server.
This client has access to hot wallets for both Bitcoin and the Interlay/Kintsugi networks.
The setup below is - on purpose - technically challenging to minimize the chance that Vaults and users lose funds due to operation errors.

?> In the future, there will be an option to split custody of funds such that the Vault client is only acting as a relayer of transactions but does not have access to users funds. This aims to solve two issues: First, it should be a lot easier to open a Vault to back IBTC/KBTC form just the Dapp. Second, it opens up the possibility to have infrastructure providers and LP providers work together together to operate Vaults with the maintenance from experience infrastructure providers and the capital of LP providers.

When operating a Vault client ensure the following:

1. **Do NOT operate two or more Vault clients with the same keyname/account at the same time.** The Vault stands the risk to execute redeem transactions twice which will lead to a loss of BTC - and, in the worst case, liquidation of collateral if the lost BTC is not sourced/recovered to fulfill redeem requests.
2. **Do NOT allow any third party access to the server operating the Vault client.** If anyone is able to access the Bitcoin or Interlay/Kintsugi wallets, the third-party is able to extract all funds. Specifically, make sure that the [RPC ports for the Bitcoin full node are not accessible via the internet](/vault/installation?id=_1-install-a-bitcoin-node).
3. **DO backup Bitcoin and Interlay/Kintsugi keys.** If the keys are not backed-up and the server operating the Vault client loses this data, the Vault stands the risk of losing all funds. There are notes for backing up the [Substrate key](/vault/installation?id=keyfile) and for backing up the [Bitcoin wallet linked in the installation below](/vault/installation?id=_1-install-a-bitcoin-node).
4. **DO monitor the Vault for potential failures.** This includes three parts: (1) keeping the collateralization level above the liquidation threshold, (2) fulfilling redeem requests on time, (3) ensuring that you have enough BTC in the Vault's wallet to fulfill redeem requests. Make sure to check out the [monitoring guides to find out how to achieve this](/vault/guide?id=monitoring).
5. **DO use unique `--prometheus-port` arguments when running multiple clients.** Otherwise the vault client may fail to open the port.

!> Multiple vaults _can_ share the same bitcoin node, but only if they are each started with a unique `--keyname` argument, regardless of the network the vault runs on. That is, vaults need to use unique `--keyname` arguments even if one is running on Kintsugi while the other is running on Interlay. Failing to do so can result in the Vaults spending BTC that is not theirs. This can lead to messy accounting, and in the worst case, to double payments and loss of funds.

## Prerequisites

- A recent version of Linux or MacOS. Windows support is not tested.
- At least 2 GB of RAM and a good CPU - exact requirements not yet benchmarked.
- Free disk space (ideally SSD):
  - at least **40 GB** for the Bitcoin testnet, *or*
  - at least **500 GB** for the Bitcoin mainnet, *or*
  - approximately **1GB** for either, if using a pruned Bitcoin node.
- A stable internet connection.
- At least 1 KINT/INTR to pay for initial transaction fees.
- A minimum amount of collateral assets, see [requirements](/vault/overview?id=minimum).

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

In this example, the left hand side is the public key and the right hand side is the secret mnemonic phrase.

!> The mnemonic shown above is for display purposes only. DO NOT share or reuse mnemonics.

You may use [subkey](https://docs.substrate.io/reference/command-line-tools/subkey/) to generate this automatically:

```shell
subkey generate --output-type json | jq '{(.accountId): .secretPhrase}' > keyfile.json
```

Please use a separate keyname and mnemonic for each client. This name determines which wallet to load on the Bitcoin full node. If the Vault spends funds from another wallet this may leave the Vault without BTC to cover future redeem requests.

?> You may also generate the keyfile manually using the [Polkadot extension](https://support.polkadot.network/support/solutions/articles/65000098878).

### Funding

The account used to register must be endowed with collateral (at launch this is KSM for Kintsugi and DOT for Interlay) and the parachain's native token for transaction fees - KINT for Kintsugi and INTR for Interlay.

Please follow [this guide](guides/transfers?id=cross-chain-transfers) for transferring assets between chains.

Please also check the [minimum collateral requirements](/vault/overview?id=minimum) for Vaults.

## Auto-Upgrading Installation

Run Bitcoin and the Vault auto-upgrading binary (the Runner) as a service on your computer or server. Follow this guide if you are interested in operating a Vault for earning and participating in the protocol.

The Runner runs and auto-updates Vault clients by reading on-chain release data.

?> This method is currently only supported for Linux.

### 1. Install a Bitcoin node

Download and install version 22 of the [Bitcoin Core full-node](https://bitcoin.org/en/full-node#what-is-a-full-node) by following the [Linux instructions](https://bitcoin.org/en/full-node#linux-instructions).

!> Remember to backup the wallet in the [data directory](https://en.bitcoin.it/wiki/Data_directory) to preserve keys held by your Vault. Please check [this guide](https://bitcoin.org/en/secure-your-wallet) for more security best-practices.

!> The newest supported Bitcoin Core version is 22. Version 23 uses descriptor wallets by default, which we don't currently support.

Please note the following default ports for incoming TCP and JSON-RPC connections:

|         | P2P   | RPC   |
|---------|-------|-------|
| Regtest | 18444 | 18443 |
| Testnet | 18333 | 18332 |
| Mainnet | 8333  | 8332  |

### 2. Start the Bitcoin node

?> Synchronizing the BTC testnet requires downloading 40GB of data and the BTC mainnet requires 400GB. Depending on your internet connection, the download time may take anything from hours to days.

Use the tested commands below to start the Bitcoin node. Please refer to the [Bitcoin full node guide](https://bitcoin.org/en/full-node#what-is-a-full-node) for more details on how to operate a Bitcoin node.

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

#### [Optional] Reducing Storage Usage: Running a Pruned Node

We support pruned node operation, which _significantly_ reduces disk space requirements. The parameter to the `-prune` argument is denoted in MiB, with 550 being the minimum value, meaning that the bitcoin node will use about 0.6GiB rather than 400+GiB (on mainnet).

The Bitcoin node will still need download the 400+GiB of data the first time the node is synced, but will delete any data above 550MiB. This reduces the required disk space to just 500MiB for the Bitcoin node at all times.

To support this, an external electrs server is used to query for transactions; if you do not wish to rely on this, skip this section and run a full node.

To use a pruned node, simply add the `-prune=550` argument to the bitcoin command shown above; the full command then looks like this:

<!-- tabs:start -->

#### **Testnet**

```shell
bitcoind -testnet -server -rpcuser=rpcuser -rpcpassword=rpcpassword -walletrbf=1 -prune=550
```

#### **Kintsugi**

```shell
bitcoind -server -rpcuser=<INSERT_CUSTOM_USERNAME> -rpcpassword=<INSERT_YOUR_PASSWORD> -walletrbf=1 -prune=550
```

#### **Interlay**

```shell
bitcoind -server -rpcuser=<INSERT_CUSTOM_USERNAME> -rpcpassword=<INSERT_YOUR_PASSWORD> -walletrbf=1 -prune=550
```

<!-- tabs:end -->

Restart the bitcoin node if it was already running.


#### [Optional] Servicing multiple vault clients from the same Bitcoin node

When vault clients start, they parse the bitcoin blockchain via the Bitcoin RPC API. If you are running multiple Kintsugi and/or Interlay vault clients on your computer and the computer is restarted, all of those vault clients might simultaneously start querying the bitcoin node to parse the entire blockchain state for interactions with their vault wallet accounts. This can trigger Bitcoind's `request rejected because http work queue depth exceeded` warning. Vault clients might restart since they're unable to successfully query the Bitcoin RPC endpoint and never be able to complete syncing up to the Bitcoin blockchain. A solution is to increase the depth of the `rpcworkqueue` which by default is set to 16. From anecdotal testing, if you add the following parameter values to your `bitcoin.conf` file (or provide them in your CLI call) then at least seven parallel vault clients can be serviced.
```
rpcworkqueue=128
rpcthreads=128
rpcservertimeout=220
```

If you modify these values and your bitcoin node was already running, restart the bitcoind service.

#### Important Notes

!> Make sure that your Bitcoin RPC port is not available from the internet. You can check this by running e.g., `nmap -p 8332 your.server.ip.address` from another computer. If you expose your Bitcoin RPC port to the internet, you stand at risk of losing all funds including all collateral and BTC locked with the Vault.
You should also use a custom RPC username and password instead of the default `rpcuser` and `rpcpassword`. Just make sure to use the same username and password combination when you start your Vault and your Bitcoin node. That bitcoin username and password combination should be different than your OS login credentials.

?> Please also note that the Vault may require additional funds to cover Bitcoin transaction fees as specified [here](/vault/guide?id=bitcoin-fees).

### 3. Install a pre-built binary

Download the asset from GitHub:
```shell
wget -O runner https://github.com/interlay/interbtc-clients/releases/download/1.22.1/runner
```

Make the binary executable:

```shell
chmod +x runner
```

### 4. [Optional] Install from source

Build the Runner from source, this is necessary if we do not host builds compatible with your architecture.
Please check the [README](https://github.com/interlay/interbtc-clients/tree/master/runner) for instructions.

### 5. Start the Runner

!> The Runner starts up a Vault client, so the client must not be started separately. At any given time there should only be one Vault client running for any given `AccountId`. Having multiple Vault clients running and using the same `AccountId` can lead to double payments (e.g. on redeem requests).


Move the Runner binary into your `$PATH`.

Pass Vault CLI arguments as positional arguments (preceeded by double dashes: `--`), after passing the command options of the runner executable.

<!-- tabs:start -->
#### **Testnet (Kintsugi)**

```shell
runner \
    # Runner CLI arguments
    --client-type vault \
    --parachain-ws 'wss://api-dev-kintsugi.interlay.io:443/parachain' \
    --download-path <CUSTOM_BINARY_DOWNLOAD_PATH, example: /opt/testnet/runner/> \
    -- \
    # Vault CLI arguments:
    --bitcoin-rpc-url http://localhost:18332 \
    --bitcoin-rpc-user rpcuser \
    --bitcoin-rpc-pass rpcpassword \
    --keyfile keyfile_kintsugi_testnet.json \
    --keyname <INSERT_YOUR_KEYNAME, example: 0x0e5aabe5ff862d66bcba0912bf1b3d4364df0eeec0a8137704e2c16259486a71> \
    --faucet-url 'https://api-dev-kintsugi.interlay.io/faucet' \
    --auto-register=KSM=faucet \
    --btc-parachain-url 'wss://api-dev-kintsugi.interlay.io:443/parachain'
```

#### **Testnet (Interlay)**

```shell
runner \
    # Runner CLI arguments
    --client-type vault \
    --parachain-ws 'wss://staging.interlay-dev.interlay.io:443/parachain' \
    --download-path <CUSTOM_BINARY_DOWNLOAD_PATH, example: /opt/testnet/runner/> \
    -- \
    # Vault CLI arguments:
    --bitcoin-rpc-url http://localhost:18332 \
    --bitcoin-rpc-user rpcuser \
    --bitcoin-rpc-pass rpcpassword \
    --keyfile keyfile_interlay_testnet.json \
    --keyname <INSERT_YOUR_KEYNAME, example: 0x0e5aabe5ff862d66bcba0912bf1b3d4364df0eeec0a8137704e2c16259486a71> \
    --faucet-url 'https://staging.interlay-dev.interlay.io/faucet' \
    --auto-register=DOT=faucet \
    --auto-register=INTR=faucet \
    --btc-parachain-url 'wss://staging.interlay-dev.interlay.io:443/parachain'
```

#### **Kintsugi**

```shell
runner \
    # Runner CLI arguments
    --client-type vault \
    --parachain-ws 'wss://api-kusama.interlay.io:443/parachain' \
    --download-path <CUSTOM_BINARY_DOWNLOAD_PATH, example: /opt/mainnet/runner/> \
    -- \
    # Vault CLI arguments:
    --bitcoin-rpc-url http://localhost:18332 \
    --bitcoin-rpc-user rpcuser \
    --bitcoin-rpc-pass rpcpassword \
    --keyfile keyfile_kintsugi.json \
    --keyname <INSERT_YOUR_KEYNAME, example: 0x0e5aabe5ff862d66bcba0912bf1b3d4364df0eeec0a8137704e2c16259486a71> \
    --auto-register=KSM=3000000000000 \
    --btc-parachain-url 'wss://api-kusama.interlay.io:443/parachain'
```

#### **Interlay**

```shell
runner \
    # Runner CLI arguments
    --client-type vault \
    --parachain-ws 'wss://api.interlay.io:443/parachain' \
    --download-path <CUSTOM_BINARY_DOWNLOAD_PATH, example: /opt/mainnet/runner/> \
    -- \
    # Vault CLI arguments:
    --bitcoin-rpc-url http://localhost:18332 \
    --bitcoin-rpc-user rpcuser \
    --bitcoin-rpc-pass rpcpassword \
    --keyfile keyfile_interlay.json \
    --keyname <INSERT_YOUR_KEYNAME, example: 0x0e5aabe5ff862d66bcba0912bf1b3d4364df0eeec0a8137704e2c16259486a71> \
    --auto-register=DOT=300000000000 \
    --btc-parachain-url 'wss://api.interlay.io:443/parachain'
```
<!-- tabs:end -->

#### Successful Startup

When your start your Runner client, you should see logs similar to these, from Kintsugi Testnet:

```sh
[2022-11-01T11:54:57Z INFO  jsonrpsee_client_transport::ws] Connection established to target: Target { sockaddrs: [], host: "api-dev-kintsugi.interlay.io", host_header: "api-dev-kintsugi.interlay.io:443", _mode: Tls, path_and_query: "/parachain" }
[2022-11-01T11:54:57Z INFO  runner] Connected to the parachain
[2022-11-01T11:54:57Z INFO  runner::runner] Downloading vault-parachain-metadata-kintsugi-testnet at: "./vault-parachain-metadata-kintsugi-testnet"
[2022-11-01T11:54:57Z INFO  runner::runner] Fetching executable from https://github.com/interlay/interbtc-clients/releases/download/1.17.3/vault-parachain-metadata-kintsugi-testnet
[2022-11-01T11:55:24Z INFO  runner::runner] Client started, with pid 533178
Nov 01 11:55:24.894  INFO vault: Starting Prometheus exporter at http://127.0.0.1:9615
Nov 01 11:55:24.895  INFO Server::run{addr=127.0.0.1:9615}: warp::server: listening on http://127.0.0.1:9615
Nov 01 11:55:25.173  INFO vault::process: Creating PID file at: /tmp/testnet-kintsugi_5FQuttDpfJPaNhA6BCU4e7gxfF5E1EJXUXDkxpxmZH2pTqJJ.pid
Nov 01 11:55:25.175  INFO service: Version: 1.17.3
Nov 01 11:55:25.175  INFO service: AccountId: a3dZN1MM8fUz1oim3RMBHqJqHb3pyunSJmnxCWq3PmjQNQSJG
Nov 01 11:55:25.175  INFO bitcoin: Connecting to bitcoin-core...
Nov 01 11:55:25.410  INFO bitcoin: Connected to test
Nov 01 11:55:25.410  INFO bitcoin: Bitcoin version 220000
Nov 01 11:55:25.419  INFO bitcoin: Waiting for bitcoin-core to sync...
Nov 01 11:55:25.577  INFO bitcoin: Synced!
Nov 01 11:55:25.739  INFO bitcoin: Creating wallet 0x941e21f31b24ee85f6b34c985a34e0e494b143e5a34986c82a55e73f844ceb4f-master...
Nov 01 11:55:26.709  INFO runtime::conn: Connecting to the btc-parachain...
Nov 01 11:55:26.783  INFO jsonrpsee_client_transport::ws: Connection established to target: Target { sockaddrs: [], host: "api-dev-kintsugi.interlay.io", host_header: "api-dev-kintsugi.interlay.io:443", _mode: Tls, path_and_query: "/parachain" }
Nov 01 11:55:26.783  INFO runtime::conn: Connected!
Nov 01 11:55:27.242  INFO runtime::rpc: spec_name=testnet-kintsugi
Nov 01 11:55:27.242  INFO runtime::rpc: spec_version=1019000
Nov 01 11:55:27.242  INFO runtime::rpc: transaction_version=1
Nov 01 11:55:27.263  INFO runtime::rpc: Refreshing nonce: 0
```

### 6. [Optional] Start the Runner as a systemd service

Download the systemd service file and a small helper script to install the service.

<!-- tabs:start -->
#### **Testnet (Kintsugi)**

```shell
wget https://raw.githubusercontent.com/interlay/interbtc-docs/master/scripts/vault/setup
wget https://raw.githubusercontent.com/interlay/interbtc-docs/master/scripts/runner/testnet-kintsugi-runner.service
```

?> Please adjust the systemd service file to insert your substrate key into the arguments. Vim is only used as an example here.

```shell
vim testnet-kintsugi-runner.service
```

Install the service and start it.

```shell
chmod +x ./setup && sudo ./setup testnet-kintsugi runner
sudo systemctl daemon-reload
sudo systemctl start testnet-kintsugi-runner.service
```

You can also automatically start the Runner on system reboot with:

```shell
sudo systemctl enable testnet-kintsugi-runner.service
```

You can then check the status of your service by running:

```shell
journalctl --follow _SYSTEMD_UNIT=testnet-kintsugi-runner.service
```

Or by streaming the logs to the `runner.log` file in the current directory:

```shell
journalctl --follow _SYSTEMD_UNIT=testnet-kintsugi-runner.service &> runner.log
```

To stop the service, run:

```shell
sudo systemctl stop testnet-kintsugi-runner.service
```

#### **Testnet (Interlay)**

```shell
wget https://raw.githubusercontent.com/interlay/interbtc-docs/master/scripts/vault/setup
wget https://raw.githubusercontent.com/interlay/interbtc-docs/master/scripts/runner/testnet-interlay-runner.service
```

?> Please adjust the systemd service file to insert your substrate key into the arguments. Vim is only used as an example here.

```shell
vim testnet-interlay-runner.service
```

Install the service and start it.

```shell
chmod +x ./setup && sudo ./setup testnet-interlay runner
sudo systemctl daemon-reload
sudo systemctl start testnet-interlay-runner.service
```

You can also automatically start the Runner on system reboot with:

```shell
sudo systemctl enable testnet-interlay-runner.service
```

You can then check the status of your service by running:

```shell
journalctl --follow _SYSTEMD_UNIT=testnet-interlay-runner.service
```

Or by streaming the logs to the `runner.log` file in the current directory:

```shell
journalctl --follow _SYSTEMD_UNIT=testnet-interlay-runner.service &> runner.log
```

To stop the service, run:

```shell
sudo systemctl stop testnet-interlay-runner.service
```

#### **Kintsugi**

```shell
wget https://raw.githubusercontent.com/interlay/interbtc-docs/master/scripts/vault/setup
wget https://raw.githubusercontent.com/interlay/interbtc-docs/master/scripts/runner/kintsugi-runner.service
```

?> Please adjust the systemd service file to insert your substrate key into the arguments. Vim is only used as an example here.

```shell
vim kintsugi-runner.service
```

Install the service and start it.

```shell
chmod +x ./setup && sudo ./setup kintsugi runner
sudo systemctl daemon-reload
sudo systemctl start kintsugi-runner.service
```

You can also automatically start the Runner on system reboot with:

```shell
sudo systemctl enable kintsugi-runner.service
```

You can then check the status of your service by running:

```shell
journalctl --follow _SYSTEMD_UNIT=kintsugi-runner.service
```

Or by streaming the logs to the `runner.log` file in the current directory:

```shell
journalctl --follow _SYSTEMD_UNIT=kintsugi-runner.service &> runner.log
```

To stop the service, run:

```shell
sudo systemctl stop kintsugi-runner.service
```

<!-- tabs:end -->

## Standard Installation

Run Bitcoin and the Vault binary as a service on your computer or server. Follow this guide if you are interested in operating a Vault for earning and participating in the protocol.

?> This method is currently only supported for Linux.

### 1. Install a Bitcoin Node

See the [section on installing a Bitcoin Node](/vault/installation?id=_1-install-a-bitcoin-node).

### 2. Start the Bitcoin Node

See the [section on starting the Bitcoin Node](/vault/installation?id=_2-start-the-bitcoin-node).

### 3. Install a pre-built binary

Download the asset from GitHub:

<!-- tabs:start -->

#### **Testnet (Kintsugi)**

```shell
wget -O vault https://github.com/interlay/interbtc-clients/releases/download/1.22.1/vault-parachain-metadata-kintsugi-testnet
```

#### **Testnet (Interlay)**

```shell
wget -O vault https://github.com/interlay/interbtc-clients/releases/download/1.22.1/vault-parachain-metadata-interlay-testnet
```

#### **Kintsugi**

```shell
wget -O vault https://github.com/interlay/interbtc-clients/releases/download/1.22.1/vault-parachain-metadata-kintsugi
```

#### **Interlay**

```shell
wget -O vault https://github.com/interlay/interbtc-clients/releases/download/1.22.1/vault-parachain-metadata-interlay
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

#### **Testnet (Kintsugi)**

```shell
git checkout 1.22.1
cargo build --bin vault --features parachain-metadata-kintsugi-testnet
```

#### **Testnet (Interlay)**

```shell
git checkout 1.22.1
cargo build --bin vault --features parachain-metadata-interlay-testnet
```

#### **Kintsugi**

```shell
git checkout 1.22.1
cargo build --bin vault --features parachain-metadata-kintsugi
```

#### **Interlay**

```shell
git checkout 1.22.1
cargo build --bin vault --features parachain-metadata-interlay
```

<!-- tabs:end -->

### 5. Start the Vault client

Move the vault binary into your `$PATH`.

To start the client, you can connect to our parachain full node:

<!-- tabs:start -->

#### **Testnet (Kintsugi)**

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

#### **Testnet (Interlay)**

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

#### **Interlay**

To register with 30 DOT (300000000000 Planck):

```shell
vault \
  --bitcoin-rpc-url http://localhost:8332 \
  --bitcoin-rpc-user <INSERT_CUSTOM_USERNAME> \
  --bitcoin-rpc-pass <INSERT_YOUR_PASSWORD> \
  --keyfile keyfile.json \
  --keyname <INSERT_YOUR_KEYNAME, example: 0x0e5aabe5ff862d66bcba0912bf1b3d4364df0eeec0a8137704e2c16259486a71> \
  --auto-register=DOT=300000000000 \
  --btc-parachain-url 'wss://api.interlay.io:443/parachain'
```

<!-- tabs:end -->

Logging can be configured using the [`RUST_LOG`](https://docs.rs/env_logger/0.8.3/env_logger/#enabling-logging) environment variable.
By default, the Vault will log at `info` or above but you may, for example, configure `debug` logs for increased verbosity.

On startup, the Vault will automatically create or load the Bitcoin wallet using the keyname specified above and import additional keys generated from issue requests.

?> We recommend saving the output of the logs. This will help you as the operator as well as the Interlay team to help debugging the Vault client should any errors arise. If you are unsure how to achieve this, we recommend using systemd for running the Vault client as it will provide an in-built way to store logs as described in [Step 6](#6-optional-start-the-vault-client-as-a-systemd-service).

#### Successful Startup

When your start your Vault client, you should see logs similar to this:

```sh
Sep 19 11:05:59.799  INFO vault: Starting Prometheus exporter at http://127.0.0.1:9615
Sep 19 11:05:59.799  INFO service: Version: 1.16.0
Sep 19 11:05:59.799  INFO service: AccountId: a3d....
Sep 19 11:05:59.799  INFO Server::run{addr=127.0.0.1:9615}: warp::server: listening on http://127.0.0.1:9615
Sep 19 11:05:59.799  INFO bitcoin: Connecting to bitcoin-core...
Sep 19 11:05:59.800  INFO bitcoin: Connected to main
Sep 19 11:05:59.800  INFO bitcoin: Bitcoin version 220000
Sep 19 11:05:59.807  INFO bitcoin: Waiting for bitcoin-core to sync...
Sep 19 11:05:59.808  INFO bitcoin: Synced!
Sep 19 11:05:59.808  INFO runtime::conn: Connecting to the btc-parachain...
Sep 19 11:06:00.235  INFO runtime::conn: Connected!
Sep 19 11:06:01.052  INFO runtime::rpc: spec_name=kintsugi-parachain
Sep 19 11:06:01.052  INFO runtime::rpc: spec_version=1018000
Sep 19 11:06:01.052  INFO runtime::rpc: transaction_version=3
Sep 19 11:06:01.163  INFO runtime::rpc: Refreshing nonce: 1097
Sep 19 11:06:01.496  INFO vault::system: Using 6 bitcoin confirmations
Sep 19 11:06:01.496  INFO vault::system: Subscribing to error events...
Sep 19 11:06:02.172  INFO vault::system: Adding derivation key...
Sep 19 11:06:02.428  INFO vault::system: Adding keys from past issues...
Sep 19 11:06:03.021  INFO vault::issue: Rescanning bitcoin chain from height 740163...
```

### 6. [Optional] Start the Vault client as a systemd service

?> Some of the most common Linux systems support this approach (see [systemd](https://en.wikipedia.org/wiki/Systemd)).

<!-- tabs:start -->

#### **Testnet (Kintsugi)**

Download the systemd service file and a small helper script to install the service.

```shell
wget https://raw.githubusercontent.com/interlay/interbtc-docs/master/scripts/vault/setup
wget https://raw.githubusercontent.com/interlay/interbtc-docs/master/scripts/vault/testnet-kintsugi-vault.service
```

?> Please adjust the systemd service file to insert your substrate key into the arguments similar to step 5 above with your favorite text editor. Vim is only used as an example here. [Related](https://stackoverflow.com/questions/11828270/how-do-i-exit-vim).

```shell
vim testnet-kintsugi-vault.service
```

Install the service and start it.

```shell
chmod +x ./setup && sudo ./setup testnet vault
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

#### **Testnet (Interlay)**

Download the systemd service file and a small helper script to install the service.

```shell
wget https://raw.githubusercontent.com/interlay/interbtc-docs/master/scripts/vault/setup
wget https://raw.githubusercontent.com/interlay/interbtc-docs/master/scripts/vault/testnet-interlay-vault.service
```

?> Please adjust the systemd service file to insert your substrate key into the arguments similar to step 5 above with your favorite text editor. Vim is only used as an example here. [Related](https://stackoverflow.com/questions/11828270/how-do-i-exit-vim).

```shell
vim testnet-interlay-vault.service
```

Install the service and start it.

```shell
chmod +x ./setup && sudo ./setup testnet-interlay vault
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

?> Please adjust the systemd service file to insert your substrate key into the arguments similar to step 5 above with your favorite text editor. Vim is only used as an example here. [Related](https://stackoverflow.com/questions/11828270/how-do-i-exit-vim).

```shell
vim kintsugi-vault.service
```

Install the service and start it.

```shell
chmod +x ./setup && sudo ./setup kintsugi vault
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

Download the systemd service file and a small helper script to install the service.

```shell
wget https://raw.githubusercontent.com/interlay/interbtc-docs/master/scripts/vault/setup
wget https://raw.githubusercontent.com/interlay/interbtc-docs/master/scripts/vault/interlay-vault.service
```

?> Please adjust the systemd service file to insert your substrate key into the arguments similar to step 5 above with your favorite text editor. Vim is only used as an example here. [Related](https://stackoverflow.com/questions/11828270/how-do-i-exit-vim).

```shell
vim interlay-vault.service
```

Install the service and start it.

```shell
chmod +x ./setup && sudo ./setup interlay vault
sudo systemctl daemon-reload
sudo systemctl start interlay-vault.service
```

You can also automatically start the Vault client on system reboot with:

```shell
sudo systemctl enable interlay-vault.service
```

You can then check the status of your service by running:

```shell
journalctl --follow _SYSTEMD_UNIT=interlay-vault.service
```

Or by streaming the logs to the `vault.log` file in the current directory:

```shell
journalctl --follow _SYSTEMD_UNIT=interlay-vault.service &> vault.log
```

To stop the service, run:

```shell
sudo systemctl stop interlay-vault.service
```

<!-- tabs:end -->
