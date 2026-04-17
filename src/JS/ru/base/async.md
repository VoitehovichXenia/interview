# Полифилл Promise.all

```js
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

const resolve = (value, timeout) => {
  return new Promise((res) => setTimeout(res, timeout, value))
}
const reject = (value, timeout) => {
  return new Promise((_, rej) => setTimeout(rej, timeout, value))
}

const promises = [resolve(1, 200), resolve(2, 300), resolve(3, 100)]
const promisesWithReject = [resolve(1, 200), reject(2, 100), resolve(3, 100)]

promiseAll(promises).then(result => console.log(result))
promiseAll(promisesWithReject).catch(err => console.log(err))
```

# Полифилл Promise.allSettled

```js
Promise.prototype.myPromiseAllSettled = function (promises) {
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
          completed++;

          if (completed === promises.length) {
            resolve(results)
          }
        })
        // в случае неуспешного выполнения
        .catch((err) => {
          results[index] = err;
          completed++;

          if (completed === promises.length) {
            resolve(results)
          }
        })
    })
  })
}
```