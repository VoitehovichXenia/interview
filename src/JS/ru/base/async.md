# Полифилл Promise.all

```js
Promise.prototype.myPromiseAll = function (promises) {
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