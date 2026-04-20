# Типы ошибок Javascript

1. EvalError: представляет ошибку, которая генерируется при выполнении глобальной функции eval()

2. RangeError: ошибка генерируется, если параметр или переменная, представляют число, которое находится вне некотоого допустимого диапазона
```js
function validateAge(age) {
  if (age < 18 || age > 115) {
    throw new RangeError('Age should be between 18 and 115')
  }

  return age;
}
```
3. ReferenceError: ошибка генерируется при обращении к несуществующей ссылке
```js
let a = udefinedVariable;
// RefferenceError
```
4. SyntaxError: представляет ошибку синтаксиса
```js
console.log(5
// SyntaxError
```
5. TypeError: ошибка генерируется, если значение переменной или параметра представляют некорректный тип или пр попытке изменить значение, которое нельзя изменять
```js
const a = 5;
a = 7
// TypeError
function isNumber(val) {
  if (val !== 'number') throw new TypeError('Value should be a number')
  return val;
}
```
6. URIError: ошибка генерируется при передаче функциям encodeURI() и decodeURI() некорректных значений
7. AggregateError: предоставляет ошибку, которая объединяет несколько возникших ошибок

# Методы перехвата и обработки ошибок в веб-приложениях?

1. Локальный перехват ошибок с помощью конструкции try/catch
```js
try {
  throw new Error('error')
} catch (e) {
  console.error(e)
  // do something
}
```
2. Для отлавливания асинхронных ошибок - .catch ветка в Promise
```js
Promise.reject('Some error')
.catch(err => {
  console.log(err)
  // do something
})
```
3. window.onerror
```js
window.onerror = (err) => {
  console.log(`Global error: ${err}`)
}
```