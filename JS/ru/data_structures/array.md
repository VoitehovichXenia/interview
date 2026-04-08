# Массив

Массив - структура данных, хранящая упорядоченную информацию, имеющая длину. Является разновидностью объекта, однако в качестве ключа используется пронумерованная позиция - индекс.

### Создание массива

- array literal
- array constructor
- from()
- of()

```ts
const arr = ['Apple'];

const arr_2 = new Array(length: number)
// Создает массив заданной длины, но с пустыми значениями [ empty, empty ...]
// Чтобы его заполнить нужно использовать метод fill(value, start, end)

const arr_3 = Array.from([1, 2, 3], (x, i, this) => x + 2) // [ 3, 4, 5 ]
// Создает массив из массивоподобных объектов (объектов со свойством length и элементами по индексным ключам) или итерируемых объектов (объектов, из которых можно достать их элементы, например Map или Set).

const arr_4 = Array.of("foo", 2, "bar", true) // [ "foo", 2, "bar", true ]
// Создает массив из заданных элементов
```

## Манипуляции с массивом и его методы

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

