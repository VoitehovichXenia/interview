# Функции

## Написать polyfills для bind call apply

### bind()

```js
// Тестовые данные
const tesla = {
  name: 'Tesla',
  getFullName (model, year) {
    return `${this.name} ${model || ''} ${year || ''}`.trim()
  }
}

const audi = {
  name: 'Audi'
}

audi.getFullName = tesla.getFullName.myBind(audi)
// audi.getFullName = myBind(tesla.getFullName, audi)

console.log(tesla.getFullName('model y', 2020))
console.log(audi.getFullName('a3', 2026))
```

<details>

<summary>Ответ</summary>

Что делает метод - возвращает новую функцию с привязанным новым контекстом

Идея:
- записать как статический метод
    - сохраняем оригинальную функцию
    - в возвращаемой фунуции определяем новый контекст (если не был передан - window)
    - создаем временный метод в новом контексте, присваивая ему оригинальную функцию
    - вызываем его с переданными аргументами сохраняя результат
    - удаляем временный метод
    - возвращаем результат
- создать функцию, которая будет получать 2 параметра: оригинальную функцию и контекст
    - в возвращаемой фунуции определяем новый контекст (если не был передан - window)
    - создаем временный метод в новом контексте, присваивая ему оригинальную функцию
    - вызываем его с переданными аргументами сохраняя результат
    - удаляем временный метод
    - возвращаем результат

```js
// статический метод
Function.prototype.myBind = function (ctx) {
  const originalFunc = this

  return function(...args) {
    const newFuncContext = ctx ?? window

    const temporarySymbol = Symbol()

    newFuncContext[temporarySymbol] = originalFunc

    const result = newFuncContext[temporarySymbol](...args)

    delete newFuncContext[temporarySymbol]

    return result
  }
}

// функция
function myBind (func, ctx) {
  if (typeof func !== 'function') throw new Error('The first argument must be a function')

  return function (...args) {
    const finalContext = ctx ?? window

    const tempSymbol = Symbol()
    finalContext[tempSymbol] = func

    const result = finalContext[tempSymbol](...args)

    delete finalContext[tempSymbol]

    return result
  }
}
```

</details>

### call()

```js
// Тестовые данные
const tesla = {
  name: 'Tesla',
  getFullName (model, year) {
    return `${this.name} ${model || ''} ${year || ''}`.trim()
  }
}

const audi = {
  name: 'Audi'
}

console.log(tesla.getFullName('model y', 2020))
console.log(tesla.getFullName.myCall(audi, 'a3', 2026))
// console.log(myCall(tesla.getFullName, audi, 'a3', 2026))
```

<details>

<summary>Ответ</summary>

Что делает метод: вызывает функцию для переданного контекста с переданными аргументами

Идея:
- записать как статический метод
    - определяем новый контекст (если не был передан - window)
    - создаем временный метод в новом контексте, присваивая ему оригинальную функцию
    - вызываем его с переданными аргументами сохраняя результат
    - удаляем временный метод
    - возвращаем результат
- создать функцию, которая будет получать 2 параметра: оригинальную функцию и контекст
    - то же самое
    - *валидация аргументов

```js
// статический метод
Function.prototype.myCall = function (ctx, ...arg) {
  if (typeof ctx !== 'object' && ctx !== undefined) throw new Error('The first argument must be an object or undefined')

  const finalContext = ctx ?? window

  const tempSymbol = Symbol()
  finalContext[tempSymbol] = this

  const result = finalContext[tempSymbol](...args)

  delete finalContext[tempSymbol]

  return result
}

// функция
function myCall (func, ctx, ...args) {
  if (typeof func !== 'function') throw new Error('The first argument must be a function')
  if (typeof ctx !== 'object' && ctx !== undefined) throw new Error('The second argument must be an object or undefined')

  const finalContext = ctx ?? window

  const tempSymbol = Symbol()
  finalContext[tempSymbol] = func

  const result = finalContext[tempSymbol](...args)

  delete finalContext[tempSymbol]

  return result
}
```

</details>

### apply()

```js
// Тестовые данные
const tesla = {
  name: 'Tesla',
  getFullName (model, year) {
    return `${this.name} ${model || ''} ${year || ''}`.trim()
  }
}

const audi = {
  name: 'Audi'
}

console.log(tesla.getFullName('model y', 2020))
console.log(tesla.getFullName.myApply(audi, ['a3', 2026]))
// console.log(myApply(tesla.getFullName, audi, ['a3', 2026]))
```

<details>

<summary>Ответ</summary>

Что делает метод: вызывает функцию для переданного контекста с переданными в качестве массива аргументами

Идея:
- записать как статический метод
    - определяем новый контекст (если не был передан - window)
    - создаем временный метод в новом контексте, присваивая ему оригинальную функцию
    - вызываем его с переданными аргументами сохраняя результат
    - удаляем временный метод
    - возвращаем результат
- создать функцию, которая будет получать 2 параметра: оригинальную функцию и контекст
    - то же самое
    - *валидация аргументов

```js
// статический метод
Function.prototype.myApply = function (ctx, args) {
  if (typeof ctx !== 'object' && ctx !== undefined) throw new Error('The first argument must be an object or undefined')
  if (args && !Array.isArray(args)) throw new Error('The second argument must be an array or undefined')

  const finalContext = ctx ?? window

  const tempSymbol = Symbol()
  finalContext[tempSymbol] = this

  const result = finalContext[tempSymbol](...(args || []))

  delete finalContext[tempSymbol]

  return result
}

// функция
function myApply(func, ctx, args) {
  if (typeof func !== 'function') throw new Error('The first argument should be a function')
  if (typeof ctx !== 'object' && ctx !== undefined) throw new Error('The second argument should be an object or undefined')
  if (args && !Array.isArray(args)) throw new Error('The third argument should be an array or undefined')

  const finalContext = ctx ?? window

  const tempSymbol = Symbol()
  finalContext[tempSymbol] = func

  const result = finalContext[tempSymbol](...(args || []))

  delete finalContext[tempSymbol]

  return result
}
```

</details>

## Написать мемоизированную функцию

- функция должна быть мемоизирована принимает только 1 аргумент
- у мемоизированной должен быть метод очистки кэша
- '1' и 1 это разные результаты вычислений
- undefined null NaN фдекватные результаты вычислений

<details>
<summary>Ответ</summary>

```js
function memo(fn) {
  // Map choossen as it could acept any value for keys
  const cache = new Map()

  const memoized = function (arg) {
    if (cache.has(arg)) return cache.get(arg)

    const result = fn(arg)
    cache.set(arg, result)

    return result
  }

  // Присваиваем метод
  memoized.clearCache = () => cache.clear()

  return memoized
}

function pow (arg) {
  console.log('Calculating...')
  let result = 0;

  for (let i = 0; i < 10 ** arg; i++) {
    result++
  }

  return result
}

const memoized = memo(pow)

console.log('Numbers:')
console.log(memoized(2))
console.log(memoized(2))
memoized.clearCache()
console.log('cache cleared')
console.log(memoized(2))
console.log(memoized(3))
console.log('Undefined:')
console.log(memoized(undefined))
console.log(memoized(undefined))
console.log('Null:')
console.log(memoized(null))
console.log(memoized(null))
console.log('NaN:')
console.log(memoized(NaN))
console.log(memoized(NaN))
```

</details>

## Написать debounce

```js
// Тестовые данные
const URL = 'https://jsonplaceholder.typicode.com/users'

async function getUsers() {
  try {
    const res = await fetch(URL)
    console.log(await res.json())
  } catch (err) {
    console.log(err.message)
  }
}

const debouncedGetUsers = debounce(getUsers, 500)

debouncedGetUsers()
debouncedGetUsers()
debouncedGetUsers()
```

<details>

<summary>Ответ</summary>

Идея:
- наша функция должна получать функцию и задержку
- внутри мы должны хранить id таймера
- мы должны вернуть функцию, которая очищает предыдущий таймер и создает новый при повторном вызове

```js
const debounce = function (func, delay) {
  let timerId
  return function(...args) {
    clearTimeout(timerId)
    timerId = setTimeout(func, delay, ...args)
  }
}
```
</details>

## Написать throttle функцию

```js
// тестовые данные
const throttled = throttle((val) => {
  console.log("call:", val, Date.now());
}, 1000);

console.log(`start: ${Date.now()}`)

const args = [0, 1, 2, 3, 4, 5, 6]
let index = 0;

const intervalId = setInterval(() => {
  throttled(args[index])
  index++
}, 300)

const timerId = setTimeout(() => clearInterval(intervalId), 2000)
// функция должна быть вызвана 2 раза
// call: 0 ...
// call: 4 ...
```

<details>

<summary>Ответ</summary>

Идея:
- в функции создаем переменную для сохранения текущего timestamp
- возвращаем функцию, которая
    - фиксирует время текущего вызова
    - проверяет сколько прошло со времени последнего вызова и сравнивает это с требуемой задержкой
    - если timestamp прошлого вызова + задержка меньше чем текущее время вызова => вызываем функцию

```js
function throttle(func, delay) {
  let lastTimestamp = 0
  return function (...args) {
    const now = Date.now()
    if (now >= lastTimestamp + delay) {
      lastTimestamp = now
      func(...args)
    }
    
  }
}
```

</details>

## Написать функцию fetchRetry

- функция должна принимать аргументы url (url запроса), retries (количество запросов), delay (интервал через который необходимо выполнить повторный запрос)


```jsx
function fetchRetry (url, retries, delay) {
  /* implement your code here */
}

export default function App () {
  const [data, setData] = useState(null)
  const [error, setError] = useState(null)

  useEffect(() => {
    fetchRetry(URL, RETRIES, DELAY)
    // fetchRetry(URL_ERROR, RETRIES, DELAY)
      .then(data => setData(data))
      .catch(err => setError(err.message))
  }, [])

  return (
    <div>
      {data && (
        <ul>
          {data.map(user => <li key={user.id}>{user.name}</li>)}
        </ul>
      )}
      {error && <div>Error: {error}</div>}
    </div>
  )
}
```

<details>
<summary>Ответ</summary>

```jsx
const URL = 'https://jsonplaceholder.typicode.com/users'
const URL_ERROR = 'https://jsonplaceholder.typicode.com/users_error'
const RETRIES = 3
const DELAY = 300

async function fetchRetry (url, retries, delay) {
  try {
    const res = await fetch(url)

    if (!res.ok) {
      throw new Error(`Request has failed with ${res.status} ${res.statusText}`)
    }

    return await res.json()
  } catch (err) {
    if (retries <= 1) throw err

    // ждем заданный delay
    await new Promise(resolve => setTimeout(resolve, delay))

    // повторный request
    return fetchRetry(url, retries - 1, delay)
  }
}

export default function App () {
  const [data, setData] = useState(null)
  const [error, setError] = useState(null)

  useEffect(() => {
    fetchRetry(URL, RETRIES, DELAY)
    // fetchRetry(URL_ERROR, RETRIES, DELAY)
      .then(data => setData(data))
      .catch(err => setError(err.message))
  }, [])

  return (
    <div>
      {data && (
        <ul>
          {data.map(user => <li key={user.id}>{user.name}</li>)}
        </ul>
      )}
      {error && <div>Error: {error}</div>}
    </div>
  )
}
```

</details>
