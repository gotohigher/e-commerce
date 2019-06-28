[✔]: assets/images/checkbox-small-blue.png

# Node.js Лучшие практики

<h1 align="center">
  <img src="assets/images/banner-2.jpg" alt="Node.js Лучшие практики">
</h1>

<br/>

<div align="center">
  <img src="https://img.shields.io/badge/⚙%20Item%20count%20-%2083%20Best%20Practices-blue.svg" alt="83 items"> <img src="https://img.shields.io/badge/%F0%9F%93%85%20Last%20update%20-%20Jun%205%202019-green.svg" alt="Последне обновление: Jun 5, 2019"> <img src="https://img.shields.io/badge/ %E2%9C%94%20Updated%20For%20Version%20-%20Node%2012.14.0%20LTS-brightgreen.svg" alt="Обновлено для Node 12.14.0 LTS">
</div>

<br/>

[![nodepractices](/assets/images/twitter-s.png)](https://twitter.com/nodepractices/) **Следите за нами в Twitter!** [**@nodepractices**](https://twitter.com/nodepractices/)

<br/>

Читайте на других языках: [![CN](/assets/flags/CN.png)**CN**](/README.chinese.md), [![BR](/assets/flags/BR.png)**BR**](/README.brazilian-portuguese.md) [(![ES](/assets/flags/ES.png)**ES**, ![FR](/assets/flags/FR.png)**FR**, ![HE](/assets/flags/HE.png)**HE**, ![KR](/assets/flags/KR.png)**KR**, ![RU](/assets/flags/RU.png)**RU** and ![TR](/assets/flags/TR.png)**TR** в процессе!)](#translations)

<br/>

###### Создано и поддерживается нашим [руководящим комитетом](#steering-committee) and [соавторами](#collaborators)

# Последние лучшие практики и новости

- **Новая лучшая практика:** 4.4: [Избегайте тестовых приспособлений, добавляйте данные для теста](https://github.com/i0natan/nodebestpractices#4-testing-and-overall-quality-practices)

- **Новая лучшая практика:** 6.25: [Избегайте публикации секретных данных в NPM реестр](/sections/security/avoid_publishing_secrets.md)

- **Новый перевод:** Теперь доступен ![BR](/assets/flags/BR.png) [бразильский португальский](/README.brazilian-portuguese.md), любезно предоставленый [Marcelo Melo](https://github.com/marcelosdm)! ❤️

<br/><br/>

# Добро пожаловать! 3 вещи, которые вы должны знать в первую очередь:

**1. На самом деле вы читаете десятки лучших статей Node.js -** этот репозиторий представляет собой краткий обзор и список наиболее популярных материалов по рекомендациям Node.js, а также материалов, написанных здесь соавторами.

**2. Это самая большая подборка, и она растет каждую неделю -** в настоящее время представлено более 80 2передовых практик, руководств по стилю и архитектурных советов. Новые выпуски и запросы на добавление, чтобы обновлять эту живую книгу, создаются каждый день. Мы бы хотели, чтобы вы внесли свой вклад здесь, будь то исправление ошибок в коде, помощь с переводами или предложение блестящих новых идей. Смотрите наши [правила написания здесь](/.operations/writing-guidelines.md).

**3. У большинства лучших практик есть дополнительная информация -** большинство маркеров включают в себя ссылку **🔗 Подробнее**, которая расширяет практику с примерами кода, цитатами из выбранных блогов и дополнительной информацией.

<br/><br/>

## Оглавление

1.  [Практики структуры проекта (5)](#1-Практики-структуры-проекта)
2.  [Практики обработки ошибок (11)](#2-Практики-обработки-ошибок)
3.  [Практики стиля кода (12)](#3-Практики-стиля-кода)
4.  [Тестирование и общие методы контроля качества (11)](#4-Тестирование-и-общие-методы-контроля-качества)
5.  [Переход к производственным практикам (18)](#5-Переход-к-производственным-практикам)
6.  [Практики безопасности (25)](#6-Практики-безопасности)
7.  [Практики эффективности (1) (В процессе️ ✍️)](#7-Практики-эффективности)

<br/><br/>

# `1. Практики структуры проекта`

## ![✔] 1.1 Структурируйте свое решение по компонентам

**TL;DR:** Наихудшая ловушка для больших приложений -- поддержка огромной базы кода с сотнями зависимостей -- такой монолит замедляет разработчиков, поскольку они пытаются внедрить новые функции. Вместо этого разделите ваш код на компоненты, каждый получает свою собственную папку или выделенную кодовую базу, и убедитесь, что каждый модуль остается маленьким и простым. Посетите «Подробнее» ниже, чтобы увидеть примеры правильной структуры проекта.

**Иначе:** Когда разработчики, которые пишут новые функции, изо всех сил пытаются понять влияние своих изменений и боятся сломать другие зависимые компоненты, развертывания становятся медленнее и рискованнее. Также считается сложнее масштабировать, когда все бизнес-единицы не разделены.

🔗 [**Подробнее: структура по компонентам**](/sections/projectstructre/breakintcomponents.md)

<br/><br/>

## ![✔] 1.2 Выделяйте ваши компоненты в отдельный слой, держите Express в его границах

**TL;DR:** Каждый компонент должен содержать «слои» -- выделенный объект для сети, логики и кода доступа к данным. Это не только четко разделяет задачи, но и значительно облегчает проверку и тестирование системы. Хотя это очень распространенный шаблон, разработчики API, как правило, смешивают слои, передавая объекты веб-слоя (Express req, res) в бизнес-логику и уровни данных - это делает ваше приложение зависимым и доступным только для Express.

**Иначе:** Приложение, которое смешивает веб-объекты с другими слоями, не может быть доступно для тестирования кода, заданий CRON и других вызовов в обход Express.

🔗 [**Подробнее: создание слоев вашего приложения**](/sections/projectstructre/createlayers.md)

<br/><br/>

## ![✔] 1.3 Завернуть общие утилиты в пакеты npm

**TL;DR:** В большом приложении, которое составляет большую кодовую базу, универсальные утилиты, такие как регистратор, модуль шифрования и т.п., должны быть обернуты вашим собственным кодом и представлены как частные пакеты npm. Это позволяет делиться ими между несколькими кодовыми базами и проектами.

**Иначе:** Вам придется изобрести собственный велосипед для развертывания и поддержания зависимостей.

🔗 [**Подробнее: Структура по функциям**](/section/projectstructre/wraputilities.md)

<br/><br/>

## ![✔] 1.4 Разделяйте Express "приложение" и "сервер"

**TL;DR:** Избегайте неприятной привычки определять все приложение [Express] (https://expressjs.com/) в одном огромном файле -- разделите определение "Экспресс" как минимум на два файла: декларация API (app.js) и сетевые задачи (www). Для еще лучшей структуры локализуйте объявление API в компонентах.

**Иначе:** Ваш API будет доступен для тестирования только через HTTP-вызовы (медленнее и намного сложнее создавать отчеты о покрытии). Скорее всего, не составит большого труда хранить сотни строк кода в одном файле.

🔗 [**Подробнее: Разделяйте Express "приложение" и "сервер"**](/section/projectstructre/separateexpress.md)

<br/><br/>

## ![✔] 1.5 Используйте конфигурацию с учетом среды, безопасную и иерархическую конфигурацию

**TL;DR:** Идеальная и безупречная конфигурация конфигурации должна обеспечивать (а) считывание ключей из файла И из переменной среды, (б) хранение секретов вне основной кодовой базы, (в) иерархическую структуру для облегчения поиска. Есть несколько пакетов, которые могут помочь поставить галочку в большинстве таких полей, как [rc](https://www.npmjs.com/package/rc), [nconf](https://www.npmjs.com/package/nconf) and [config](https://www.npmjs.com/package/config)

**Иначе:** Невыполнение каких-либо требований к конфигурации приведет к срывам в работе разработчиков или devops командой. Вероятно, и тех и других.

🔗 [**Подробнее: лучшие рекомендации по настройке**](/section/projectstructre/configguide.md)

<br/><br/><br/>

<p align="right"><a href="#оглавление">⬆ К началу</a></p>

# `2. Практики обработки ошибок`

## ![✔] 2.1 Используйте Async-Await или обещания для обработки асинхронных ошибок

**TL;DR:** Обработка асинхронных ошибок в стиле обратного вызова, вероятно, является самым быстрым путем в ад (еще говорят "Callback Hell" или "The Pyramid of Doom"). Лучший подарок, который вы можете сделать своему коду, -- это использовать надежную библиотеку обещаний или async-await, что позволяет использовать более компактный и знакомый синтаксис кода, такой как try-catch.

**Иначе:** Стиль обратного вызова Node.js, функция (err, response), является многообещающим способом создания непригодного для использования кода из-за сочетания обработки ошибок со случайным кодом, чрезмерных вложений и слабых шаблонов кодирования.

🔗 [**Подробнее: уход от обратных вызовов**](/section/errorhandling/asyncerrorhandling.md)

<br/><br/>

## ![✔] 2.2 Используйте только встроенный объект Error

**TL;DR:** Многие выдают ошибки в виде строки или некоторого пользовательского типа -- это усложняет логику обработки ошибок и взаимодействие между модулями. Отклоните ли вы обещание, сгенерируете исключение или сгенерируете ошибку -- использование лишь встроенного объекта `Error` увеличит единообразие и предотвратит потерю информации.

**Иначе:** При вызове какого-либо компонента нельзя быть уверенным, какой тип ошибок приходит в ответ -- это значительно затрудняет правильную обработку ошибок. Хуже того, использование пользовательских типов для описания ошибок может привести к потере информации о критических ошибках, таких как трассировка стека!

🔗 [**Подробнее: использование встроенного объекта ошибки**](/section/errorhandling/useonlythebuiltinerror.md)

<br/><br/>

## ![✔] 2.3 Различайте ошибки в работе и программировании

**TL;DR:** Операционные ошибки (например, API получил неверный ввод) относятся к известным случаям, когда влияние ошибки полностью осознается и может быть обработано вдумчиво. С другой стороны, ошибка программиста (например, попытка прочитать неопределенную переменную) относится к неизвестным ошибкам кода, которые требуют изящного перезапуска приложения.

**Иначе:** Вы всегда можете перезапустить приложение, когда появляется ошибка, но зачем подводить ~5000 онлайн-пользователей из-за незначительной, прогнозируемой, операционной ошибки? Обратное также не идеально -- поддержание приложения в том случае, если возникла неизвестная проблема (ошибка программиста), может привести к непредсказуемому поведению. Разграничение между ними позволяет действовать тактично и применять сбалансированный подход, основанный на данном контексте.

🔗 [**Подробнее: операционная или программистская ошибка**](/sections/errorhandling/operationalvsprogrammererror.md)

<br/><br/>

## ![✔] 2.4 Обрабатывате ошибки централизованно, а не в промежуточном слое Express

**TL;DR:** Логика обработки ошибок, такая как уведомление по почте администратора или ведение журнала, должна быть заключена в выделенный и централизованный объект, который вызывается всеми конечными точками (например, промежуточные слои Express, задания cron, модульное тестирование) при возникновении ошибки.

**Иначе:** Необработка ошибок в одном месте приведет к дублированию кода и, возможно, к ошибкам, обработанным неправильно.

🔗 [**Подробнее: обработка ошибок в централизованном месте**](/sections/errorhandling/centralizedhandling.md)

<br/><br/>

## ![✔] 2.5 Документирование ошибок API при использовании Swagger или GraphQL

**TL;DR:** Пусть ваши вызовы API знают, какие ошибки могут прийти взамен, чтобы они могли обрабатывать их вдумчиво без сбоев. Для API RESTful это обычно делается с помощью каркасов документации, таких как Swagger. Если вы используете GraphQL, вы также можете использовать свою схему и комментарии.

**Иначе:** Клиент API может принять решение о сбое и перезапуске только потому, что он получил ошибку, которую он не может понять. Примечание: вызывающим абонентом вашего API можете быть и вы сами (очень типично для микросервисной среды)

🔗 [**Подробнее: документирование ошибок API в Swagger или GraphQL**](/sections/errorhandling/documentingusingswagger.md)

<br/><br/>

## ![✔] 2.6 Изящно выходите из процесса, когда в город приезжает незнакомец

**TL;DR:** При возникновении неизвестной ошибки (ошибка разработчика, см. рекомендацию 2.3) - существует неопределенность в отношении работоспособности приложения. Обычная практика предполагает осторожный перезапуск процесса с использованием инструмента управления процессами, такого как [Forever](https://www.npmjs.com/package/forever) или [PM2](http://pm2.keymetrics.io/).

**Иначе:** Когда происходит незнакомое исключение, некоторый объект может быть в неисправном состоянии (например, источник событий, который используется глобально и больше не генерирует события из-за некоторого внутреннего сбоя), и все будущие запросы могут давать сбой или вести себя безумно.

🔗 [**Подробнее: закрытие процесса**](/sections/errorhandling/shuttingtheprocess.md)

<br/><br/>

## ![✔] 2.7 Используйте надежный регистратор для улучшения видимости ошибок

**TL;DR:** Набор развитых инструментов ведения журналов, таких как [Winston](https://www.npmjs.com/package/winston), [Bunyan](https://github.com/trentm/node-bunyan) или [Log4js] http://stritti.github.io/log4js/) ускорит обнаружение и понимание ошибок. Так что забудьте о console.log.

**Иначе:** Сканирование через console.logs или вручную через грязный текстовый файл без запросов инструментов или приличного просмотра журнала может занять вас на работе до поздна.

🔗 [**Подробнее: использование надежного регистратора**](/sections/errorhandling/usematurelogger.md)

<br/><br/>

## ![✔] 2.8 Тестируйте потоки ошибок, используя ваш любимый тестовый фреймворк

**TL;DR:** Будь то профессиональный автоматический контроль качества или простое ручное тестирование разработчиком -- убедитесь, что ваш код не только удовлетворяет положительным сценариям, но также обрабатывает и возвращает правильные ошибки. Среды тестирования, такие как Mocha & Chai, могут легко справиться с этим (см. Примеры кода в "Gist popup").

**Иначе:** Без тестирования, будь то автоматически или вручную, вы не сможете полагаться на свой код для возврата правильных ошибок. Без значения ошибок -- нет обработки ошибок.

🔗 [**Подробнее: тестирование потоков ошибок**](/section/errorhandling/testingerrorflows.md)

<br/><br/>

## ![✔] 2.9 Находите ошибки и простои с использованием продуктов APM

**TL;DR:** Продукты для мониторинга и производительности (a.k.a APM) проактивно измеряют вашу кодовую базу или API, чтобы они могли автоматически подсвечивать ошибки, сбои и медленные части, которые вы пропустили.

**Иначе:** Вы можете потратить огромные усилия на измерение производительности и времени простоя API, возможно, вы никогда самостоятельно не узнаете, какие части кода в реально сценарии самые медленные, и как они влияют на UX.

🔗 [**Подробнее: использование продуктов APM**](/section/errorhandling/apmproducts.md)

<br/><br/>

## ![✔] 2.10 Ловите необработанные отказы от обещаний

**TL;DR:** Любое исключение, выданное в обещании, будет проглочено и отброшено, если разработчик не забудет явно обработать. Даже если ваш код подписан на `process.uncaughtException`! Преодолейте это, зарегистрировавшись на событие `process.unhandledRejection`.

**Иначе:** Ваши ошибки будут проглочены и не оставят следов. Не о чем беспокоиться!

🔗 [**Подробнее: перехват необработанного отказа от обещания**](/sections/errorhandling/catchunhandledpromiserejection.md)

<br/><br/>

## ![✔] 2.11. Быстро проваливайтесь, проверяя аргументы, используя выделенную библиотеку

**TL;DR:** Это должно быть частью вашей лучшей практики Express - вводите данные API, чтобы избежать неприятных ошибок, которые потом будет намного сложнее отследить. Код проверки обычно утомителен, если вы не используете очень классную вспомогательную библиотеку, такую ​​как Joi.

**Иначе:** Учтите это -- ваша функция ожидает числовой аргумент "Скидка", который вызывающая сторона забывает передать, позже ваш код проверяет, если Скидка !=0 (сумма разрешенной скидки больше нуля), тогда она позволит пользователю пользоваться скидкой. О, Боже, какая неприятная ошибка! Видишь?

🔗 [**Подробнее: быстрый провал**](/sections/errorhandling/failfast.md)

<br/><br/><br/>

<p align="right"><a href="#оглавление">⬆ К началу</a></p>

# `3. Практики стиля кода`

## ![✔] 3.1 Используйте ESLint

**TL;DR:** [ESLint](https://eslint.org) является стандартом де-факто для проверки возможных ошибок кода и исправления стиля кода не только для выявления проблем с пробелами, но и для выявления серьезных анти-паттернов, которые выдают разработчики ошибки без классификации. Хотя ESLint может автоматически исправлять стили кода, другие инструменты, такие как [prettier](https://www.npmjs.com/package/prettier) и [beautify](https://www.npmjs.com/package/js-beautify) более эффективны при форматировании исправлений и работают совместно с ESLint.

**Иначе:** Разработчики сосредоточатся на утомительных проблемах с интервалами и шириной линии, и время может быть потрачено впустую на продумывание стиля кода проекта.

🔗 [**Подробнее: использование ESLint и Prettier**](/section/codestylepractices/eslint_prettier.md)

<br/><br/>

## ![✔] 3.2 Специальные плагины Node.js

**TL;DR:** Помимо стандартных правил ESLint, которые охватывают ванильный JavaScript, добавьте специальные плагины Node.js, например [eslint-plugin-node](https://www.npmjs.com/package/eslint-plugin-node), [eslint-plugin-mocha](https://www.npmjs.com/package/eslint-plugin-mocha) и [eslint-plugin-node-security](https://www.npmjs.com/package/eslint-plugin-security).

**Иначе:** Многие неисправные шаблоны кода Node.js могут скрыться за радаром. Например, разработчикам могут потребоваться файлы (variableAsPath) с переменной, указанной в качестве пути, которая позволяет злоумышленникам выполнить любой сценарий JS. Линтеры Node.js могут обнаружить такие паттерны и заранее сообщить о проблеме.

<br/><br/>

##! [✔] 3.3 Начинайте кодовый блок фигурными скобками на той же линии.

**TL;DR:** Открывающие фигурные скобки блока кода должны находиться на той же строке, что и оператор открытия.

### Пример кода

```javascript
// Делайте так
function someFunction() {
  // code block
}

// Избегайте
function someFunction()
{
  // code block
}
```

**Иначе:** Отклонение от этой передовой практики может привести к неожиданным результатам, как видно из топика StackOverflow ниже:

🔗 [**Подробнее:** "Why do results vary based on curly brace placement?" (StackOverflow)](https://stackoverflow.com/questions/3641519/why-does-a-results-vary-based-on-curly-brace-placement)

<br/><br/>

##! [✔] 3.4 Разделяйте свои выражения правильно

Независимо от того, используете ли вы точки с запятой или нет для разделения своих операторов, знание общих ошибок неправильных разрывов строк или автоматической вставки точек с запятой поможет вам устранить обычные синтаксические ошибки.

**TL;DR:** Используйте ESLint для получения информации о проблемах разделения. [Prettier](https://prettier.io/) или [Standardjs](https://standardjs.com/) могут автоматически решить эти проблемы.

**Иначе:** Как видно из предыдущего раздела, интерпретатор JavaScript автоматически добавляет точку с запятой в конце оператора, если его нет, или считает, что оператор не завершен, где он должен, что может привести к нежелательным результатам. , Вы можете использовать присваивания и избегать немедленного вызова выражений функций для предотвращения большинства непредвиденных ошибок.

### Пример кода

```javascript
// Делайте так
function doThing() {
    // ...
}

doThing()

// Делайте так

const items = [1, 2, 3]
items.forEach(console.log)

// Избегайте — будет выброшена ошибка
const m = new Map()
const a = [1,2,3]
[...m.values()].forEach(console.log)
> [...m.values()].forEach(console.log)
>  ^^^
> SyntaxError: Unexpected token ...

// Избегайте — будет выброшена ошибка
const count = 2 // it tries to run 2(), but 2 is not a function
(function doSomething() {
  // do something amazing
}())
// ставим точку с запятой перед непосредственно вызванной функцией, после определения const сохраняем возвращаемое значение анонимной функции в переменной или вообще избегаем последовательного написания IIFE
```

🔗 [**Подробнее:** "Semi ESLint rule"](https://eslint.org/docs/rules/semi)
🔗 [**Подробнее:** "No unexpected multiline ESLint rule"](https://eslint.org/docs/rules/no-unexpected-multiline)

<br/><br/>

## ![✔] 3.5 Назовите свои функции

**TL;DR:** Назовите все функции, включая замыкания и обратные вызовы. Избегайте анонимных функций. Это особенно полезно при профилировании приложения узла. Обозначение всех функций позволит вам легко понять, на что вы смотрите при проверке снимка памяти.

**Иначе:** Отладка производственных проблем с использованием дампа ядра (снимока памяти) может стать сложной, так как вы замечаете значительное потребление памяти анонимными функциями.

<br/><br/>

## ![✔] 3.6 Используйте соглашения об именах переменных, констант, функций и классов

**TL;DR:** Используйте **_lowerCamelCase_** при именовании констант, переменных и функций и **_UpperCamelCase_** (также с большой буквы) при именовании классов. Это поможет вам легко различать простые переменные/функции и классы, которые требуют реализации. Используйте описательные имена, но старайтесь, чтобы они были короткими.

**Иначе:** Javascript -- это единственный язык в мире, который позволяет напрямую вызывать конструктор ("Class") без его инициализации. Следовательно, классы и конструкторы функций должщны различаться, начинаясь с UpperCamelCase.

### Пример кода

```javascript
// для класса мы используем UpperCamelCase
class SomeClassExample {}

// для константы мы используем служебное слово const и lowerCamelCase
const config = {
  key: 'value'
};

// для переменных и функций мы используем lowerCamelCase
let someVariableExample = 'value';
function doSomething() {}
```

<br/><br/>

## ![✔] 3.7 Предпочитайте const, а не let. Забудьте var

**TL;DR:** Использование `const` означает, что как только переменная назначена, она не может быть переназначена. Предпочтение `const` поможет вам не поддаваться искушению использовать одну и ту же переменную для разных целей и сделает ваш код более понятным. Если переменная должна быть переназначена, например, в цикле for используйте `let`, чтобы объявить ее. Другим важным аспектом `let` является то, что переменная, объявленная с ее использованием, доступна только в той области блока, в которой она была определена. `var` является областью функции, а не областью блока и [не должен использоваться в ES6](https://hackernoon.com/why-you-shouldnt-use-var-anymore-f109a58b9b70) теперь, когда у вас есть `const` и `let` в вашем распоряжении.

**Иначе:** Отладка становится намного более громоздкой, когда необходимо следовать за переменной, которая часто изменяется.

🔗 [**Подробнее: JavaScript ES6+: var, let, or const?** ](https://medium.com/javascript-scene/javascript-es6-var-let-or-const-ba58b8dcde75)

<br/><br/>

## ![✔] 3.8 Подключайте модули вначале, а не внутри функций

**TL;DR:** Подключайте модули в начале каждого файла, до и вне каких-либо функций. Эта простая рекомендация не только поможет вам легко и быстро определить зависимости файла прямо вверху, но и позволит избежать пары потенциальных проблем.

**Иначе:** Подключения выполняются синхронно с Node.js. Если они вызываются из функции, она может заблокировать обработку других запросов в более критическое время. Кроме того, если требуемый модуль или какая-либо из его собственных зависимостей выдает ошибку и приводит к сбою сервера, лучше узнать об этом как можно скорее, что может быть не так, если этот модуль требуется изнутри функции.

<br/><br/>

## ![✔] 3.9 Подключайте модули по папкам, а не по файлам напрямую

**TL;DR:** При разработке модуля/библиотеки в папке поместите файл index.js, который раскрывает внутреннюю часть модуля, чтобы каждый потребитель проходил через него. Это служит "интерфейсом" для вашего модуля и облегчает будущие изменения, не нарушая контракт.

**Иначе:** Изменение внутренней структуры файлов или подписи может нарушить интерфейс с клиентами.

### Пример кода

```javascript
// Делайте так
module.exports.SMSProvider = require('./SMSProvider');
module.exports.SMSNumberResolver = require('./SMSNumberResolver');

// Избегайте
module.exports.SMSProvider = require('./SMSProvider/SMSProvider.js');
module.exports.SMSNumberResolver = require('./SMSNumberResolver/SMSNumberResolver.js');
```

<br/><br/>

## ![✔] 3.10 Используйте оператор `===`

**TL;DR:** Предпочитайте оператор строгого равенства `===` более слабому абстрактному оператору равенства `==`. `==` сравнивает две переменные после преобразования их в общий тип. В `===` нет преобразования типов, и обе переменные должны иметь одинаковый тип, чтобы быть равными.

**Иначе:** Неравные переменные могут возвращать true при сравнении с оператором `==`.

### Пример кода

```javascript
'' == '0'           // false
0 == ''             // true
0 == '0'            // true

false == 'false'    // false
false == '0'        // true

false == undefined  // false
false == null       // false
null == undefined   // true

' \t\r\n ' == 0     // true
```

Все приведенные выше операторы вернут false, если используются с `===`.

<br/><br/>

## ![✔] 3.11 Используйте Async Await, избегайте колбэков

**TL;DR:** Node 8 LTS теперь имеет полную поддержку Async-await. Это новый способ работы с асинхронным кодом, который заменяет обратные вызовы и обещания. Async-await не блокирует и делает асинхронный код синхронным. Лучший подарок, который вы можете дать своему коду, -- это использовать async-await, который обеспечивает гораздо более компактный и знакомый синтаксис кода, такой как try-catch.

**Иначе:** Обработка асинхронных ошибок в стиле обратного вызова, вероятно, самый быстрый путь в ад -- этот стиль вынуждает проверять ошибки повсюду, справляться с неудобным вложением кода и затрудняет рассуждение о потоке кода.

🔗 [**Подробнее:** Guide to async await 1.0](https://github.com/yortus/asyncawait)

<br/><br/>

## ![✔] 3.12 Используйте стрелочные функции (=>)

**TL;DR:** Хотя рекомендуется использовать async-await и избегать параметров функций при работе со старыми API, которые принимают обещания или обратные вызовы -- стрелочные функции делают структуру кода более компактной и поддерживают лексический контекст корневой функции (т.к. `this`)

**Иначе:** Более длинный код (в функциях ES5) более подвержен ошибкам и неудобен для чтения.

🔗 [**Подробнее: It’s Time to Embrace Arrow Functions**](https://medium.com/javascript-scene/familiarity-bias-is-holding-you-back-its-time-to-embrace-arrow-functions-3d37e1a9bb75)

<br/><br/><br/>

<p align="right"><a href="#оглавление">⬆ К началу</a></p>

# `4. Тестирование и общие методы контроля качества`

## ![✔] 4.1 At the very least, write API (component) testing

**TL;DR:** Most projects just don't have any automated testing due to short timetables or often the 'testing project' ran out of control and was abandoned. For that reason, prioritize and start with API testing which is the easiest way to write and provides more coverage than unit testing (you may even craft API tests without code using tools like [Postman](https://www.getpostman.com/). Afterward, should you have more resources and time, continue with advanced test types like unit testing, DB testing, performance testing, etc

**Иначе:** You may spend long days on writing unit tests to find out that you got only 20% system coverage

<br/><br/>

## ![✔] 4.2 Include 3 parts in each test name

**TL;DR:** Make the test speak at the requirements level so it's self explanatory also to QA engineers and developers who are not familiar with the code internals. State in the test name what is being tested (unit under test), under what circumstances and what is the expected result

**Иначе:** A deployment just failed, a test named “Add product” failed. Does this tell you what exactly is malfunctioning?

🔗 [**Подробнее: Include 3 parts in each test name**](/sections/testingandquality/3-parts-in-name.md)

<br/><br/>

## ![✔] 4.3 Detect code issues with a linter

**TL;DR:** Use a code linter to check basic quality and detect anti-patterns early. Run it before any test and add it as a pre-commit git-hook to minimize the time needed to review and correct any issue. Also check [Section 3](https://github.com/i0natan/nodebestpractices#3-code-style-practices) on Code Style Practices

**Иначе:** You may let pass some anti-pattern and possible vulnerable code to your production environment.

<br/><br/>

## ![✔] 4.4 Avoid global test fixtures and seeds, add data per-test

**TL;DR:** To prevent tests coupling and easily reason about the test flow, each test should add and act on its own set of DB rows. Whenever a test needs to pull or assume the existence of some DB data - it must explicitly add that data and avoid mutating any other records

**Иначе:** Consider a scenario where deployment is aborted due to failing tests, team is now going to spend precious investigation time that ends in a sad conclusion: the system works well, the tests however interfere with each other and break the build

🔗 [**Подробнее: Avoid global test fixtures**](/sections/testingandquality/avoid-global-test-fixture.md)

<br/><br/>



## ![✔] 4.5 Constantly inspect for vulnerable dependencies

**TL;DR:** Even the most reputable dependencies such as Express have known vulnerabilities. This can get easily tamed using community and commercial tools such as 🔗 [npm audit](https://docs.npmjs.com/cli/audit) and 🔗 [snyk.io](https://snyk.io) that can be invoked from your CI on every build

**Иначе:** Keeping your code clean from vulnerabilities without dedicated tools will require to constantly follow online publications about new threats. Quite tedious

<br/><br/>

## ![✔] 4.6 Tag your tests

**TL;DR:** Different tests must run on different scenarios: quick smoke, IO-less, tests should run when a developer saves or commits a file, full end-to-end tests usually run when a new pull request is submitted, etc. This can be achieved by tagging tests with keywords like #cold #api #sanity so you can grep with your testing harness and invoke the desired subset. For example, this is how you would invoke only the sanity test group with [Mocha](https://mochajs.org/): mocha --grep 'sanity'

**Иначе:** Running all the tests, including tests that perform dozens of DB queries, any time a developer makes a small change can be extremely slow and keeps developers away from running tests

<br/><br/>

## ![✔] 4.7 Check your test coverage, it helps to identify wrong test patterns

**TL;DR:** Code coverage tools like [Istanbul/NYC ](https://github.com/gotwarlost/istanbul)are great for 3 reasons: it comes for free (no effort is required to benefit this reports), it helps to identify a decrease in testing coverage, and last but not least it highlights testing mismatches: by looking at colored code coverage reports you may notice, for example, code areas that are never tested like catch clauses (meaning that tests only invoke the happy paths and not how the app behaves on errors). Set it to fail builds if the coverage falls under a certain threshold

**Иначе:** There won't be any automated metric telling you when a large portion of your code is not covered by testing

<br/><br/>

## ![✔] 4.8 Inspect for outdated packages

**TL;DR:** Use your preferred tool (e.g. 'npm outdated' or [npm-check-updates](https://www.npmjs.com/package/npm-check-updates) to detect installed packages which are outdated, inject this check into your CI pipeline and even make a build fail in a severe scenario. For example, a severe scenario might be when an installed package is 5 patch commits behind (e.g. local version is 1.3.1 and repository version is 1.3.8) or it is tagged as deprecated by its author - kill the build and prevent deploying this version

**Иначе:** Your production will run packages that have been explicitly tagged by their author as risky

<br/><br/>

## ![✔] 4.9 Use docker-compose for e2e testing

**TL;DR:** End to end (e2e) testing which includes live data used to be the weakest link of the CI process as it depends on multiple heavy services like DB. Docker-compose turns this problem into a breeze by crafting production-like environment using a simple text file and easy commands. It allows crafting all the dependent services, DB and isolated network for e2e testing. Last but not least, it can keep a stateless environment that is invoked before each test suite and dies right after

**Иначе:** Without docker-compose teams must maintain a testing DB for each testing environment including developers' machines, keep all those DBs in sync so test results won't vary across environments

<br/><br/>

## ![✔] 4.10 Refactor regularly using static analysis tools

**TL;DR:** Using static analysis tools helps by giving objective ways to improve code quality and keeps your code maintainable. You can add static analysis tools to your CI build to fail when it finds code smells. Its main selling points over plain linting are the ability to inspect quality in the context of multiple files (e.g. detect duplications), perform advanced analysis (e.g. code complexity) and follow the history and progress of code issues. Two examples of tools you can use are [Sonarqube](https://www.sonarqube.org/) (2,600+ [stars](https://github.com/SonarSource/sonarqube)) and [Code Climate](https://codeclimate.com/) (1,500+ [stars](https://github.com/codeclimate/codeclimate)).

**Иначе:** With poor code quality, bugs and performance will always be an issue that no shiny new library or state of the art features can fix

🔗 [**Подробнее: Refactoring!**](/sections/testingandquality/refactoring.md)

<br/><br/>

## ![✔] 4.11 Carefully choose your CI platform (Jenkins vs CircleCI vs Travis vs Rest of the world)

**TL;DR:** Your continuous integration platform (CICD) will host all the quality tools (e.g test, lint) so it should come with a vibrant ecosystem of plugins. [Jenkins](https://jenkins.io/) used to be the default for many projects as it has the biggest community along with a very powerful platform at the price of complex setup that demands a steep learning curve. Nowadays, it has become much easier to set up a CI solution using SaaS tools like [CircleCI](https://circleci.com) and others. These tools allow crafting a flexible CI pipeline without the burden of managing the whole infrastructure. Eventually, it's a trade-off between robustness and speed - choose your side carefully

**Иначе:** Choosing some niche vendor might get you blocked once you need some advanced customization. On the other hand, going with Jenkins might burn precious time on infrastructure setup

🔗 [**Подробнее: Choosing CI platform**](/sections/testingandquality/citools.md)

<br/><br/><br/>


<p align="right"><a href="#оглавление">⬆ К началу</a></p>

# `5. Переход к производственным практикам`

## ![✔] 5.1. Monitoring!

**TL;DR:** Monitoring is a game of finding out issues before customers do – obviously this should be assigned unprecedented importance. The market is overwhelmed with offers thus consider starting with defining the basic metrics you must follow (my suggestions inside), then go over additional fancy features and choose the solution that ticks all boxes. Click ‘The Gist’ below for an overview of the solutions

**Иначе:** Failure === disappointed customers. Simple

🔗 [**Подробнее: Monitoring!**](/sections/production/monitoring.md)

<br/><br/>

## ![✔] 5.2. Increase transparency using smart logging

**TL;DR:** Logs can be a dumb warehouse of debug statements or the enabler of a beautiful dashboard that tells the story of your app. Plan your logging platform from day 1: how logs are collected, stored and analyzed to ensure that the desired information (e.g. error rate, following an entire transaction through services and servers, etc) can really be extracted

**Иначе:** You end up with a black box that is hard to reason about, then you start re-writing all logging statements to add additional information

🔗 [**Подробнее: Increase transparency using smart logging**](/sections/production/smartlogging.md)

<br/><br/>

## ![✔] 5.3. Delegate anything possible (e.g. gzip, SSL) to a reverse proxy

**TL;DR:** Node is awfully bad at doing CPU intensive tasks like gzipping, SSL termination, etc. You should use ‘real’ middleware services like nginx, HAproxy or cloud vendor services instead

**Иначе:** Your poor single thread will stay busy doing infrastructural tasks instead of dealing with your application core and performance will degrade accordingly

🔗 [**Подробнее: Delegate anything possible (e.g. gzip, SSL) to a reverse proxy**](/sections/production/delegatetoproxy.md)

<br/><br/>

## ![✔] 5.4. Lock dependencies

**TL;DR:** Your code must be identical across all environments, but amazingly npm lets dependencies drift across environments by default – when you install packages at various environments it tries to fetch packages’ latest patch version. Overcome this by using npm config files, .npmrc, that tell each environment to save the exact (not the latest) version of each package. Alternatively, for finer grained control use `npm shrinkwrap`. \*Update: as of NPM5, dependencies are locked by default. The new package manager in town, Yarn, also got us covered by default

**Иначе:** QA will thoroughly test the code and approve a version that will behave differently in production. Even worse, different servers in the same production cluster might run different code

🔗 [**Подробнее: Lock dependencies**](/sections/production/lockdependencies.md)

<br/><br/>

## ![✔] 5.5. Guard process uptime using the right tool

**TL;DR:** The process must go on and get restarted upon failures. For simple scenarios, process management tools like PM2 might be enough but in today's ‘dockerized’ world, cluster management tools should be considered as well

**Иначе:** Running dozens of instances without a clear strategy and too many tools together (cluster management, docker, PM2) might lead to DevOps chaos

🔗 [**Подробнее: Guard process uptime using the right tool**](/sections/production/guardprocess.md)

<br/><br/>

## ![✔] 5.6. Utilize all CPU cores

**TL;DR:** At its basic form, a Node app runs on a single CPU core while all others are left idling. It’s your duty to replicate the Node process and utilize all CPUs – For small-medium apps you may use Node Cluster or PM2. For a larger app consider replicating the process using some Docker cluster (e.g. K8S, ECS) or deployment scripts that are based on Linux init system (e.g. systemd)

**Иначе:** Your app will likely utilize only 25% of its available resources(!) or even less. Note that a typical server has 4 CPU cores or more, naive deployment of Node.js utilizes only 1 (even using PaaS services like AWS beanstalk!)

🔗 [**Подробнее: Utilize all CPU cores**](/sections/production/utilizecpu.md)

<br/><br/>

## ![✔] 5.7. Create a ‘maintenance endpoint’

**TL;DR:** Expose a set of system-related information, like memory usage and REPL, etc in a secured API. Although it’s highly recommended to rely on standard and battle-tests tools, some valuable information and operations are easier done using code

**Иначе:** You’ll find that you’re performing many “diagnostic deploys” – shipping code to production only to extract some information for diagnostic purposes

🔗 [**Подробнее: Create a ‘maintenance endpoint’**](/sections/production/createmaintenanceendpoint.md)

<br/><br/>

## ![✔] 5.8. Discover errors and downtime using APM products

**TL;DR:** Application monitoring and performance products (a.k.a APM) proactively gauge codebase and API so they can auto-magically go beyond traditional monitoring and measure the overall user-experience across services and tiers. For example, some APM products can highlight a transaction that loads too slow on the end-users side while suggesting the root cause

**Иначе:** You might spend great effort on measuring API performance and downtimes, probably you’ll never be aware which is your slowest code parts under real-world scenario and how these affect the UX

🔗 [**Подробнее: Discover errors and downtime using APM products**](/sections/production/apmproducts.md)

<br/><br/>

## ![✔] 5.9. Make your code production-ready

**TL;DR:** Code with the end in mind, plan for production from day 1. This sounds a bit vague so I’ve compiled a few development tips that are closely related to production maintenance (click Gist below)

**Иначе:** A world champion IT/DevOps guy won’t save a system that is badly written

🔗 [**Подробнее: Make your code production-ready**](/sections/production/productioncode.md)

<br/><br/>

## ![✔] 5.10. Measure and guard the memory usage

**TL;DR:** Node.js has controversial relationships with memory: the v8 engine has soft limits on memory usage (1.4GB) and there are known paths to leak memory in Node’s code – thus watching Node’s process memory is a must. In small apps, you may gauge memory periodically using shell commands but in medium-large apps consider baking your memory watch into a robust monitoring system

**Иначе:** Your process memory might leak a hundred megabytes a day like how it happened at [Walmart](https://www.joyent.com/blog/walmart-node-js-memory-leak)

🔗 [**Подробнее: Measure and guard the memory usage**](/sections/production/measurememory.md)

<br/><br/>

## ![✔] 5.11. Get your frontend assets out of Node

**TL;DR:** Serve frontend content using dedicated middleware (nginx, S3, CDN) because Node performance really gets hurt when dealing with many static files due to its single-threaded model

**Иначе:** Your single Node thread will be busy streaming hundreds of html/images/angular/react files instead of allocating all its resources for the task it was born for – serving dynamic content

🔗 [**Подробнее: Get your frontend assets out of Node**](/sections/production/frontendout.md)

<br/><br/>

## ![✔] 5.12. Be stateless, kill your servers almost every day

**TL;DR:** Store any type of data (e.g. user sessions, cache, uploaded files) within external data stores. Consider ‘killing’ your servers periodically or use ‘serverless’ platform (e.g. AWS Lambda) that explicitly enforces a stateless behavior

**Иначе:** Failure at a given server will result in application downtime instead of just killing a faulty machine. Moreover, scaling-out elasticity will get more challenging due to the reliance on a specific server

🔗 [**Подробнее: Be stateless, kill your Servers almost every day**](/sections/production/bestateless.md)

<br/><br/>

## ![✔] 5.13. Use tools that automatically detect vulnerabilities

**TL;DR:** Even the most reputable dependencies such as Express have known vulnerabilities (from time to time) that can put a system at risk. This can be easily tamed using community and commercial tools that constantly check for vulnerabilities and warn (locally or at GitHub), some can even patch them immediately

**Иначе:** Keeping your code clean from vulnerabilities without dedicated tools will require you to constantly follow online publications about new threats. Quite tedious

🔗 [**Подробнее: Use tools that automatically detect vulnerabilities**](/sections/production/detectvulnerabilities.md)

<br/><br/>

## ![✔] 5.14. Assign a transaction id to each log statement

**TL;DR:** Assign the same identifier, transaction-id: {some value}, to each log entry within a single request. Then when inspecting errors in logs, easily conclude what happened before and after. Unfortunately, this is not easy to achieve in Node due to its async nature, see Пример кодаs inside

**Иначе:** Looking at a production error log without the context – what happened before – makes it much harder and slower to reason about the issue

🔗 [**Подробнее: Assign ‘TransactionId’ to each log statement**](/sections/production/assigntransactionid.md)

<br/><br/>

## ![✔] 5.15. Set NODE_ENV=production

**TL;DR:** Set the environment variable NODE_ENV to ‘production’ or ‘development’ to flag whether production optimizations should get activated – many npm packages determine the current environment and optimize their code for production

**Иначе:** Omitting this simple property might greatly degrade performance. For example, when using Express for server-side rendering omitting `NODE_ENV` makes it slower by a factor of three!

🔗 [**Подробнее: Set NODE_ENV=production**](/sections/production/setnodeenv.md)

<br/><br/>

## ![✔] 5.16. Design automated, atomic and zero-downtime deployments

**TL;DR:** Research shows that teams who perform many deployments lower the probability of severe production issues. Fast and automated deployments that don’t require risky manual steps and service downtime significantly improve the deployment process. You should probably achieve this using Docker combined with CI tools as they became the industry standard for streamlined deployment

**Иначе:** Long deployments -> production downtime & human-related error -> team unconfident in making deployment -> fewer deployments and features

<br/><br/>

## ![✔] 5.17. Use an LTS release of Node.js

**TL;DR:** Ensure you are using an LTS version of Node.js to receive critical bug fixes, security updates and performance improvements

**Иначе:** Newly discovered bugs or vulnerabilities could be used to exploit an application running in production, and your application may become unsupported by various modules and harder to maintain

🔗 [**Подробнее: Use an LTS release of Node.js**](/sections/production/LTSrelease.md)

<br/><br/>

## ![✔] 5.18. Don't route logs within the app

**TL;DR:** Log destinations should not be hard-coded by developers within the application code, but instead should be defined by the execution environment the application runs in. Developers should write logs to `stdout` using a logger utility and then let the execution environment (container, server, etc.) pipe the `stdout` stream to the appropriate destination (i.e. Splunk, Graylog, ElasticSearch, etc.).

**Иначе:** Application handling log routing === hard to scale, loss of logs, poor separation of concerns

🔗 [**Подробнее: Log Routing**](/sections/production/logrouting.md)

<br/><br/><br/>

<p align="right"><a href="#оглавление">⬆ К началу</a></p>

# `6. Практики безопасности`

<div align="center">
<img src="https://img.shields.io/badge/OWASP%20Threats-Top%2010-green.svg" alt="54 items"/>
</div>

## ![✔] 6.1. Embrace linter security rules

<a href="https://www.owasp.org/index.php/Top_10-2017_A1-Injection" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A1:Injection%20-green.svg" alt=""/></a> <a href="https://www.owasp.org/index.php/Top_10-2017_A7-Cross-Site_Scripting_(XSS)" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20XSS%20-green.svg" alt=""/></a>

**TL;DR:** Make use of security-related linter plugins such as [eslint-plugin-security](https://github.com/nodesecurity/eslint-plugin-security) to catch security vulnerabilities and issues as early as possible, preferably while they're being coded. This can help catching security weaknesses like using eval, invoking a child process or importing a module with a string literal (e.g. user input). Click 'Read more' below to see Пример кодаs that will get caught by a security linter

**Иначе:** What could have been a straightforward security weakness during development becomes a major issue in production. Also, the project may not follow consistent code security practices, leading to vulnerabilities being introduced, or sensitive secrets committed into remote repositories

🔗 [**Подробнее: Lint rules**](/sections/security/lintrules.md)

<br/><br/>

## ![✔] 6.2. Limit concurrent requests using a middleware

<a href="https://www.owasp.org/index.php/Denial_of_Service" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20DDOS%20-green.svg" alt=""/></a>

**TL;DR:** DOS attacks are very popular and relatively easy to conduct. Implement rate limiting using an external service such as cloud load balancers, cloud firewalls, nginx, [rate-limiter-flexible](https://www.npmjs.com/package/rate-limiter-flexible) package, or (for smaller and less critical apps) a rate-limiting middleware (e.g. [express-rate-limit](https://www.npmjs.com/package/express-rate-limit))

**Иначе:** An application could be subject to an attack resulting in a denial of service where real users receive a degraded or unavailable service.

🔗 [**Подробнее: Implement rate limiting**](/sections/security/limitrequests.md)

<br/><br/>

## ![✔] 6.3 Extract secrets from config files or use packages to encrypt them

<a href="https://www.owasp.org/index.php/Top_10-2017_A6-Security_Misconfiguration" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A6:Security%20Misconfiguration%20-green.svg" alt=""/></a> <a href="https://www.owasp.org/index.php/Top_10-2017_A3-Sensitive_Data_Exposure" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A3:Sensitive%20Data%20Exposure%20-green.svg" alt=""/></a>

**TL;DR:** Never store plain-text secrets in configuration files or source code. Instead, make use of secret-management systems like Vault products, Kubernetes/Docker Secrets, or using environment variables. As a last resort, secrets stored in source control must be encrypted and managed (rolling keys, expiring, auditing, etc). Make use of pre-commit/push hooks to prevent committing secrets accidentally

**Иначе:** Source control, even for private repositories, can mistakenly be made public, at which point all secrets are exposed. Access to source control for an external party will inadvertently provide access to related systems (databases, apis, services, etc).

🔗 [**Подробнее: Secret management**](/sections/security/secretmanagement.md)

<br/><br/>

## ![✔] 6.4. Prevent query injection vulnerabilities with ORM/ODM libraries

<a href="https://www.owasp.org/index.php/Top_10-2017_A1-Injection" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A1:Injection%20-green.svg" alt=""/></a>

**TL;DR:** To prevent SQL/NoSQL injection and other malicious attacks, always make use of an ORM/ODM or a database library that escapes data or supports named or indexed parameterized queries, and takes care of validating user input for expected types. Never just use JavaScript template strings or string concatenation to inject values into queries as this opens your application to a wide spectrum of vulnerabilities. All the reputable Node.js data access libraries (e.g. [Sequelize](https://github.com/sequelize/sequelize), [Knex](https://github.com/tgriesser/knex), [mongoose](https://github.com/Automattic/mongoose)) have built-in protection against injection attacks.

**Иначе:** Unvalidated or unsanitized user input could lead to operator injection when working with MongoDB for NoSQL, and not using a proper sanitization system or ORM will easily allow SQL injection attacks, creating a giant vulnerability.

🔗 [**Подробнее: Query injection prevention using ORM/ODM libraries**](/sections/security/ormodmusage.md)

<br/><br/>

## ![✔] 6.5. Collection of generic security best practices

**TL;DR:** This is a collection of security advice that is not related directly to Node.js - the Node implementation is not much different than any other language. Click read more to skim through.

🔗 [**Подробнее: Common security best practices**](/sections/security/commonsecuritybestpractices.md)

<br/><br/>

## ![✔] 6.6. Adjust the HTTP response headers for enhanced security

<a href="https://www.owasp.org/index.php/Top_10-2017_A6-Security_Misconfiguration" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A6:Security%20Misconfiguration%20-green.svg" alt=""/></a>

**TL;DR:** Your application should be using secure headers to prevent attackers from using common attacks like cross-site scripting (XSS), clickjacking and other malicious attacks. These can be configured easily using modules like [helmet](https://www.npmjs.com/package/helmet).

**Иначе:** Attackers could perform direct attacks on your application's users, leading to huge security vulnerabilities

🔗 [**Подробнее: Using secure headers in your application**](/sections/security/secureheaders.md)

<br/><br/>

## ![✔] 6.7. Constantly and automatically inspect for vulnerable dependencies

<a href="https://www.owasp.org/index.php/Top_10-2017_A9-Using_Components_with_Known_Vulnerabilities" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A9:Known%20Vulnerabilities%20-green.svg" alt=""/></a>

**TL;DR:** With the npm ecosystem it is common to have many dependencies for a project. Dependencies should always be kept in check as new vulnerabilities are found. Use tools like [npm audit](https://docs.npmjs.com/cli/audit) or [snyk](https://snyk.io/) to track, monitor and patch vulnerable dependencies. Integrate these tools with your CI setup so you catch a vulnerable dependency before it makes it to production.

**Иначе:** An attacker could detect your web framework and attack all its known vulnerabilities.

🔗 [**Подробнее: Dependency security**](/sections/security/dependencysecurity.md)

<br/><br/>

## ![✔] 6.8. Avoid using the Node.js crypto library for handling passwords, use Bcrypt

<a href="https://www.owasp.org/index.php/Top_10-2017_A2-Broken_Authentication" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A9:Broken%20Authentication%20-green.svg" alt=""/></a>

**TL;DR:** Passwords or secrets (API keys) should be stored using a secure hash + salt function like `bcrypt`, that should be a preferred choice over its JavaScript implementation due to performance and security reasons.

**Иначе:** Passwords or secrets that are persisted without using a secure function are vulnerable to brute forcing and dictionary attacks that will lead to their disclosure eventually.

🔗 [**Подробнее: Use Bcrypt**](/sections/security/bcryptpasswords.md)

<br/><br/>

## ![✔] 6.9. Escape HTML, JS and CSS output

<a href="https://www.owasp.org/index.php/Top_10-2017_A7-Cross-Site_Scripting_(XSS)" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A7:XSS%20-green.svg" alt=""/></a>

**TL;DR:** Untrusted data that is sent down to the browser might get executed instead of just being displayed, this is commonly referred as a cross-site-scripting (XSS) attack. Mitigate this by using dedicated libraries that explicitly mark the data as pure content that should never get executed (i.e. encoding, escaping)

**Иначе:** An attacker might store malicious JavaScript code in your DB which will then be sent as-is to the poor clients

🔗 [**Подробнее: Escape output**](/sections/security/escape-output.md)

<br/><br/>

## ![✔] 6.10. Validate incoming JSON schemas

<a href="https://www.owasp.org/index.php/Top_10-2017_A7-Cross-Site_Scripting_(XSS)" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A7: XSS%20-green.svg" alt=""/></a> <a href="https://www.owasp.org/index.php/Top_10-2017_A8-Insecure_Deserialization" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A8:Insecured%20Deserialization%20-green.svg" alt=""/></a>

**TL;DR:** Validate the incoming requests' body payload and ensure it meets expectations, fail fast if it doesn't. To avoid tedious validation coding within each route you may use lightweight JSON-based validation schemas such as [jsonschema](https://www.npmjs.com/package/jsonschema) or [joi](https://www.npmjs.com/package/joi)

**Иначе:** Your generosity and permissive approach greatly increases the attack surface and encourages the attacker to try out many inputs until they find some combination to crash the application

🔗 [**Подробнее: Validate incoming JSON schemas**](/sections/security/validation.md)

<br/><br/>

## ![✔] 6.11. Support blacklisting JWTs

<a href="https://www.owasp.org/index.php/Top_10-2017_A2-Broken_Authentication" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A9:Broken%20Authentication%20-green.svg" alt=""/></a>

**TL;DR:** When using JSON Web Tokens (for example, with [Passport.js](https://github.com/jaredhanson/passport)), by default there's no mechanism to revoke access from issued tokens. Once you discover some malicious user activity, there's no way to stop them from accessing the system as long as they hold a valid token. Mitigate this by implementing a blacklist of untrusted tokens that are validated on each request.

**Иначе:** Expired, or misplaced tokens could be used maliciously by a third party to access an application and impersonate the owner of the token.

🔗 [**Подробнее: Blacklist JSON Web Tokens**](/sections/security/expirejwt.md)

<br/><br/>

## ![✔] 6.12. Prevent brute-force attacks against authorization

<a href="https://www.owasp.org/index.php/Top_10-2017_A2-Broken_Authentication" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A9:Broken%20Authentication%20-green.svg" alt=""/></a>

**TL;DR:** A simple and powerful technique is to limit authorization attempts using two metrics:
           
1. The first is number of consecutive failed attempts by the same user unique ID/name and IP address.
2. The second is number of failed attempts from an IP address over some long period of time. For example, block an IP address if it makes 100 failed attempts in one day.

**Иначе:** An attacker can issue unlimited automated password attempts to gain access to privileged accounts on an application

🔗 [**Подробнее: Login rate limiting**](/sections/security/login-rate-limit.md)

<br/><br/>

## ![✔] 6.13. Run Node.js as non-root user

<a href="https://www.owasp.org/index.php/Top_10-2017_A5-Broken_Access_Control" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A5:Broken%20Access%20Access%20Control-green.svg" alt=""/></a>

**TL;DR:** There is a common scenario where Node.js runs as a root user with unlimited permissions. For example, this is the default behaviour in Docker containers. It's recommended to create a non-root user and either bake it into the Docker image (examples given below) or run the process on this user's behalf by invoking the container with the flag "-u username"

**Иначе:** An attacker who manages to run a script on the server gets unlimited power over the local machine (e.g. change iptable and re-route traffic to his server)

🔗 [**Подробнее: Run Node.js as non-root user**](/sections/security/non-root-user.md)

<br/><br/>

## ![✔] 6.14. Limit payload size using a reverse-proxy or a middleware

<a href="https://www.owasp.org/index.php/Top_10-2017_A8-Insecure_Deserialization" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A8:Insecured%20Deserialization%20-green.svg" alt=""/></a> <a href="https://www.owasp.org/index.php/Top_10-2017_A1-Injection" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20DDOS%20-green.svg" alt=""/></a>

**TL;DR:** The bigger the body payload is, the harder your single thread works in processing it. This is an opportunity for attackers to bring servers to their knees without tremendous amount of requests (DOS/DDOS attacks). Mitigate this limiting the body size of incoming requests on the edge (e.g. firewall, ELB) or by configuring [express body parser](https://github.com/expressjs/body-parser) to accept only small-size payloads

**Иначе:** Your application will have to deal with large requests, unable to process the other important work it has to accomplish, leading to performance implications and vulnerability towards DOS attacks

🔗 [**Подробнее: Limit payload size**](/sections/security/requestpayloadsizelimit.md)

<br/><br/>

## ![✔] 6.15. Avoid JavaScript eval statements

<a href="https://www.owasp.org/index.php/Top_10-2017_A7-Cross-Site_Scripting_(XSS)" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A7:XSS%20-green.svg" alt=""/></a> <a href="https://www.owasp.org/index.php/Top_10-2017_A1-Injection" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A1:Injection%20-green.svg" alt=""/></a> <a href="https://www.owasp.org/index.php/Top_10-2017_A4-XML_External_Entities_(XXE)" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A4:External%20Entities%20-green.svg" alt=""/></a>

**TL;DR:** `eval` is evil as it allows executing custom JavaScript code during run time. This is not just a performance concern but also an important security concern due to malicious JavaScript code that may be sourced from user input. Another language feature that should be avoided is `new Function` constructor. `setTimeout` and `setInterval` should never be passed dynamic JavaScript code either.

**Иначе:** Malicious JavaScript code finds a way into text passed into `eval` or other real-time evaluating JavaScript language functions, and will gain complete access to JavaScript permissions on the page. This vulnerability is often manifested as an XSS attack.

🔗 [**Подробнее: Avoid JavaScript eval statements**](/sections/security/avoideval.md)

<br/><br/>

## ![✔] 6.16. Prevent evil RegEx from overloading your single thread execution

<a href="https://www.owasp.org/index.php/Denial_of_Service" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20DDOS%20-green.svg" alt=""/></a>

**TL;DR:** Regular Expressions, while being handy, pose a real threat to JavaScript applications at large, and the Node.js platform in particular. A user input for text to match might require an outstanding amount of CPU cycles to process. RegEx processing might be inefficient to an extent that a single request that validates 10 words can block the entire event loop for 6 seconds and set the CPU on 🔥. For that reason, prefer third-party validation packages like [validator.js](https://github.com/chriso/validator.js) instead of writing your own Regex patterns, or make use of [safe-regex](https://github.com/substack/safe-regex) to detect vulnerable regex patterns

**Иначе:** Poorly written regexes could be susceptible to Regular Expression DoS attacks that will block the event loop completely. For example, the popular `moment` package was found vulnerable with malicious RegEx usage in November of 2017

🔗 [**Подробнее: Prevent malicious RegEx**](/sections/security/regex.md)

<br/><br/>

## ![✔] 6.17. Avoid module loading using a variable

<a href="https://www.owasp.org/index.php/Top_10-2017_A7-Cross-Site_Scripting_(XSS)" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A7:XSS%20-green.svg" alt=""/></a> <a href="https://www.owasp.org/index.php/Top_10-2017_A1-Injection" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A1:Injection%20-green.svg" alt=""/></a> <a href="https://www.owasp.org/index.php/Top_10-2017_A4-XML_External_Entities_(XXE)" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A4:External%20Entities%20-green.svg" alt=""/></a>

**TL;DR:** Avoid requiring/importing another file with a path that was given as parameter due to the concern that it could have originated from user input. This rule can be extended for accessing files in general (i.e. `fs.readFile()`) or other sensitive resource access with dynamic variables originating from user input. [Eslint-plugin-security](https://www.npmjs.com/package/eslint-plugin-security) linter can catch such patterns and warn early enough

**Иначе:** Malicious user input could find its way to a parameter that is used to require tampered files, for example, a previously uploaded file on the filesystem, or access already existing system files.

🔗 [**Подробнее: Safe module loading**](/sections/security/safemoduleloading.md)

<br/><br/>

## ![✔] 6.18. Run unsafe code in a sandbox

<a href="https://www.owasp.org/index.php/Top_10-2017_A7-Cross-Site_Scripting_(XSS)" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A7:XSS%20-green.svg" alt=""/></a> <a href="https://www.owasp.org/index.php/Top_10-2017_A1-Injection" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A1:Injection%20-green.svg" alt=""/></a> <a href="https://www.owasp.org/index.php/Top_10-2017_A4-XML_External_Entities_(XXE)" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A4:External%20Entities%20-green.svg" alt=""/></a>

**TL;DR:** When tasked to run external code that is given at run-time (e.g. plugin), use any sort of 'sandbox' execution environment that isolates and guards the main code against the plugin. This can be achieved using a dedicated process (e.g. `cluster.fork()`), serverless environment or dedicated npm packages that act as a sandbox

**Иначе:** A plugin can attack through an endless variety of options like infinite loops, memory overloading, and access to sensitive process environment variables

🔗 [**Подробнее: Run unsafe code in a sandbox**](/sections/security/sandbox.md)

<br/><br/>

## ![✔] 6.19. Take extra care when working with child processes

<a href="https://www.owasp.org/index.php/Top_10-2017_A7-Cross-Site_Scripting_(XSS)" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A7:XSS%20-green.svg" alt=""/></a> <a href="https://www.owasp.org/index.php/Top_10-2017_A1-Injection" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A1:Injection%20-green.svg" alt=""/></a> <a href="https://www.owasp.org/index.php/Top_10-2017_A4-XML_External_Entities_(XXE)" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A4:External%20Entities%20-green.svg" alt=""/></a>

**TL;DR:** Avoid using child processes when possible and validate and sanitize input to mitigate shell injection attacks if you still have to. Prefer using `child_process.execFile` which by definition will only execute a single command with a set of attributes and will not allow shell parameter expansion.

**Иначе:** Naive use of child processes could result in remote command execution or shell injection attacks due to malicious user input passed to an unsanitized system command.

🔗 [**Подробнее: Be cautious when working with child processes**](/sections/security/childprocesses.md)

<br/><br/>

## ![✔] 6.20. Hide error details from clients

<a href="https://www.owasp.org/index.php/Top_10-2017_A6-Security_Misconfiguration" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A6:Security%20Misconfiguration%20-green.svg" alt=""/></a>

**TL;DR:** An integrated express error handler hides the error details by default. However, great are the chances that you implement your own error handling logic with custom Error objects (considered by many as a best practice). If you do so, ensure not to return the entire Error object to the client, which might contain some sensitive application details

**Иначе:** Sensitive application details such as server file paths, third party modules in use, and other internal workflows of the application which could be exploited by an attacker, could be leaked from information found in a stack trace

🔗 [**Подробнее: Hide error details from client**](/sections/security/hideerrors.md)

<br/><br/>

## ![✔] 6.21. Configure 2FA for npm or Yarn

<a href="https://www.owasp.org/index.php/Top_10-2017_A6-Security_Misconfiguration" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A6:Security%20Misconfiguration%20-green.svg" alt=""/></a>

**TL;DR:** Any step in the development chain should be protected with MFA (multi-factor authentication), npm/Yarn are a sweet opportunity for attackers who can get their hands on some developer's password. Using developer credentials, attackers can inject malicious code into libraries that are widely installed across projects and services. Maybe even across the web if published in public. Enabling 2-factor-authentication in npm leaves almost zero chances for attackers to alter your package code.

**Иначе:** [Have you heard about the eslint developer who's password was hijacked?](https://medium.com/@oprearocks/eslint-backdoor-what-it-is-and-how-to-fix-the-issue-221f58f1a8c8)

<br/><br/>

## ![✔] 6.22. Modify session middleware settings

<a href="https://www.owasp.org/index.php/Top_10-2017_A6-Security_Misconfiguration" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A6:Security%20Misconfiguration%20-green.svg" alt=""/></a>

**TL;DR:** Each web framework and technology has its known weaknesses - telling an attacker which web framework we use is a great help for them. Using the default settings for session middlewares can expose your app to module- and framework-specific hijacking attacks in a similar way to the `X-Powered-By` header. Try hiding anything that identifies and reveals your tech stack (E.g. Node.js, express)

**Иначе:** Cookies could be sent over insecure connections, and an attacker might use session identification to identify the underlying framework of the web application, as well as module-specific vulnerabilities

🔗 [**Подробнее: Cookie and session security**](/sections/security/sessions.md)

<br/><br/>

## ![✔] 6.23. Avoid DOS attacks by explicitly setting when a process should crash

<a href="https://www.owasp.org/index.php/Denial_of_Service" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20DDOS%20-green.svg" alt=""/></a>

**TL;DR:** The Node process will crash when errors are not handled. Many best practices even recommend to exit even though an error was caught and got handled. Express, for example, will crash on any asynchronous error - unless you wrap routes with a catch clause. This opens a very sweet attack spot for attackers who recognize what input makes the process crash and repeatedly send the same request. There's no instant remedy for this but a few techniques can mitigate the pain: Alert with critical severity anytime a process crashes due to an unhandled error, validate the input and avoid crashing the process due to invalid user input, wrap all routes with a catch and consider not to crash when an error originated within a request (as opposed to what happens globally)

**Иначе:** This is just an educated guess: given many Node.js applications, if we try passing an empty JSON body to all POST requests - a handful of applications will crash. At that point, we can just repeat sending the same request to take down the applications with ease

<br/><br/>

## ![✔] 6.24. Prevent unsafe redirects

<a href="https://www.owasp.org/index.php/Top_10-2017_A1-Injection" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A1:Injection%20-green.svg" alt=""/></a>

**TL;DR:** Redirects that do not validate user input can enable attackers to launch phishing scams, steal user credentials, and perform other malicious actions.

**Иначе:** If an attacker discovers that you are not validating external, user-supplied input, they may exploit this vulnerability by posting specially-crafted links on forums, social media, and other public places to get users to click it.

🔗 [**Подробнее: Prevent unsafe redirects**](/sections/security/saferedirects.md)

<br/><br/>

## ![✔] 6.25. Avoid publishing secrets to the npm registry

<a href="https://www.owasp.org/index.php/Top_10-2017_A6-Security_Misconfiguration" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A6:Security%20Misconfiguration%20-green.svg" alt=""/></a>

**TL;DR:** Precautions should be taken to avoid the risk of accidentally publishing secrets to public npm registries. An `.npmignore` file can be used to blacklist specific files or folders, or the `files` array in `package.json` can act as a whitelist.

**Иначе:** Your project's API keys, passwords or other secrets are open to be abused by anyone who comes across them, which may result in financial loss, impersonation, and other risks.

🔗 [**Подробнее: Avoid publishing secrets**](/sections/security/avoid_publishing_secrets.md)
<br/><br/><br/>

<p align="right"><a href="#оглавление">⬆ К началу</a></p>

# `7. Практики эффективности`

## Our contributors are working on this section. [Would you like to join?](https://github.com/i0natan/nodebestpractices/issues/256)

## ![✔] 7.1. Prefer native JS methods over user-land utils like Lodash

 **TL;DR:** It's often more penalising to use utility libraries like `lodash` and `underscore` over native methods as it leads to unneeded dependencies and slower performance.
 Bear in mind that with the introduction of the new V8 engine alongside the new ES standards, native methods were improved in such a way that it's now about 50% more performant than utility libraries.

**Иначе:** You'll have to maintain less performant projects where you could have simply used what was **already** available or dealt with a few more lines in exchange of a few more files.

🔗 [**Подробнее: Native over user land utils**](/sections/performance/nativeoverutil.md)

<br/><br/><br/>

# Milestones

To maintain this guide and keep it up to date, we are constantly updating and improving the guidelines and best practices with the help of the community. You can follow our [milestones](https://github.com/i0natan/nodebestpractices/milestones) and join the working groups if you want to contribute to this project

<br/>

## Translations

All translations are contributed by the community. We will be happy to get any help with either completed, ongoing or new translations!

### Completed translations

- ![BR](/assets/flags/BR.png) [Brazilian Portuguese](/README.brazilian-portuguese.md) - Courtesy of [Marcelo Melo](https://github.com/marcelosdm)
- ![CN](/assets/flags/CN.png) [Chinese](README.chinese.md) - Courtesy of [Matt Jin](https://github.com/mattjin)

### Translations in progress

- ![FR](/assets/flags/FR.png) [French](https://github.com/gaspaonrocks/nodebestpractices/blob/french-translation/README.french.md) ([Discussion](https://github.com/i0natan/nodebestpractices/issues/129))
- ![HE](/assets/flags/HE.png) Hebrew ([Discussion](https://github.com/i0natan/nodebestpractices/issues/156))
- ![KR](/assets/flags/KR.png) [Korean](README.korean.md) - Courtesy of [Sangbeom Han](https://github.com/uronly14me) ([Discussion](https://github.com/i0natan/nodebestpractices/issues/94))
- ![RU](/assets/flags/RU.png) [Russian](https://github.com/i0natan/nodebestpractices/blob/russian-translation/README.russian.md) ([Discussion](https://github.com/i0natan/nodebestpractices/issues/105))
- ![ES](/assets/flags/ES.png) [Spanish](https://github.com/i0natan/nodebestpractices/blob/spanish-translation/README.spanish.md) ([Discussion](https://github.com/i0natan/nodebestpractices/issues/95))
- ![TR](/assets/flags/TR.png) Turkish ([Discussion](https://github.com/i0natan/nodebestpractices/issues/139))

<br/><br/>

## Steering Committee

Meet the steering committee members - the people who work together to provide guidance and future direction to the project. In addition, each member of the committee leads a project tracked under our [Github projects](https://github.com/i0natan/nodebestpractices/projects).

<img align="left" width="100" height="100" src="assets/images/members/yoni.png">

[Yoni Goldberg](https://github.com/i0natan)
<a href="https://twitter.com/goldbergyoni"><img src="assets/images/twitter-s.png" width="16" height="16"></img></a>
<a href="https://goldbergyoni.com"><img src="assets/images/www.png" width="16" height="16"></img></a>

Independent Node.js consultant who works with customers in USA, Europe, and Israel on building large scale scalable Node applications. Many of the best practices above were first published at [goldbergyoni.com](https://goldbergyoni.com). Reach Yoni at @goldbergyoni or me@goldbergyoni.com

<br/>

<img align="left" width="100" height="100" src="assets/images/members/bruno.png">

[Bruno Scheufler](https://github.com/BrunoScheufler)
<a href="https://brunoscheufler.com/"><img src="assets/images/www.png" width="16" height="16"></img></a>

💻 full-stack web engineer, Node.js & GraphQL enthusiast

<br/>

<img align="left" width="100" height="100" src="assets/images/members/kyle.png">

[Kyle Martin](https://github.com/js-kyle)
<a href="https://twitter.com/kylemartin_93"><img src="assets/images/twitter-s.png" width="16" height="16"></img></a>
<a href="https://www.linkedin.com/in/kylemartinnz"><img src="assets/images/linkedin.png" width="16" height="16"></img></a>

Full Stack Developer & Site Reliability Engineer based in New Zealand, interested in web application security, and architecting and building Node.js applications to perform at global scale.

<br/>

<img align="left" width="100" height="100" src="assets/images/members/sagir.png">

[Sagir Khan](https://github.com/sagirk)
<a href="https://twitter.com/sagir_k"><img src="assets/images/twitter-s.png" width="16" height="16"></img></a>
<a href="https://sagirk.com"><img src="assets/images/www.png" width="16" height="16"></img></a>
<a href="https://linkedin.com/in/sagirk"><img src="assets/images/linkedin.png" width="16" height="16"></img></a>

Deep specialist in JavaScript and its ecosystem — React, Node.js, MongoDB, pretty much anything that involves using JavaScript/JSON in any layer of the system — building products using the web platform for the world’s most recognized brands. Individual Member of the Node.js Foundation, collaborating on the Community Committee's Website Redesign Initiative.

<br/>

## Collaborators

Thank you to all our collaborators! 🙏

Our collaborators are members who are contributing to the repository on a reguar basis, through suggesting new best practices, triaging issues, reviewing pull requests and more. If you are interested in helping us guide thousands of people to craft better Node.js applications, please read our [contributor guidelines](/.operations/CONTRIBUTING.md) 🎉

| <a href="https://github.com/idori" target="_blank"><img src="assets/images/members/ido.png" width="75" height="75"></a> | <a href="https://github.com/TheHollidayInn" target="_blank"><img src="assets/images/members/keith.png" width="75" height="75"></a> |
| :--: | :--: |
| [Ido Richter (Founder)](https://github.com/idori) | [Keith Holliday](https://github.com/TheHollidayInn) |

### Past collaborators

| <a href="https://github.com/refack" target="_blank"><img src="assets/images/members/refael.png" width="50" height="50"></a> |
| :--: |
| [Refael Ackermann](https://github.com/refack) |

<br/>

## Thank You Notes

We appreciate any contribution, from a single word fix to a new best practice. Below is a list of everyone who contributed to this project. A 🌻 marks a successful pull request and a ⭐ marks an approved new best practice.

### Flowers

🌻 [Kevin Rambaud](https://github.com/kevinrambaud),
🌻 [Michael Fine](https://github.com/mfine15),
🌻 [Shreya Dahal](https://github.com/squgeim),
🌻 [ChangJoo Park](https://github.com/ChangJoo-Park),
🌻 [Matheus Cruz Rocha](https://github.com/matheusrocha89),
🌻 [Yog Mehta](https://github.com/BitYog),
🌻 [Kudakwashe Paradzayi](https://github.com/kudapara),
🌻 [t1st3](https://github.com/t1st3),
🌻 [mulijordan1976](https://github.com/mulijordan1976),
🌻 [Matan Kushner](https://github.com/matchai),
🌻 [Fabio Hiroki](https://github.com/fabiothiroki),
🌻 [James Sumners](https://github.com/jsumners),
🌻 [Chandan Rai](https://github.com/crowchirp),
🌻 [Dan Gamble](https://github.com/dan-gamble),
🌻 [PJ Trainor](https://github.com/trainorpj),
🌻 [Remek Ambroziak](https://github.com/reod),
🌻 [Yoni Jah](https://github.com/yonjah),
🌻 [Misha Khokhlov](https://github.com/hazolsky),
🌻 [Evgeny Orekhov](https://github.com/EvgenyOrekhov),
🌻 [Gediminas Petrikas](https://github.com/gediminasml),
🌻 [Isaac Halvorson](https://github.com/hisaac),
🌻 [Vedran Karačić](https://github.com/vkaracic),
🌻 [lallenlowe](https://github.com/lallenlowe),
🌻 [Nathan Wells](https://github.com/nwwells),
🌻 [Paulo Vítor S Reis](https://github.com/paulovitin),
🌻 [syzer](https://github.com/syzer),
🌻 [David Sancho](https://github.com/davesnx),
🌻 [Robert Manolea](https://github.com/pupix),
🌻 [Xavier Ho](https://github.com/spaxe),
🌻 [Aaron Arney](https://github.com/ocularrhythm),
🌻 [Jan Charles Maghirang Adona](https://github.com/septa97),
🌻 [Allen Fang](https://github.com/AllenFang),
🌻 [Leonardo Villela](https://github.com/leonardovillela),
🌻 [Michal Zalecki](https://github.com/MichalZalecki)
🌻 [Chris Nicola](https://github.com/chrisnicola),
🌻 [Alejandro Corredor](https://github.com/aecorredor),
🌻 [Ye Min Htut](https://github.com/ymhtut),
🌻 [cwar](https://github.com/cwar),
🌻 [Yuwei](https://github.com/keyfoxth),
🌻 [Utkarsh Bhatt](https://github.com/utkarshbhatt12),
🌻 [Duarte Mendes](https://github.com/duartemendes),
🌻 [Sagir Khan](https://github.com/sagirk),
🌻 [Jason Kim](https://github.com/serv),
🌻 [Mitja O.](https://github.com/Max101),
🌻 [Sandro Miguel Marques](https://github.com/SandroMiguel),
🌻 [Gabe Kuslansky](https://github.com/GabeKuslansky),
🌻 [Ron Gross](https://github.com/ripper234),
🌻 [Valeri Karpov](https://github.com/vkarpov15)
🌻 [Sergio](https://github.com/imsergiobernal),
🌻 [Duarte Mendes](https://github.com/duartemendes),
🌻 [Nikola Telkedzhiev](https://github.com/ntelkedzhiev),
🌻 [Vitor Godoy](https://github.com/vitordagamagodoy),
🌻 [Manish Saraan](https://github.com/manishsaraan),
🌻 [Sangbeom Han](https://github.com/uronly14me),
🌻 [blackmatch](https://github.com/blackmatch),
🌻 [Joe Reeve](https://github.com/ISNIT0),
🌻 [Marcelo Melo](https://github.com/marcelosdm),
🌻 [Ryan Busby](https://github.com/BusbyActual),
🌻 [Iman Mohamadi](https://github.com/ImanMh),
🌻 [Remek Ambroziak](https://github.com/reod),
🌻 [Sergii Paryzhskyi](https://github.com/HeeL),
🌻 [Kapil Patel](https://github.com/kapilepatel),
🌻 [迷渡](https://github.com/justjavac),
🌻 [Hozefa](https://github.com/hozefaj),
🌻 [Ethan](https://github.com/el-ethan),
🌻 [Sam](https://github.com/milkdeliver),
🌻 [Arlind](https://github.com/ArlindXh),
🌻 [Teddy Toussaint](https://github.com/ttous),
🌻 [Lewis](https://github.com/LewisArdern),
🌻 [DouglasMV](https://github.com/DouglasMV),
🌻 [Corey Cleary](https://github.com/coreyc),
🌻 [Mehmet Perk](https://github.com/mperk),
🌻 [Ryan Ouyang](https://github.com/ryanouyang),
🌻 [Gabriel Lidenor](https://github.com/GabrielLidenor),
🌻 [Roman](https://github.com/animir),
🌻 [Francozeira](https://github.com/Francozeira),
🌻 [Invvard](https://github.com/Invvard),
🌻 [Rômulo Garofalo](https://github.com/romulogarofalo),
🌻 [Tho Q Luong](https://github.com/thoqbk),
🌻 [Burak Shen](https://github.com/Qeneke),
🌻 [Martin Muzatko](https://github.com/MartinMuzatko),
🌻 [zhuweiyou](https://github.com/zhuweiyou)


### Stars

⭐ [Kyle Martin](https://github.com/js-kyle),
⭐ [Keith Holliday](https://github.com/TheHollidayInn),
⭐ [Corey Cleary](https://github.com/coreyc),
⭐ [Maximilian Berkmann](https://github.com/Berkmann18),
⭐ [DouglasMV](https://github.com/DouglasMV),
⭐ [Marcelo Melo](https://github.com/marcelosdm),
⭐ [Mehmet Perk](https://github.com/mperk),
⭐ [Ryan Ouyang](https://github.com/ryanouyang)

<br/><br/><br/>
