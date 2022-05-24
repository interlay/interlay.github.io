# Upgrading the Vault Client

The Vault client is constantly being improved. Follow this guide to get the latest version of the Vault client.

At the end of this document you will have:

- [x] Upgraded your Vault client to the latest version
- [x] Restarted the Vault client to connect to the bridge

We will announce on public channels when a new release is made available for the Vault client. The changelog and binaries will be published on the [release page](https://github.com/interlay/interbtc-clients/releases). Depending on the method of installation:

!> Occasionally, breaking changes will be introduced to the Vault client. In these cases it is important to update your Vault client in a timely manner to not run the risk of being slashed. We will notify on our channels (Discord, Twitter, Telegram) when this is the case.

## Important Notes [`1.11.0`](https://github.com/interlay/interbtc-clients/releases/tag/1.11.0)

- All transactions are now made with [Opt-in RBF](https://bitcoincore.org/en/faq/optin_rbf/)
- The CLI changed to support registration with multi-collateral:
    - Remove `--collateral-currency-id`
    - Replace `--auto-register-with-faucet-url=$FAUCET_URL` with `--faucet-url=$FAUCET_URL` and `--auto-register=KSM=faucet`
    - Replace `--auto-register-with-collateral=$AMOUNT` with `--auto-register=KSM=$AMOUNT`

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

### 1. Migrate to the new file path and name

Some older versions of the docs used different paths and service names. If you were using interbtc-vault.service for testnet or kintsugi, you need to run the following:

<!-- tabs:start -->

#### **Testnet**

```shell
# rename the service file
mv /usr/lib/systemd/system/interbtc-vault.service /usr/lib/systemd/system/testnet-vault.service

# replace the /opt/interbtc/ path in the service by /opt/testnet/
sed -i 's+/opt/interbtc/+/opt/testnet/+g' /usr/lib/systemd/system/testnet-vault.service
```

#### **Kintsugi**

```shell
# rename the service file
mv /usr/lib/systemd/system/interbtc-vault.service /usr/lib/systemd/system/kintsugi-vault.service

# replace the /opt/interbtc/ path in the service by /opt/kintsugi/
sed -i 's+/opt/interbtc/+/opt/kintsugi/+g' /usr/lib/systemd/system/kintsugi-vault.service
```

<!-- tabs:end -->

### 2. Stop the service

<!-- tabs:start -->

#### **Testnet**

```shell
sudo systemctl stop testnet-vault.service
```

#### **Kintsugi**

```shell
sudo systemctl stop kintsugi-vault.service
```

<!-- tabs:end -->


OR terminate the process with `Ctrl+C`.

### 3. Re-download the binary and setup script

<!-- tabs:start -->

#### **Testnet**

```shell
wget -O vault https://github.com/interlay/interbtc-clients/releases/download/1.11.2/vault-parachain-metadata-testnet

wget https://raw.githubusercontent.com/interlay/interbtc-docs/master/scripts/vault/setup -O setup
chmod +x ./setup && sudo ./setup testnet
```

#### **Kintsugi**

```shell
wget -O vault https://github.com/interlay/interbtc-clients/releases/download/1.11.2/vault-parachain-metadata-kintsugi

wget https://raw.githubusercontent.com/interlay/interbtc-docs/master/scripts/vault/setup -O setup
chmod +x ./setup && sudo ./setup kintsugi
```

<!-- tabs:end -->

### 4. Restart the service

<!-- tabs:start -->

#### **Testnet**

```shell
sudo systemctl start testnet-vault.service
```

#### **Kintsugi**

```shell
sudo systemctl start kintsugi-vault.service
```

<!-- tabs:end -->

OR start the [process manually](vault/installation?id=_5-start-the-vault-client).
