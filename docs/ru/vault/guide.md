# Работа с клиентом Хранилища

Обычно вам не нужно взаимодействовать с клиентом хранилища, поскольку он автоматически выполняет свои [основные задачи](vault/overview?id=what-do-vaults-do).
Однако иногда необходимо взаимодействовать с клиентом хранилища, например, если вы хотите добавить или снять обеспечение.

В конце этого документа вы сможете:

- [x] Внести дополнительное обеспечение
- [x] Изъять излишки обеспечения
- [x] Узнать об автоматических действиях вашего Vault
- [x] Посетить дашбоард хранилища

## Изменение обеспечения

### Увеличение залога

**Веб-интерфейс**

Перейдите на вкладку Хранилище и нажмите на кнопку рядом с надписью `Collateral: X DOT за Y BTC` (над таблицей Issue Requests). Затем следуйте инструкциям.

**библиотека interbtc-js**.

Вы можете использовать [interbtc-js](https://github.com/interlay/interbtc-js) для блокировки дополнительного обеспечения.

``js
import { createinterbtcAPI } from "@interlay/interbtc";

const interbtc = await createinterbtcAPI("ws://127.0.0.1:9944", "testnet");
interbtc.setAccount(KEYRING);

// 100 DOT, деноминированных в Planck.
const additionalCollateralInPlanck = "1000000000000";
await interbtc.vaults.lockAdditionalCollateral(additionalCollateralInPlanck);
```

### Снятие обеспечения

**Веб-интерфейс**

Перейдите на вкладку Vault и нажмите на кнопку рядом с надписью `Collateral: X DOT за Y BTC` (над таблицей Заявки на выпуск). Затем следуйте инструкциям.

**библиотека interbtc-js**.

Вы можете использовать [interbtc-js](https://github.com/interlay/interbtc-js) для вывода залога.

``js
import { createinterbtcAPI } from "@interlay/interbtc";

const interbtc = await createinterbtcAPI("ws://127.0.0.1:9944", "testnet");
interbtc.setAccount(KEYRING);

// 100 DOT, деноминированных в Planck.
const collateralToWithdrawInPlanck = "1000000000000";
await interbtc.vaults.withdrawCollateral(collateralToWithdrawInPlanck);
```

## Автоматические действия

### Регистрация вашего хранилища

По умолчанию в testnet используется **автоматическая регистрация** с помощью DOT-крана Interlay, заданного в аргументе `auto-register-with-faucet-url`. Другим вариантом регистрации является флаг `auto-register-with-collateral`, как описано в [README](https://github.com/interlay/interbtc-clients/tree/master/vault).

Вы также можете зарегистрировать свое хранилище через наш веб-интерфейс, перейдя на вкладку " Хранилище", нажав кнопку `Регистрация` и выполнив все шаги.

Кроме того, вы можете взаимодействовать с пакетом хранилища напрямую, используя [interbtc-js](https://github.com/interlay/interbtc-js).

``js
import { createinterbtcAPI } from "@interlay/interbtc";

const interbtc = await createinterbtcAPI("ws://127.0.0.1:9944", "testnet");
interbtc.setAccount(KEYRING);

// 100 DOT, деноминированных в Планке
const collateralInPlanck = "1000000000000";
await interbtc.vaults.register(collateralInPlanck, BTC_PUBLIC_KEY);
```

### Заработок комиссионных

Клиент хранилища автоматически зарабатывает комиссионные, как описано в разделе [Модель оплаты вознаграждения](vault/overview?id=fee-model).

### Принятие запросов на выдачу и погашение

Запросы на выпуск и погашение обрабатываются автоматически, подписывая транзакции в сетях Bitcoin и Polkadot с использованием мнемоники/учетных данных, которые вы предоставляете клиенту при его запуске.

### Выход из interBTC

Процесс выхода из interBTC зависит от того, есть ли у вашего клиента хранилища BTC на хранении.

Если в вашем Vault нет BTC на хранении, вы можете в любой момент вывести весь свой залог DOT и покинуть систему. Вы можете остановить клиента Vault без риска быть наказанным. Вы не будете участвовать в запросах на выпуск или погашение после того, как снимете свое обеспечение DOT.

Если ваши клиенты Vault держат на хранении хотя бы _некоторое количество BTC_, у вас есть два варианта выхода из системы. Оба варианта требуют перемещения BTC, которые находятся у вас на хранении. Вариант А, уход через _замену_, требует, чтобы вы запросили замену на другое хранилище. Вы можете запросить замену через [Vault dashboard](https://bridge.interlay.io/vault). Вариант B, уход через _redeem_, требует, чтобы вы ждали, пока пользователь выкупит всю сумму BTC, которую хранит Vault. Только после того, как у вас останется 0 BTC, клиент Vault сможет вывести весь свой залог.

## Приборная панель

Вы можете следить за работой вашего Vault на приборной панели Vault, добавив ключ в расширение [polkadot{.js}](https://polkadot.js.org/extension/).

Как только хранилище будет запущено, в верхней панели приложения появится вкладка "Vault" по адресу [bridge.interlay.io](https://bridge.interlay.io/) (или вы можете получить доступ непосредственно по адресу [bridge.interlay.io/vault](https://bridge.interlay.io/vault)).
