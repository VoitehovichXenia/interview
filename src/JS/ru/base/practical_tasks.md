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

# Promise

## Полифилл Promise.all

```js
// тестовые данные
const resolve = (value, timeout) => {
  return new Promise((res) => setTimeout(res, timeout, value))
}
const reject = (value, timeout) => {
  return new Promise((_, rej) => setTimeout(rej, timeout, value))
}

const promises = [resolve(1, 200), resolve(2, 300), resolve(3, 100)]
const promisesWithReject = [resolve(1, 200), reject(2, 100), resolve(3, 100)]

Promise.all(promises).then(result => console.log('Promise.all: ', result))
Promise.all(promisesWithReject).catch(err => console.log('Promise.all: ',err))

promiseAll(promises).then(result => console.log('Polyfill: ', result))
promiseAll(promisesWithReject).catch(err => console.log('Polyfill: ', err))
// Promise.myAll(promises).then(result => console.log('Polyfill: ', result))
// Promise.myAll(promisesWithReject).catch(err => console.log('Polyfill: ', err))
```
<details>
<summary>Ответ</summary>

Что делает метод: принимает массив промисов и возвращает промис который выполнится тогда, когда будут выполнены все промисы, переданные в виде перечисляемого аргумента, или отклонено любое из переданных промисов.

В случае успешного выполнения вернет массив с результатами сохраняя порядок переданного массива

В случае если какой либо из промисов отклонен промис будет также отклонен

Идея:
- возвращаем промис
- создаем в нем переменные для хранения результатов и счетчик выполнененных промисов
- циклически обходим переданный массив, в цикле:
    - резолвим исходный промис
    - в ветке then записываем результат в массив по индексу и увеличиваем счетчик
    - проверяем счетчик - если дошли до конца => резолвим возвращаемый промис
    - в ветку catch передаем reject возвращаемого промиса
- *валидация входящего массива

```js
// Статический метод
Promise.myAll = function (promises) {
  return new Promise((resolve, reject) => {
    // Проверка аргумента
    if (!Array.isArray(promises)) {
      reject(`${promises} is not an array`)
    }

    // Если массив пустой
    if (!promises.length) {
      resolve([])
    }

    const results = []
    let completed = 0;

    promises.forEach((promise, index) => {
      Promise.resolve(promise)
        .then(result => {
          results[index] = result
          completed++
          if (completed === promises.length) resolve(results)
        })
        .catch(reject)
    })
  })
}

// функция
function promiseAll(promises) {
  return new Promise((resolve, reject) => {
    // Проверка аргумента
    if (!Array.isArray(promises)) {
      reject(`${promises} is not an array`)
    }

    // Если массив пустой
    if (!promises.length) {
      resolve([])
    }

    const results = []
    let completed = 0

    promises.forEach((promise, index) => {
      Promise.resolve(promise)
        // в случае успешного выполнения
        .then(result => {
          results[index] = result;
          completed += 1;

          if (completed === promises.length) {
            resolve(results)
          }
        })
        // в случае неуспешного выполнения
        .catch(reject)
    })
  })
}
```

</details>

## Полифилл Promise.allSettled

```js
// тестовые данные
const resolve = (value, timeout) => {
  return new Promise((res) => setTimeout(res, timeout, value))
}
const reject = (value, timeout) => {
  return new Promise((_, rej) => setTimeout(rej, timeout, value))
}

const promisesWithReject = [resolve(1, 200), reject(2, 100), resolve(3, 100)]

Promise.allSettled(promisesWithReject).then(result => console.log('Promise.allSettled: ', result))

promiseAllSettled(promisesWithReject).then(result => console.log('Polyfill: ', result))
// Promise.myAllSettled(promisesWithReject).then(result => console.log('Polyfill: ', result))
```
<details>
<summary>Ответ</summary>

Что делает метод: возвращает промис, который будет выполнен когда все переданные промисы завершатся (успешно или нет)

Возвращает массив со статусом промиса и его результатом

Идея:
- возвращаем промис
- в промисе создаем массив с результатами и счетчик выполненых промисов
- циклически обходим массив промисов
    - резолвим каждый из них
    - в ветке then записываем статус и результат по индексу в массив результатов и увеличиваем счетчик
    - далее проверяем счетчик => если счетчик равен числу промисов в массиве => резолвим возвращаемый промис 
    - в ветке catch повторяем то же самое

```js
// статический метод
Promise.myPromiseAllSettled = function (promises) {
  return new Promise((resolve, reject) => {
    // Проверка аргумента
    if (!Array.isArray(promises)) {
      reject(`${promises} is not an array`)
    }

    // Если массив пустой
    if (!promises.length) {
      resolve([])
    }

    const results = []
    let completed = 0

    promises.forEach((promise, index) => {
      Promise.resolve(promise)
        // в случае успешного выполнения
        .then(result => {
          results[index] = { status: 'fulfilled', value: result };
          completed++;

          if (completed === promises.length) {
            resolve(results)
          }
        })
        // в случае неуспешного выполнения
        .catch((err) => {
          results[index] = { status: 'rejected', reason: err };
          completed++;

          if (completed === promises.length) {
            resolve(results)
          }
        })
    })
  })
}

// функция
function promiseAllSettled(promises) {
  return new Promise((resolve, reject) => {
    const results = []
    let completed = 0

    promises.forEach((promise, index) => {
      Promise.resolve(promise)
        .then(result => {
          results[index] = { status: 'fulfilled', value: result}
          completed++

          if (completed === promises.length) resolve(results)
        })
        .catch(result => {
          results[index] = { status: 'rejected', reason: result}
          completed++

          if (completed === promises.length) resolve(results)
        })
    })
  })
}
```

</details>

## Написать поллифил для Promise.race

```js
// тестовые данные
const resolve = (value, timeout) => {
  return new Promise((res) => setTimeout(res, timeout, value))
}
const reject = (value, timeout) => {
  return new Promise((_, rej) => setTimeout(rej, timeout, value))
}

const promises = [resolve(1, 200), resolve(2, 300), resolve(3, 100)]
const promisesWithReject = [resolve(1, 200), reject(2, 100), resolve(3, 100)]

Promise.race(promises).then(result => console.log('Promise.race: ', result))
Promise.race(promisesWithReject)
  .then(result => console.log('Promise.race: ', result))
  .catch(err => console.log('Promise.race: ', err))

promiseRace(promises).then(result => console.log('Polyfill: ', result))
promiseRace(promisesWithReject)
  .then(result => console.log('Polyfill: ', result))
  .catch(err => console.log('Polyfill: ', err))
// Promice.myRace(promises).then(result => console.log('Polyfill: ', result))
// Promice.myRace(promisesWithReject)
//   .then(result => console.log('Polyfill: ', result))
//   .catch(err => console.log('Polyfill: ', err))
```
<details>
<summary>Ответ</summary>

Что делает метод: принимает массив промисов и возвращает любой промис который выполнится или отклонится первый

Идея:
- возвращаем промис
- в промисе обходим переданный массив циклом:
    - резолвим промис
    - в ветке then передаем resolve возвращаемого промиса (зарезолвит первый выполненный)
    - в ветке catch передаем reject возвращаемого промиса (отклонит первый отклоненный промис)

```js
// статический метод
Promise.myRace = function(promises) {
  return new Promise((resolve, reject) => {
    promises.forEach(promise => {
      Promise.resolve(promise)
        .then(resolve)
        .catch(reject)
    })
  })
}

// функция
function promiseRace(promises) {
  return new Promise((resolve, reject) => {
    promises.forEach(promise => {
      Promise.resolve(promise)
        .then(resolve)
        .catch(reject)
    })
  })
}
```

</details>

## Написать поллифил для Promise.any

```js
// тестовые данные
const resolve = (value, timeout) => {
  return new Promise((res) => setTimeout(res, timeout, value))
}
const reject = (value, timeout) => {
  return new Promise((_, rej) => setTimeout(rej, timeout, value))
}

const promises = [resolve(1, 200), resolve(2, 300), resolve(3, 100)]
const promisesWithReject = [reject(1, 200), reject(2, 100), reject(3, 100)]

Promise.any(promises).then(result => console.log('Promise.race: ', result))
Promise.any(promisesWithReject).catch(err => console.log('Promise.race: ', err))

promiseAny(promises).then(result => console.log('Polyfill: ', result))
promiseAny(promisesWithReject).catch(err => console.log('Polyfill: ', err))
// Promise.myAny(promises).then(result => console.log('Polyfill: ', result))
// Promise.myAny(promisesWithReject).catch(err => console.log('Polyfill: ', err))
```

<details>
<summary>Ответ</summary>

Что делает метод: принимает массив промисов и возврашает первый успешно выполненый промис, если все промисы были отклонены возвращает AggregateError

Идея:
- возвращаем промис
- в промисе создаем переменную для счетчика ошибок
- циклически обходим переданный массив промисов
    - резолвим каждый промис
    - в ветке then передаем resolve
    - в ветке catch при каждой ошибке увеличиваем счетчик ошибок и проверяем равен ли счетчик ошибок количеству промисов в переданном массиве, если равен => реджектим возвращаемый промис с AggregateError

```js
Promise.myAny = function(promises) {
  return new Promise((resolve, reject) => {
    let rejected = 0
  
    promises.forEach(promise => {
      Promise.resolve(promise)
        .then(resolve)
        .catch(err => {
          rejected++
          if (rejected === promises.length) reject('AggregateError: All promises were rejected')
        })
    })
  })
}

function promiseAny(promises) {
  return new Promise((resolve, reject) => {
    let rejected = 0
  
    promises.forEach(promise => {
      Promise.resolve(promise)
        .then(resolve)
        .catch(err => {
          rejected++
          if (rejected === promises.length) reject('AggregateError: All promises were rejected')
        })
    })
  })
}
```

</details>