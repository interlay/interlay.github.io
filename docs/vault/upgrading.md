# Upgrading the Vault Client

The Vault client is constantly being improved. Follow this guide to get the latest version of the Vault client.

At the end of this document you will have:

- [x] Upgraded your Vault client to the latest version
- [x] Restarted the Vault client to connect to the bridge

We will announce on public channels when a new release is made available for the Vault client. The changelog and binaries will be published on the [release page](https://github.com/interlay/interbtc-clients/releases). Depending on the method of installation:

!> Occasionally, breaking changes will be introduced to the Vault client. In these cases it is important to update your Vault client in a timely manner to not run the risk of being slashed. We will notify on our channels (Discord, Twitter, Telegram) when this is the case.

## Quickstart Installation

### 1. Stop the containers

```shell
docker-compose down
```

### 2. Re-download the script

```shell
rm docker-compose.yaml
wget https://raw.githubusercontent.com/interlay/interbtc-docs/master/scripts/vault/docker-compose.yml -O docker-compose.yml
```

### 3. Restart the containers

```shell
docker-compose up
```

## Standard Installation

### 1. Stop the service

```shell
sudo systemctl stop interbtc-vault.service
```

OR terminate the process with `Ctrl+C`.

### 2. Re-download the binary and setup script

<!-- tabs:start -->

#### **Testnet**

```shell
wget -O vault https://github.com/interlay/interbtc-clients/releases/download/1.7.0/vault-parachain-metadata-testnet
```

#### **Kintsugi**

```shell
wget -O vault https://github.com/interlay/interbtc-clients/releases/download/1.7.0/vault-parachain-metadata-kintsugi
```

<!-- tabs:end -->

```shell
wget https://raw.githubusercontent.com/interlay/interbtc-docs/master/scripts/vault/setup -O setup
chmod +x ./setup && sudo ./setup
```

### 3. Update command line arguments

If upgrading from a version before 1.5.9, make sure to remove the `--network` and `--wrapped-currency-id` command line arguments. If using the service to run the vault, these are located in the `interbtc-vault.service` file

### 4. Restart the service

```shell
sudo systemctl start interbtc-vault.service
```

OR start the [process manually](vault/installation?id=_5-start-the-vault-client).
