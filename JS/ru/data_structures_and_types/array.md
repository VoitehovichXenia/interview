# Массив

**Массив** - структура данных, хранящая упорядоченную информацию, имеющая длину. Является разновидностью объекта, однако в качестве ключа используется пронумерованная позиция - индекс.

## Создание массива

- Array literal
- Array constructor
- [from()](#from) и [fromAsync()](#fromasync)
- [of()](#of)

```ts
// Array literal
const arr = ['Apple'];

// Array constructor
// Создает массив заданной длины, но с пустыми значениями [ empty, empty ...]
// Чтобы его заполнить нужно использовать метод fill(value, start, end)
const arr_2 = new Array(length: number)
```

## Манипуляции с массивом

### Обращение к элементу

- через индекс
- at(index)

```ts
const arr = ['apple', 'orange', 'banana']
console.log(arr[0]) // 'apple'
console.log(arr[100]) // undefined

arr.at(2) // 'banana'
arr.at(-2) // 'orange'
```

### Вставка

- вставка в конец с помощью обращения к индексу
- push()
- unshift()

```ts
const arr = ['apple']
// Вставка в конец
arr[arr.length - 1] = 'orange'

arr.push('orange')

// Вставка в начало
// Приводит к сдвигу
arr.unshift('banana')
```

### Удаление

- pop()
- shift()
- splice()

```ts
const arr = ['apple', 'orange']
// Удаление с конца
// Возвращает удаленный элемент
arr.pop()

// Удаление из начала
// Приводит к сдвигу
// Возвращает удаленный элемент
arr.shift()
```

### Поиск

- find()
- findIndex()
- findLast()
- findLastIndex()

### Фильтрация

- filter()

### Проверка условий

- some()
- every()

## Другие

- map()
- forEach()
- concat()
- copyWithin() - [read more](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/copyWithin)
- reduce() - [read more](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce)
- toReversed()
- toSorted()
- toSpliced()
- toString()
- with(index, value) - возвращает новый массив, с изменненым значением по выбраному индексу

# Методы массива 

## Статические методы

### from()

``from(iterable)``

создаёт новый экземпляр Array из массивоподобного или итерируемого объекта [Читать больше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Array/from)

```ts
// Создает массив из массивоподобных объектов (объектов со свойством length и элементами по индексным ключам) или итерируемых объектов (объектов, из которых можно достать их элементы, например Map или Set).
const arr_3 = Array.from([1, 2, 3], (x, i, this) => x + 2) // [ 3, 4, 5 ]
```

### fromAsync()

``fromAsync(iterable)``

создаёт новый экземпляр Array из массивоподобного или async iterable, iterable, или array-like объекта.
[Читать больше](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/fromAsync)

### isArray()

``isArray(something)``

возвращает true, если объект является массивом и false, если он массивом не является [Читать больше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Array/isArray)

### of()

``of(el, el1, ... elN)``

создаёт новый экземпляр массива Array из произвольного числа аргументов, вне зависимости от числа или типа аргумента [Читать больше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Array/of)

```ts
// Создает массив из заданных элементов
const arr_4 = Array.of("foo", 2, "bar", true) // [ "foo", 2, "bar", true ]
```
