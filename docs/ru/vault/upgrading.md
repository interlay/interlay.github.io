# Обновление клиента хранилища

Клиент хранилища постоянно совершенствуется. Следуйте этому руководству, чтобы получить последнюю версию клиента хранилища.

В конце этого документа вы получите:

- [x] Обновление клиента хранилища до последней версии
- [x] Перезапуск клиента Хранилища для подключения к мосту interBTC.

Мы будем объявлять в публичных каналах, когда будет выпущен новый релиз для клиента хранилища. Журнал изменений и двоичные файлы будут опубликованы на [странице релиза](https://github.com/interlay/interbtc-clients/releases). В зависимости от метода установки:

!> Время от времени в клиент хранилища будут вноситься серьезные изменения. В этих случаях важно своевременно обновить клиент хранилища, чтобы не подвергаться риску взлома. Мы будем уведомлять об этом на наших каналах (Discord, Twitter, Telegram).

## Быстрый старт установки

### 1. Остановите контейнеры

`` shell
docker-compose down
```

### 2. Повторно загрузите скрипт

```shell
rm docker-compose.yaml
wget https://raw.githubusercontent.com/interlay/interbtc-docs/master/scripts/vault/docker-compose.yml -O docker-compose.yml
```

### 3. Перезапустите контейнеры

```shell
docker-compose up
```

</details>

## Стандартная установка

### 1. Остановите службу

```shell
sudo systemctl stop interbtc-vault.service
```

ИЛИ завершите процесс с помощью `Ctrl+C`.

### 2. Повторно загрузите бинарный файл и установочный скрипт

```shell
wget https://github.com/interlay/interbtc-clients/releases/download/1.0.4/vault -O vault
wget https://raw.githubusercontent.com/interlay/interbtc-docs/master/scripts/vault/setup -O setup
chmod +x ./setup && sudo ./setup
```

### 3. Заново запустите службу

```shell
sudo systemctl start interbtc-vault.service
```

</details>

## Установка из исходного кода

Завершите процесс с помощью `Ctrl+C` и следуйте инструкциям из инструкции [установки](vault/installation?id=install-from-source).
