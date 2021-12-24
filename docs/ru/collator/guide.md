# Настройка коллатора и полнофункциональной ноды

Запуск коллатора или полнофункциональной ноды позволит вам синхронизировать и проверять целостность моста interBTC.
Чтобы начать работу, следуйте этому руководству.

В конце этого документа вы получите:

- [x] Подключились к сети
- [x] Синхронизировали локальную полнофункциональную ноду

?> Обратите внимание, что мы еще не открыли сообществу возможность запуска коллаторов. В настоящее время можно запустить только полнофункциональную ноду. Мы будем сообщать об обновлениях на нашем discord.

Следующие инструкции были протестированы на Linux.

## Предварительные условия

Ознакомьтесь со стандартными требованиями к оборудованию на [Polkadot Wiki](https://wiki.polkadot.network/docs/en/maintain-guides-how-to-validate-polkadot#requirements).

Создайте локальный каталог для хранения постоянных данных:

```shell
mkdir -p data
```

### Дополнительно: Снепшоты

Синхронизация встроенной ретрансляционной цепочки может занять несколько дней. На сайте [Polkashots](https://polkashots.io/) размещены снепшоты баз данных Kusama и Polkadot, которые можно загрузить за несколько минут. Следуйте соответствующему руководству и извлеките снепшот в каталог `data`.

## Быстрый старт

Разместите каталог на локальном томе, используемом контейнером docker.

<!-- tabs:start -->

#### **Kintsugi**

```shell
docker run \
  --network host \
  --volume ${PWD}/data:/data \
  interlayhq/interbtc:interbtc-parachain-1-4-2 \
  interbtc-parachain \
  --base-path=/data \
  --chain=kintsugi \
  --execution=wasm \
  --wasm-execution=compiled \
  --unsafe-ws-external \
  --rpc-methods=unsafe \
  --pruning=archive \
  -- \
  --rpc-cors=all \
  --no-telemetry \
  --execution=wasm \
  --wasm-execution=compiled \
  --database=RocksDb \
  --unsafe-pruning \
  --pruning=1000
```

#### **Testnet**

```shell
docker run \
  --network host \
  --volume ${PWD}/data:/data \
  interlayhq/interbtc:interbtc-standalone-1-3-0 \
  interbtc-standalone \
  --base-path=/data \
  --chain=testnet \
  --execution=wasm \
  --wasm-execution=compiled \
  --unsafe-ws-external \
  --rpc-methods=unsafe
```

<!-- tabs:end -->

## Стандартная установка

Загрузите предварительно созданный двоичный файл и сопоставьте каталог с локальным `base-path`.

<!-- tabs:start -->

#### **Kintsugi**

```shell
wget https://github.com/interlay/interbtc/releases/download/1.4.2/interbtc-parachain
chmod +x interbtc-parachain
./interbtc-parachain \
  --base-path=${PWD}/data \
  --chain=kintsugi \
  --execution=wasm \
  --wasm-execution=compiled \
  --unsafe-ws-external \
  --rpc-methods=unsafe \
  --pruning=archive \
  -- \
  --rpc-cors=all \
  --no-telemetry \
  --execution=wasm \
  --wasm-execution=compiled \
  --database=RocksDb \
  --unsafe-pruning \
  --pruning=1000
```

#### **Testnet**

```shell
wget https://github.com/interlay/interbtc/releases/download/1.3.0/interbtc-standalone
chmod +x interbtc-standalone
./interbtc-standalone \
  --base-path=${PWD}/data \
  --chain=testnet \
  --execution=wasm \
  --wasm-execution=compiled \
  --unsafe-ws-external \
  --rpc-methods=unsafe
```

<!-- tabs:end -->

## Установка из источника

?> Сборка из исходников требует `clang 11`. Обязательно проверьте это через `clang -v`.

### 1. Установка Rust

Мы обычно стремимся поддерживать последнюю версию nightly, проверьте README для получения наиболее актуальных инструкций по сборке.

```shell
curl https://sh.rustup.rs -sSf | sh
rustup toolchain install nightly
rustup default nightly
rustup target add wasm32-unknown-unknown --toolchain nightly
```

### 2. Компиляция ноды

?> Этот шаг займет некоторое время в зависимости от вашего оборудования.

Клонируйте код моста interBTC, проверьте соответствующий релиз и соберите ноду:

```shell
git clone git@github.com:interlay/interbtc.git
cd interbtc
```

### 3. Запауск ноды

<!-- tabs:start -->

#### **Kintsugi**

```shell
git checkout 1.4.2
cargo build --release

./target/release/interbtc-parachain \
  --base-path=${PWD}/data \
  --chain=kintsugi \
  --execution=wasm \
  --wasm-execution=compiled \
  --unsafe-ws-external \
  --rpc-methods=unsafe \
  --pruning=archive \
  -- \
  --rpc-cors=all \
  --no-telemetry \
  --execution=wasm \
  --wasm-execution=compiled \
  --database=RocksDb \
  --unsafe-pruning \
  --pruning=1000
```

#### **Testnet**

```shell
git checkout 1.3.0
cargo build --release

./target/release/interbtc-standalone \
  --base-path=${PWD}/data \
  --chain=testnet \
  --execution=wasm \
  --wasm-execution=compiled \
  --unsafe-ws-external \
  --rpc-methods=unsafe
```

<!-- tabs:end -->

## Обновление

В большинстве случаев внесение изменений в логику цепочки будет осуществляться через механизм [forkless runtime upgrades](https://substrate.dev/docs/en/knowledgebase/runtime/upgrades#forkless-runtime-upgrades). Однако это не распространяется на обновления инструментария RPC, которые потребуют завершения работы, загрузки и перезапуска ноды.
