# Установка клиента Хранилища

Запуск Хранилища позволит вам держать BTC пользователей на хранении и получать доход от залога.
Чтобы установить клиент Vault, следуйте этому руководству.

В конце этого документа вы сможете:

- [x] Запустить клиент хранилища локально
- [x] Зарегистрировать свое хранилище в сети тестнет interBTC 

## Предварительные условия

- Убедитесь, что у вас установлена последняя версия Linux или MacOS. Поддержка Windows не тестировалась.
- Убедитесь, что у вас не менее 2 ГБ оперативной памяти.
- Убедитесь, что у вас есть не менее 50 ГБ свободного дискового пространства (в идеале - SSD).
- У вас должно быть стабильное подключение к Интернету.
- Убедитесь, что ваш клиент хранилища работает не менее 8 часов в день (вы можете заниматься другими делами в это время).

## Быстрый старт

<details>
<summary>
Установка клиента хранилища с помощью пакета docker-compose. Лучше всего подходит, если вы хотите быстро попробовать запустить клиент.
</summary>

### 0. Установите docker и docker-compose.

Убедитесь, что [docker](https://docs.docker.com/engine/install/ ) и [docker-compose](https://docs.docker.com/compose/install/) установлены в вашей системе.

### 1.Загрузите файл docker-compose для запуска клиента хранилища и ноды Bitcoin

```shell
mkdir vault && cd vault
wget https://raw.githubusercontent.com/interlay/interbtc-docs/master/scripts/vault/docker-compose.yml
```

### 2. Добавьте свою учетную запись Polkadot для использования с вашим хранилищем

Добавьте в эту папку файл `keyfile.json`, содержащий мнемонику учетной записи, которую вы хотите использовать для хранилища, например:

```json
{
  "interbtcvault": "mango inspire guess truly stone husband double exhaust reflect wood soldier steel"
}
```

!> Мнемоника, показанная выше, предназначена только для демонстрации. НЕ распространяйте и не используйте мнемоники повторно.

Вы можете использовать [subkey](https://substrate.dev/docs/en/knowledgebase/integrate/subkey) для автоматической генерации:

```shell
subkey generate --output-type json | jq '{"interbtcvault": .secretPhrase}' > keyfile.json
```

Пожалуйста, используйте отдельное ключевое имя и мнемонику для каждого клиента. Это имя определяет, какой кошелек будет загружен на ноде Bitcoin.
Если хранилище тратит средства из другого кошелька, это может быть отмечено как кража.

### 3. Запустите клиент хранилища

( Опционально) Если у вас уже есть локально запущенна нода Bitcoin в тестовой сети, запустите только клиент хранилища:

```shell
docker-compose up vault -d
```

?> Вам может понадобиться отредактировать docker-compose, чтобы указать `--bitcoin-rpc-url` на `http://localhost:18332`.

Вы можете запустить весь клиент хранилища и ноду Bitcoin с помощью следующей команды:

```shell
docker-compose up -d
```

Вы можете просмотреть запущенные контейнеры docker с помощью команды `docker-compose ps`.
При желании вы можете просмотреть журналы, чтобы узнать, что происходит в контейнерах, с помощью команд `docker-compose logs -f vault` и `docker-compose logs -f bitcoind`.
Обратите внимание, что для первой синхронизации биткойн-ядра может потребоваться несколько часов.

</details>

## Стандартная установка

<details>
<summary>
Запустите Bitcoin и бинарный протокол хранилища в качестве сервиса на своем компьютере или сервере. Этот вариант подходит для тех, кто заинтересован в работе хранилища для заработка InterBTC и участия в протоколе.
</summary>

!> В настоящее время этот метод поддерживается только для Linux.

### 1. Установите локальную ноду Биткойн

Скачайте и установите [Bitcoin Core full-node](https://bitcoin.org/en/full-node#what-is-a-full-node), следуя [инструкциям по Linux](https://bitcoin.org/en/full-node#linux-instructions).

!> Не забудьте сделать резервную копию кошелька в [каталог данных](https://en.bitcoin.it/wiki/Data_directory), чтобы сохранить ключи, хранящиеся в вашем хранилище.

### 2. Запустите ноду Bitcoin тестнет .

?> Синхронизация BTC тестнет занимает около 30 ГБ памяти и занимает пару часов в зависимости от вашего интернет-соединения.

Поскольку Хранилищу не требуется нода Bitcoin со всеми данными и для снижения аппаратных требований, вы можете запустить Bitcoin со следующими [оптимизациями](https://bitcoin.org/en/full-node#what-is-a-full-node):

```shell
bitcoind -testnet -server -par=1 -maxuploadtarget=200 -blocksonly -rpcuser=rpcuser -rpcpassword=rpcpassword -fallbackfee=0.0002
```

!> Аргумент о резервной комиссии имеет решающее значение. Без него ваше хранилище может не осуществить платежи в определенных обстоятельствах, за что оно будет подвергнуто штрафу.

### 3. Установите клиент хранилища Vault

Создайте папку для вашего хранилища и войдите в нее:

```shell
mkdir vault && cd vault
```

Download the vault binary:

```shell
wget https://github.com/interlay/interbtc-clients/releases/download/1.0.4/vault
```

Сделайте двоичный файл исполняемым:

```shell
chmod +x vault
```

### 4. Добавьте свою учетную запись Polkadot для использования с вашим хранилищем

Добавьте в эту папку файл `keyfile.json`, содержащий мнемонику учетной записи, которую вы хотите использовать для хранилища, например:

```json
{
  "interbtcvault": "mango inspire guess truly stone husband double exhaust reflect wood soldier steel"
}
```

!> Мнемоника, показанная выше, предназначена только для демонстрации. НЕ распространяйте и не используйте мнемоники повторно.

Вы можете использовать [subkey](https://substrate.dev/docs/en/knowledgebase/integrate/subkey) для автоматической генерации:

```shell
subkey generate --output-type json | jq '{"interbtcvault": .secretPhrase}' > keyfile.json
```

Пожалуйста, используйте разные ключевые имена и мнемоники для каждого клиента. Это имя определяет, какой кошелек будет загружен на ноде Bitcoin .
Если хранилище тратит средства из другого кошелька, это может быть отмечено как кража.

### 5.A. Запуск клиента Vault в качестве службы systemd

?> Некоторые из наиболее распространенных систем Linux поддерживают этот способ (см. [systemd](https://en.wikipedia.org/wiki/Systemd)).

```shell
wget https://raw.githubusercontent.com/interlay/interbtc-docs/master/scripts/vault/setup
wget https://raw.githubusercontent.com/interlay/interbtc-docs/master/scripts/vault/interbtc-vault.service
chmod +x ./setup && sudo ./setup
sudo systemctl daemon-reload
sudo systemctl start interbtc-vault.service
```

Затем вы можете проверить состояние вашей службы, выполнив команду:

```shell
journalctl --follow _SYSTEMD_UNIT=interbtc-vault.service
```

Или путем потоковой передачи журналов в файл `vault.log` в текущем каталоге:

```shell
journalctl --follow _SYSTEMD_UNIT=interbtc-vault.service &> vault.log
```

Чтобы остановить службу, выполните команду:

```shell
sudo systemctl stop interbtc-vault.service
```

### 5.B. ВАРИАНТ: Запуск клиента хранилища напрямую

Чтобы запустить клиент вручную, следуйте [инструкциям ниже](#_6-start-the-vault-client).

</details>

## Установка из источника

<details>
<summary>
Сборка клиента Vault из исходных данных. Лучше всего, если у вас есть опыт компиляции rust-кода, вы хотите внести свой вклад и посмотреть, как работает клиент хранилища под капотом.
</summary>

''> Сборка из исходников требует `clang 11`. Обязательно проверьте это через `clang -v`.

### 1. Установите Rust

`` shell
curl https://sh.rustup.rs -sSf | sh
```

### 2. Установите локальную ноду Bitcoin

Скачайте и установите [Bitcoin Core full-node](https://bitcoin.org/en/full-node#what-is-a-full-node), следуя инструкциям [Linux instructions](https://bitcoin.org/en/full-node#linux-instructions), [Windows instructions](https://bitcoin.org/en/full-node#windows-instructions) или [Mac OS X instructions](https://bitcoin.org/en/full-node#mac-os-x-instructions).

!> Не забудьте сделать резервную копию кошелька в [каталог данных](https://en.bitcoin.it/wiki/Data_directory), чтобы сохранить ключи, хранящиеся в вашем хранилище.

### 3. Запустите узел тестовой сети Bitcoin.

?> Синхронизация BTC тестнет занимает около 30 ГБ памяти и длится пару часов в зависимости от вашего интернет-соединения.

Поскольку хранилищу не требуется нода Bitcoin со всеми данными и для снижения аппаратных требований, вы можете запустить Bitcoin со следующими [оптимизациями](https://bitcoin.org/en/full-node#what-is-a-full-node):

`` shell
bitcoind -testnet -server -par=1 -maxuploadtarget=200 -blocksonly -rpcuser=rpcuser -rpcpassword=rpcpassword -fallbackfee=0.0002
```

!!!> Аргумент платы за откат имеет решающее значение. Без него ваше хранилище может не выполнить платежи в определенных обстоятельствах, за что оно будет наказано.

### 4. Создайте клиент хранилища

?> Этот шаг займет около 45 минут в зависимости от вашего процессора.

Клонируйте код Vault, проверьте релиз `1.0.4` и соберите клиент:

`` shell
git clone git@github.com:interlay/interbtc-clients.git
cd interbtc-clients
git checkout 1.0.4
cargo build -p vault
```

### 5. Добавьте свою учетную запись Polkadot для использования с вашим хранилищем.

Вы можете выполнить этот шаг параллельно с шагом 4.

Добавьте в эту папку файл `keyfile.json`, содержащий мнемонику учетной записи, которую вы хотите использовать для Vault, например:

``json
{
  "interbtcvault": "mango inspire guess truly stone husband double exhaust reflect wood soldier steel"
}
```

Мнемоника, показанная выше, предназначена только для демонстрации. НЕ распространяйте и не используйте мнемоники повторно.

Вы можете использовать [subkey](https://substrate.dev/docs/en/knowledgebase/integrate/subkey), чтобы сгенерировать это автоматически:

```shell
subkey generate --output-type json | jq '{"interbtcvault": .secretPhrase}' > keyfile.json
```

Пожалуйста, используйте отдельное имя ключа и мнемонику для каждого клиента. Это имя определяет, какой кошелек будет загружен на полную ноду Bitcoin.
Если хранилище тратит средства из другого кошелька, это может быть отмечено как кража.

### 6. Запуск клиента хранилища

Чтобы запустить клиент, вы можете подключиться к нашей парачейн ноде:

``shell
RUST_LOG=info cargo run -p vault -- \\
  --bitcoin-rpc-url http://localhost:18332 \00 \
  --bitcoin-rpc-user rpcuser \
  --bitcoin-rpc-pass rpcpassword \
  --keyfile keyfile.json \
  --keyname interbtcvault \
  --auto-register-with-faucet-url 'https://api.interlay.io/faucet' \
  --telemetry-url 'https://api.interlay.io/telemetry' \
  ---btc-parachain-url 'wss://api.interlay.io/parachain' \
  --network=testnet \
  --currency-id=dot
```

Ведение журнала можно настроить с помощью переменной окружения [`RUST_LOG`](https://docs.rs/env_logger/0.8.3/env_logger/#enabling-logging).
По умолчанию хранилище будет вести журнал на уровне `info` или выше, но вы можете, например, настроить журнал `debug` для увеличения его информативности.

При запуске хранилища автоматически создаст или загрузит кошелек Bitcoin, используя ключевое имя, указанное выше, и импортирует дополнительные ключи, сгенерированные из запросов на выпуск.

### Для локальной настройки разработки ознакомьтесь с файлом README.

Перейдите к клиенту хранилища [README](https://github.com/interlay/interbtc-clients/tree/master/vault).

</details>

## Дополнительно

Для дополнительной безопасности вы можете зашифровать кошелек Bitcoin паролем.

<!-- tabs:start -->

#### **Regtest**

`` shell
bitcoin-cli -regtest -rpcwallet=interbtcvault encryptwallet "password"
bitcoin-cli -regtest -rpcwallet=interbtcvault walletpassphrase "password" 100000000
```

#### **Testnet**

```shell
bitcoin-cli -testnet -rpcwallet=interbtcvault encryptwallet "password"
bitcoin-cli -testnet -rpcwallet=interbtcvault walletpassphrase "password" 100000000
```

#### **Mainnet**

```shell
bitcoin-cli -rpcwallet=interbtcvault encryptwallet "password"
bitcoin-cli -rpcwallet=interbtcvault walletpassphrase "password" 100000000
```

<!-- tabs:end -->

Это сохранит ключ расшифровки в памяти в течение указанного тайм-аута - в данном примере 100000000 секунд или 3 года.
По истечении этого тайм-аута (или если узел будет завершен) кошелек должен быть разблокирован вручную.
