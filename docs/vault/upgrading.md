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

<!-- tabs:start -->

#### **Testnet-Kintsugi**

```shell
sudo systemctl stop testnet-vault.service
```


#### **Testnet-Interlay**

```shell
sudo systemctl stop testnet-interlay-vault.service
```

#### **Kintsugi**

```shell
sudo systemctl stop kintsugi-vault.service
```

<!-- tabs:end -->


OR terminate the process with `Ctrl+C`.

### 3. Re-download the binary and setup script

<!-- tabs:start -->

#### **Testnet-Kintsugi**

```shell
wget -O vault https://github.com/interlay/interbtc-clients/releases/download/1.16.0-hotfix/vault-parachain-metadata-kintsugi-testnet

wget https://raw.githubusercontent.com/interlay/interbtc-docs/master/scripts/vault/setup -O setup
chmod +x ./setup && sudo ./setup testnet
```

#### **Testnet-Interlay**

```shell
wget -O vault https://github.com/interlay/interbtc-clients/releases/download/1.16.0-hotfix/vault-parachain-metadata-interlay-testnet

wget https://raw.githubusercontent.com/interlay/interbtc-docs/master/scripts/vault/setup -O setup
chmod +x ./setup && sudo ./setup testnet-interlay
```

#### **Kintsugi**

```shell
wget -O vault https://github.com/interlay/interbtc-clients/releases/download/1.16.0-hotfix/vault-parachain-metadata-kintsugi

wget https://raw.githubusercontent.com/interlay/interbtc-docs/master/scripts/vault/setup -O setup
chmod +x ./setup && sudo ./setup kintsugi
```

<!-- tabs:end -->

### 4. Restart the service

<!-- tabs:start -->

#### **Testnet-Kintsugi**

```shell
sudo systemctl start testnet-vault.service
```

#### **Testnet-Interlay**

```shell
sudo systemctl start testnet-interlay-vault.service
```

#### **Kintsugi**

```shell
sudo systemctl start kintsugi-vault.service
```

<!-- tabs:end -->

OR start the [process manually](vault/installation?id=_5-start-the-vault-client).
