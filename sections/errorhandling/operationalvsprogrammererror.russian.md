# Различайте операционные и программистские ошибки

### Объяснение в один абзац

Различение следующих двух типов ошибок минимизирует время простоя вашего приложения и помогает избежать сумасшедших ошибок: Операционные ошибки относятся к ситуациям, когда вы понимаете, что произошло, и влияние этого - например, запрос к какой-либо службе HTTP не выполнен из-за проблемы с подключением. С другой стороны, ошибки программиста относятся к случаям, когда вы не знаете, почему, а иногда и откуда возникла ошибка - это может быть какой-то код, который пытался прочитать неопределенное значение или пул соединений с БД, который приводит к утечке памяти. Операционные ошибки относительно легко обрабатываются - обычно достаточно регистрации ошибки. Вещи становятся неприятными, когда появляется ошибка программиста, приложение может быть в несовместимом состоянии, и нет ничего лучше, чем перезапуск изящно.

### Пример кода - пометка ошибки как рабочей (доверенной)

<details>
<summary><strong>Javascript</strong></summary>

```javascript
// marking an error object as operational 
const myError = new Error('How can I add new product when no value provided?');
myError.isOperational = true;

// or if you're using some centralized error factory (see other examples at the bullet "Use only the built-in Error object")
class AppError {
  constructor (commonType, description, isOperational) {
    Error.call(this);
    Error.captureStackTrace(this);
    this.commonType = commonType;
    this.description = description;
    this.isOperational = isOperational;
  }
};

throw new AppError(errorManagement.commonErrors.InvalidInput, 'Describe here what happened', true);

```
</details>

<details>
<summary><strong>Typescript</strong></summary>

```typescript
// some centralized error factory (see other examples at the bullet "Use only the built-in Error object")
export class AppError extends Error {
  public readonly commonType: string;
  public readonly isOperational: boolean;

  constructor(commonType: string, description: string, isOperational: boolean) {
    super(description);

    Object.setPrototypeOf(this, new.target.prototype); // restore prototype chain

    this.commonType = commonType;
    this.isOperational = isOperational;

    Error.captureStackTrace(this);
  }
}

// marking an error object as operational (true)
throw new AppError(errorManagement.commonErrors.InvalidInput, 'Describe here what happened', true);

```
</details>

### Цитата блога: "Ошибки программиста - это ошибки в программе"

Из блога Джойент, занявшему 1 место по ключевым словам "Обработка ошибок Node.js"

> … Лучший способ исправить ошибки программиста - это сразу же аварийно завершить работу. Вы должны запустить свои программы, используя перезапуск, который автоматически перезапустит программу в случае сбоя. С перезапуском на месте сбой является самым быстрым способом восстановления надежного сервиса перед лицом временной ошибки программиста …

### Цитата из блога: "Нет безопасного способа уйти, не создав неопределенного хрупкого состояния"

Из официальной документации Node.js

> … По самой природе того, как throw работает в JavaScript, почти никогда не существует способа безопасно "выбрать то, на чем остановился", без утечки ссылок или создания какого-либо другого неопределенного хрупкого состояния. Самый безопасный способ отреагировать на возникшую ошибку - завершить процесс. Конечно, на обычном веб-сервере у вас может быть открыто много подключений, и нет смысла внезапно закрывать их, потому что кто-то еще вызвал ошибку. Лучший подход - отправить ответ об ошибке на запрос, который вызвал ошибку, позволяя остальным завершить работу в обычное время, и прекратить прослушивание новых запросов этого работника.

### Цитата блога: "В противном случае вы рискуете состоянием вашего приложения"

Из блога debugable.com, занявшего 3 место по ключевым словам "Node.js uncaught exception"

> … Итак, если вы действительно не знаете, что делаете, вам следует выполнить постепенный перезапуск службы после получения события исключения "uncaughtException". В противном случае вы рискуете привести к несогласованности состояния вашего приложения или сторонних библиотек, что приведет к всевозможным сумасшедшим ошибкам …

### Цитата блога: "Есть три школы мысли об обработке ошибок"

Из блога: JS Recipes

> … Существует три основных направления работы с ошибками:
1. Дайте приложению упасть и перезапустите его.
2. Обрабатывайте все возможные ошибоки и никогда не роняйте.
3. Сбалансированный подход между двумя предыдущими.
