# Upgrading the Vault Client

The Vault client is constantly being improved. Follow this guide to get the latest version of the Vault client.

At the end of this document you will have:

- [x] Upgraded your Vault client to the latest version
- [x] Restarted the Vautl client to connect to the interBTC bridge

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

### 3. Re-start the containers

```shell
docker-compose up
```

</details>

## Standard Installation

### 1. Stop the service

```shell
sudo systemctl stop interbtc-vault.service
```

OR terminate the process with `Ctrl+C`.

### 2. Re-download the binary and setup script

```shell
wget https://github.com/interlay/interbtc-clients/releases/download/1.0.4/vault -O vault
wget https://raw.githubusercontent.com/interlay/interbtc-docs/master/scripts/vault/setup -O setup
chmod +x ./setup && sudo ./setup
```

### 3. Re-start the service

```shell
sudo systemctl start interbtc-vault.service
```

</details>

## Install from Source

Terminate the process using `Ctrl+C` and follow the instructions from the [installation](vault/installation?id=install-from-source).
