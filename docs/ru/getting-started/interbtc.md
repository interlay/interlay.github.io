# interBTC: Bitcoin на любом блокчейне

## Обзор

interBTC - это актив, обеспеченный Биткойном в соотношении 1:1, который можно использовать для инвестирования, заработка и оплаты BTC в экосистеме DeFi на Polkadot, Ethereum, Cosmos и многих других.

Пока существует interBTC, BTC хранятся под замком в обеспеченных хранилищах Bitcoin - у частных лиц или поставщиков услуг, которые

- (1) принимают BTC на хранение, пока существует interBTC,
- (2) блокируют залог в мультизалоговой системе, основанной на MakerDAO, чтобы защитить пользователей от кражи и потери BTC.

Жизненный цикл interBTC состоит из четырех основных этапов:

![Активы, обеспеченные криптовалютой](../_assets/img/CbA.jpg)

- **Блокировка:** Заприте свои BTC в хранилище. Выберите одно из них или запустите свое собственное. Ваши BTC всегда в безопасности и застрахованы залогом Vault.
- **Минт:** Получайте InterBTC в соотношении 1:1 к вашим заблокированным BTC.
- **BTC DeFi:** Зарабатывайте на своем Bitcoin. Используйте interBTC в качестве залога, для кредитования, выращивания урожая и многого другого. На Polkadot, Kusama, Cosmos, Ethereum и других основных платформах DeFi.
- **Обмен:** Обменивайте interBTC на реальные BTC в Bitcoin - без доверия и в любое время.

## Безопасность interBTC

Что делает interBTC уникальным, так это строгая приверженность принципу недоверия и децентрализации.

- **Страховая безопасность**. Хранилища фиксируют залог на парачейне interBTC в различных цифровых активах - в системе мультизалогового обеспечения, вдохновленной MakerDAO. Если хранилища ведут себя неправильно, их залог уничтожается, а пользователям выплачивается компенсация. Как пользователь, вы доверяете только тому, что Биткойн и платформа DeFi, которую вы используете, безопасны.

- **Радикально открытое**. Любой может стать хранилищем и помочь обеспечить безопасность InterBTC в любое время. Да, вы можете управлять своим собственным хранилищем!

Таким образом, как владелец interBTC, вы имеете следующие гарантии:

?> Вы всегда можете обменять interBTC на BTC или получить возмещение в валюте обеспечения по выгодному курсу.

Если хранилище поведет себя неправильно, вы получите возмещение из залога хранилища и в итоге совершите *прибыльную* сделку между BTC и залоговым активом(ами).

На старте залог будет размещен в DOT. В среднесрочной/долгосрочной перспективе это может быть распространено на стейблкоины или наборы токенов для повышения стабильности.

Резюмируя, чтобы доверять interBTC, вам нужно только:

- *доверять, что Биткойн безопасен*. То есть: доверять тому, что блоки Биткойна являются окончательными после X подтверждений. Мост рекомендует минимум 6 подтверждений, хотя пользователям и приложениям рекомендуется устанавливать более высокие пороги.
- *Доверие, что Polkadot / цепочка, на которой вы используете interBTC, безопасна*. Это предположение делают все приложения, работающие поверх Polkadot.

''> interBTC основан на фреймворке XCLAIM - рецензируемая статья, опубликованная на IEEE S&P, одной из самых престижных конференций по безопасности. Вы можете [прочитать статью](https://eprint.iacr.org/2018/643.pdf) и ознакомиться с другими [исследованиями, опубликованными командой, стоящей за Interlay](../about/research).

Более подробное обсуждение вопросов безопасности можно найти в 200 с лишним страницах [спецификации] (https://spec.interlay.io/security_performance/liquidations.html).

## Участники сети

При разработке interBTC особое внимание уделялось открытости и отсутствию разрешений. Таким образом, любой пользователь может одновременно выполнять несколько ролей, а также покинуть систему в любой момент. Таким образом, для участия в системе вы можете выбирать из:

**хранилища:** залоговые посредники, которые держат BTC, запертые на Bitcoin. Любой пользователь может стать хранилищем, просто заблокировав залог DOT. Требования: (1) полнофункциональная нода  Bitcoin, (2) аккаунт Polkadot и (3) ликвидность в принятых залоговых активах.

**Пользователи:** На BTC Parachain есть два типа пользователей:

- **Провайдеры ликвидности** блокируют BTC в хранилищах для майнинга 1:1 подкрепленных *interBTC* на Парачейне. Требования: (1) Биткойн-кошелек и (2) кошелек Polkadot.

- **Конечные пользователи** получают interBTC от поставщиков ликвидности на Polkadot и используют interBTC для платежей и в приложениях. Требования: Кошелек Polkadot

Оба могут в любое время обменять принадлежащие им InterBTC на BTC (требуется кошелек Bitcoin).

**Коллаторы** поддерживают парачейн Interlay, собирая транзакции и создавая доказательства безопасности, которые проверяются валидаторами релейной цепочки Polkadot. Это специфическая для Polkadot роль. [Подробнее о Коллаторах в Polkadot Wiki] (https://wiki.polkadot.network/docs/learn-collator).

<div id="step-by-step"></div>

## Минт и обмен

### Заблокировать BTC для майнинга interBTC

Пользователь (поставщик ликвидности) майнит новые InterBTC.

1. Хранилище блокирует DOT в качестве обеспечения с помощью моста interBTC (Interlay BTC Parachain).

1. Пользователь создает запрос на эмиссию в выбранном им Vault с обеспечением. При этом резервируется обеспечение DOT хранилища.

1. Затем пользователь отправляет BTC в хранилище.

1. Пользователь подтверждает мосту interBTC, что он отправил BTC в хранилище (используя доказательство включения транзакции против BTC Relay).

1. После успешной проверки доказательства пользователь майнит interBTC и получает токены на баланс своего счета.

![Высокоуровневый процесс эмиссии interBTC](../_assets/img/issue.png) *Высокоуровневый процесс эмиссии interBTC*.

### Обмен interBTC на BTC

Пользователь обменивает interBTC на эквивалентное количество BTC или получает DOT в качестве возмещения.

1. Чтобы запросить выкуп, пользователь блокирует interBTC с помощью моста interBTC (Interlay BTC парачейн).

1. Парачейн поручает хранилищу выполнить выкуп.

1. Хранилище переводит пользователю нужную сумму BTC.

1. Чтобы разблокировать залог DOT, хранилище отправляет в BTC-Relay доказательство включения транзакции.

1. Если доказательство верное, Парачейн разблокирует DOT-ы хранилища.

1. Если доказательство не было предоставлено вовремя, парачейн сокращает DOTs хранилища и возмещает их пользователю по выгодному курсу.

![Высокоуровневый процесс InterBTC Redeem](../_assets/img/redeem.png)* Высокоуровневый процесс обмен InterBTC*.

## Залог

Чтобы защитить пользователей от кражи и потери BTC, хранилища должны заблокировать залог с парачейном таким образом, чтобы стоимость залога была выше стоимости BTC, заблокированных в хранилище. Чтобы у хранилищ не было стимула красть BTC пользователей, хранилища предоставляют залог в активах, включенных в белый список - по процедуре, аналогичной MakerDAO. Для смягчения колебаний обменных курсов interBTC использует избыточное обеспечение и многоуровневую схему балансировки обеспечения.

### Многоуровневая система обеспечения

Операторы хранилищ могут свободно выбирать для блокировки залога любой из активов, включенных в белый список [protocol governance](/getting-started/interbtc?id=governance). Каждое хранилище ассоциируется с одним конкретным активом обеспечения - и один оператор может содержать неограниченное количество хранилищ. Валюта каждого залогового актива имеет установленный руководством порог, определяющий, какое ее количество может быть заблокировано в системе в качестве залога. По достижении этого порога новые хранилища должны выбираться из других залоговых активов или запрашиваться для увеличения порога с помощью протокола управления.

Для пользователей различие между залоговыми активами становится актуальным только при обмене InterBTC на BTC. Пользователи могут выбрать конкретные хранилища (одно или несколько) для процесса выкупа - и в случае неудачи получают возмещение в виде залога этих конкретных хранилищ.

Подробнее о системе обеспечения и ребалансировке можно прочитать в разделе [Коллатор на странице хранилища](/vault/overview?id=collateral).

### Ликвидации

Хранилища могут быть ликвидированы, т.е. их залог будет уничтожен и использован для восстановления баланса системы или для возмещения ущерба пользователям, если:

- они крадут BTC,
- не выполняют запросы на погашение,
- или имеют недостаточное обеспечение.

Подробнее о ликвидациях читайте в разделе [Ликвидации на странице хранилища](/vault/overview?id=liquidations).

## kBTC: Канареечная Сеть 

Флагманский продукт как Kintsugi, так и Interlay - "Bitcoin без доверия для DeFi".

kBTC на Kintsugi. interBTC на Interlay.

interBTC и kBTC - это один и тот же продукт в основе, но с разными параметрами и профилями риска. В частности, у них будут разные залоги и пороги безопасности.

- **interBTC**. В качестве залога для interBTC будут приниматься только активы с большой ликвидностью, прошедшие строгий анализ безопасности - аналогично процессу MakerDAO для DAI.

- **kBTC**, с другой стороны, будет открыт для более экспериментальных активов, включая те, которые имеют более низкую ликвидность - например, новые формы деривативов на ставку, токены LP, новые активы DeFi  и даже NFT

# InterBTC против аналогов

> *Рим не был построен за один день* (пословица)

Централизованные решения быстро создаются. Поэтому существует ряд централизованных и надежных провайдеров Bitcoin, в основном работающих на Ethereum (полный список см. [здесь](https://defipulse.com/btc)). Тем не менее, Bitcoin был создан с концепцией децентрализации - и задача Interlay состоит в том, чтобы гарантировать, что Bitcoin на других цепочках следует тем же принципам.

Мы приводим сравнение ниже, используя [эту рецензируемую схему анализа кроссчейн взаимодействия] (https://fc21.ifca.ai/papers/139.pdf):

![InterBTC против других централизованных систем Bitcoin](../_assets/img/interbtc-vs-other.png) *InterBTC против других централизованных систем Bitcoin.

- **wBTC** был создан BitGo, компанией по хранению криптовалют. Все BTC, заблокированные в wBTC, хранятся в BitGo. Вы не можете свободно присоединиться, чтобы стать хранителем BTC (только BitGo имеет хранилище), и нет никакой гарантии на случай сбоя. Пользователи должны доверять BitGo. Если BTC будет потерян, украден или подвергнется регуляторным действиям, wBTC не будет иметь никакой ценности, подкрепляющей его. Если использовать в качестве аналогии долларовые стейблкоины, то эквивалентом wBTC будет USDT/USDC.
- **renBTC** is a product of the Ren protocol, a crypto-startup that pivoted from Republic Protocol (a former protocol for dark pools, aka privacy-preserving trading). The BTC locked in renBTC is reportedly<sup>[1,](https://www.theblockcrypto.com/news+/76787/ren-bitcoin-wallet-decentralization)</sup><sup>[2](https://decrypt.co/40110/massive-honeypot-ren-holds-100m-bitcoin-centralized-wallet)</sup> held in a multisig controlled by the Ren team. It appears that Ren's Dark Nodes (part of the former dark pool protocol) are not responsible for the BTC custody - and hence it is not possible for new users to help secure Ren's locked BTC, making it a centralized protocol. Just like with wBTC, there is no insurance to reimburse users if BTC is lost - users must simply trust the Ren team. Using the USD stablecoin analogy, the equivalent of renBTC is USDT/USDC.
- **tBTC v1** tried to build a decentralized version of BTC on Ethereum, similar to the design proposed in the XCLAIM paper (i.e., similar to interBTC). The BTC locked in tBTC is locked with Signers, who share control over the BTC keys using ECDSA threshold signatures (3-of-5). Signers provide collateral in ETH, which is used to reimburse users if BTC is lost. However, not everyone can become a Signer, citing the [tBTC FAQ](https://tbtc.network/faq/): "*Shortly after launch, there should be a group of roughly 80 private sale KEEP purchasers and a few other trusted parties signing for tBTC*". Due to tBTC v1 being a single-collateral system (ETH only), Signers [reportedly](https://tian7eth.medium.com/an-analyse-of-a-liquidation-of-tbtc-system-on-mainnet-82c70c9743e6) suffered from frequent liquidations, when the BTC/ETH price was volatile.
    - *Why tBTC cannot decentralize the Signers: The ECDSA Threshold attack vector*: In fact, tBTC cannot decentralized the Signers, allowing anyone to join the Signer set, due to a security issue with their ECDSA threshold signatures: currently, it is only possible to check that sufficient signatures were made, but not *who* signed. This means that in a 3-of-5 threshold sig, all 5 Signers will be slashed if BTC is lost or stolen. If anyone could become a Signer, an attacked would create multiple identities try to mint/redeem tBTC until she is assigned a Signer set where she controls 3 out of the 5 Signer keys. The attacked then proceeds to steal (*"from herself"!*) the BTC using her 3 Signer keys. The protocol would slash the collateral of all 5 Signers - **effectively allowing the attacked to steal collateral of the 2 honest signers**. This can be repeated until all honest signers are slashed (or there is a manual/governance intervention).
    - **tBTC v2** is moving back to a more centralized model<sup>[3](https://evandrosaturnino.medium.com/why-does-a-trustless-bitcoin-in-defi-matter-77c0d544f0d9)</sup>: BTC will be controlled by a larger group of 100 Signers, insurance will be locked in KEEP tokens only and cover a fraction of the locked BTC value. Apart from being a federated model, a v2 introduces the risk of using the native token as collateral: if there is a security breach, the token price will likely fall rapidly, making the insurance potentially worthless.


**interBTC**, for comparison:

- **Anyone** can become a Vault in interBTC, making it fully **decentralized**;
- Vaults cannot prevent users from minting interBTC, making it **censorship resistant**
- Vaults lock **collateral in different assets (MakerDAO-like multi-collateral system)**, making the price peg **more stable** and avoiding frequent liquidations. This also allows to use interest bearing assets, such as liquid staking assets, as collatera, making interBTC **more capital efficient**.
- If BTC is lost or stolen, users are **reimbursed in collateral at a beneficial rate** (~110%), making it **financially trustless**.

Using the USD stablecoin analogy, **interBTC is comparable to multi-collateral DAI - but better, because interBTC can be redeemed for BTC but DAI cannot be redeemed for physical USD**. So, strictly speaking, interBTC is a Bitcoin stablecoin.
