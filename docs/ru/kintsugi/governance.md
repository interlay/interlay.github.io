# Управление: Ликвидное, Оптимистичное, Stake-to-Vote

Модель управления Kintsugi вдохновлена [структурой управления Polkadot] (https://wiki.polkadot.network/docs/learn-governance), но в нее внесены два основных изменения: (1) Ликвидность, оптимистичная модель управления и (2) Stake-to-Vote.

## Право голоса

### Кто может голосовать?

Любой владелец KINT может участвовать в управлении. Для этого вам необходимо заблокировать KINT - см. ниже [stake-to-vote](kintsugi/governance?id=stake-to-vote).

Как **инвестированные**, так и **неинвестированные** KINT могут быть использованы для голосования (и stake-to-vote)!

### Что может быть поставлено на голосование?

Руководство сообщества может предлагать и голосовать по практически любым возможным изменениям в системе. 
В частности:

- **Обновления времени выполнения** (изменения кода для исправления ошибок, обновления субстрата/библиотеки, новые возможности, ...)
- **Обновления параметров** (белый список залогов, пороги ликвидации, принятые оракулы, ...)
- **Расходы казны**

?> В то время как многие протоколы DeFi делают ставку на "постепенную децентрализацию", Interlay фокусируется **на децентрализации с первого дня**. Это влечет за собой повышенный риск в первые дни существования сети, пока сообщество учится организовываться - но этот риск в конечном итоге окупается в среднесрочной и долгосрочной перспективе. Таким образом, Kintsugi выступает в качестве эксперимента, рассчитывая на силу сообщества. 

## Ликвидное и Оптимистичное управление

Чтобы способствовать более активному процессу управления и избежать проблемы "ленивого избирателя", Kintsugi внедряет " оптимистичное управление". Это означает:

- **Нет Совета**, только публичные предложения от сообщества
- Сообщество может избрать **Технический комитет** для ускоренного рассмотрения предложений
- Референдумы по умолчанию **Супербольшинство против (отрицательная явка)**.

### Сверхбольшинство против (негативная предвзятость явки)

Важным отличием является порог голосования с отрицательной явкой (Super-Majority Against). Лучше всего это описано в [Polkadot wiki](https://wiki.polkadot.network/docs/learn-governance):

> При низкой явке для отклонения требуется значительное большинство голосов "против", но при увеличении явки до 100% голосование становится простым большинством голосов "за".

Результат голосования определяется по следующей формуле:

![Формула смещения отрицательных оборотов](../_assets/img/kintsugi/negative-turnout-bias.png)

где

- `против` - голоса против предложения ("против")
- `одобрить` - голоса, поданные за предложение ("за")
- `электорат` - общее количество голосов в сети
- `явка` - это явка избирателей (`против + за`)

Ниже мы представляем процент голосов "против" (по отношению к "электорату"), необходимых для провала предложения:

![Схема: Минимальное количество голосов "ЗА" для провала голосования, для заданных голосов "ПРОТИВ"](../_assets/img/kintsugi/nay-votes-to-fail-vote.png)

Как мы видим, при низкой явке число голосов "против" должно быть значительно больше, чем голосов "за". По мере приближения к 100% явке мы приближаемся к 50% "против" против 50% "за".

?> Идея заключается в том, что предложения, которые сообщество не считает важными (например, незначительные изменения параметров, небольшие предложения по казне и т.д.), как правило, проходят, если только против них нет сильных возражений. Если спорное голосование начнет набирать больше голосов, мы перейдем к традиционному голосованию простым большинством.

## Stake-to-Vote

Чтобы проголосовать за предложения по управлению, пользователи должны заблокировать KINT в парачайне Kintsugi (escrow) - майнинг vKINT, непередаваемого токена, представляющего право голоса каждого пользователя в любой момент времени.

### Неопровержимые факты

- Чем дольше блокируются KINT, тем больше чеканится vKINT.
- Периоды блокировки:
  - **Минимальный: 1 неделя**
  - **Максимум: 96 недель** (в 2 раза больше максимального срока аренды парачейна на Kusama).
- С течением времени баланс vKINT **уменьшается линейно со временем** (за блок).
- KINT могут быть сняты в конце периода блокировки (все сразу).
- Вы можете продлить период блокировки в любое время (до максимального периода блокировки)

?> Идея проста: **Чем дольше KINT заблокирован**, тем **больше голоса** имеет избиратель, поскольку он имеет **долгосрочную заинтересованность в здоровом состоянии и успехе** Kintsugi.

### Награды за стекинг

Пользователи, которые блокируют KINT для чеканки vKINT и участвуют в управлении, получают вознаграждение за стекинг в KINT:

- Годы 1-4: **5% от первоначального 4-летнего предложения KINT**.
- Год 5+: **6,7% от годовой инфляции**.

Вознаграждения за стекинг распределяются на основе каждого блока, пропорционально балансу vKINT для каждого пользователя (с использованием [масштабируемого механизма распределения вознаграждений] от interBTC (https://spec.interlay.io/economics/fees.html#excursion-scalable-reward-distribution)). Это означает: чтобы продолжать получать одинаковое количество вознаграждений, необходимо периодически продлевать блокировки vKINT.

?> Перспективы: В среднесрочной перспективе могут быть добавлены дополнительные критерии для получения вознаграждения за стекинг, если так решит руководство. Например, аккаунты с очень активным участием в голосовании могут получать более высокое вознаграждение.

## Предложения и референдумы

Процесс подачи предложений и голосования очень похож на [структуру управления Polkadot](https://wiki.polkadot.network/docs/learn-governance) - с той разницей, что все предложения являются публичными и проходят через референдум (= процесс голосования), как показано ниже:

<img src="../_assets/img/kintsugi/governance.jpeg" alt="KINT Governance" width="400"/>

### Процесс

1) Любой держатель токенов vKINT может подать публичное предложение. Для этого необходимо внести депозит, который "резервирует" токены vKINT (т.е. ограничивает доступность vKINT для голосования на текущих референдумах или поддержания других предложений. ).
2) Другие держатели vKINT могут [second](https://wiki.polkadot.network/docs/maintain-guides-democracy#seconding-a-proposal) это предложение, указав, сколько vKINT они хотят зарезервировать.

?> Резервирование vKINT не влияет на продолжительность блокировки. Это просто способ ограничить количество предложений, которые вы можете делать параллельно. Пример: у вас есть 10 vKINT и вы поддержали предложение с 2 vKINT. Теперь вы используете 8 vKINT, чтобы поддержать другие предложения (или сделать новые предложения самостоятельно), а 2 зарезервированы до тех пор, пока предложение не перейдет в разряд референдума.  

3) Раз в ``7 дней`` (= ``LaunchPeriod``) предложение с наибольшей поддержкой vKINT становится референдумом (т.е. выходит на голосование).
4) Все vKINT, зарезервированные для этого предложения, высвобождаются (т.е. теперь могут быть использованы для голосования на новом референдуме или для поддержки других предложений).
5) Держатели vKINT голосуют на референдумах. 
6) После окончания периода голосования производится подсчет голосов (см. выше [оптимистичное управление](kintsugi/governance?id=optimistic-governance)).
7) Если голосование прошло, предложение исполняется.

## Технический комитет

В Kintsugi также есть Технический комитет (ТК), состоящий из команд разработчиков и голосуемый руководством сообщества. 

Единственной функцией ТК является ускоренное отслеживание предложений**: ускоренное предложение мгновенно попадает на голосование, даже если другие предложения не получили поддержку vKINT. Этот механизм ускоренного отслеживания используется для обеспечения возможности быстрого обновления системы в случае критических исправлений.

!!!> ТК имеет **без права вето**! ТК может только ускорить рассмотрение предложений - но **не может вмешиваться в голосование руководства**.

При запуске команда Interlay, как основные авторы кода, будет представлять Технический комитет, чтобы гарантировать, что исправления ошибок могут быть быстро отслежены в случае необходимости. Сообщество может избрать другой ТК или увеличить количество мест в ТК в любое время посредством предложения.

## Распределение полномочий при голосовании

Kintsugi будет запущена на 100% децентрализованной с первого дня:

- Никаких ключей администратора,
- Ни один субъект / группа не имеет более 50% голосующего права,
- Любой владелец KINT может участвовать в управлении.

Рекомендуемая литература для понимания динамики управления и права голоса: [Техническое описание экономики токенов KINT, выпущенное Kintsugi Labs] (https://raw.githubusercontent.com/interlay/whitepapers/master/Kintsugi_Token_Economy.pdf).

### Ограничение права голоса команды и инвесторов 
Поскольку в голосовании могут участвовать как наделенные, так и ненаделенные токены, команда Interlay и инвесторы вводят дополнительные ограничения на свое право голоса в течение первых 2 лет после запуска сети. А именно: в то время как сообщество может использовать 100% своих наделенных и ненаделенных KINT для голосования, команда и инвесторы могут использовать только часть своих KINT (которые подлежат блокировке и наделению правами) для участия в управлении - линейно увеличиваясь в течение 2 лет.

### Оценки: Первые 2 года
Предполагаемое распределение голосов Kintsugi в течение первых 2 лет выглядит следующим образом*:

![Распределение голосов Kintsugi за первые 2 года](../_assets/img/kintsugi/voting-power.png)

\* Сюда не входят 10% резерва Фонда и оставшиеся 21% цепной казны, которые исключены из голосования. Вознаграждение LP является приблизительным и может быть выше в течение первых двух лет.