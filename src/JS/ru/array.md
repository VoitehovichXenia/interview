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
- [at()](#at)

```ts
const arr = ['apple', 'orange', 'banana']
console.log(arr[0]) // 'apple'
console.log(arr[100]) // undefined
```

### Вставка

- вставка в конец с помощью обращения к индексу
- [push()](#push)
- [unshift()](#unshift)

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

- [pop()](#pop)
- [shift()](#shift)
- [splice()](#splice)

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

- [find()](#find)
- [findIndex()](#findindex)
- [findLast()](#findlast)
- [findLastIndex()](#findlastindex)

# Методы массива 

## Статические методы

### from()

``from(iterable, mapFn)``

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

## Методы экземпляра

### at()

``at(index)``

принимает значение в виде целого числа и возвращает элемент массива с данным индексом [Читать больше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Array/at)

### concat()

``concat(value1[, value2[, ...[, valueN]]])``

возвращает новый массив, состоящий из массива, на котором он был вызван, соединённого с другими массивами и/или значениями, переданными в качестве аргументов [Читать больше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Array/concat)

### copyWithin()

``copyWithin(target, start[, end = this.length])``

копирует последовательность элементов массива внутри него в позицию, начинающуюся по индексу ``target``. Копия берётся по индексам, задаваемым вторым и третьим аргументами ``start`` и ``end``. Аргумент end является необязательным и по умолчанию равен длине массива [Читать больше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Array/copyWithin)

```ts
[1, 2, 3, 4, 5].copyWithin(0, 3);
// [4, 5, 3, 4, 5]
```

### entries()

возвращает новый объект итератора массива ``Array Iterator``, содержащий пары ключ / значение для каждого индекса в массиве [Читать больше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Array/entries)

```ts
const arr = ["a", "b", "c"];
const eArr = arr.entries();

console.log(eArr.next().value); // [0, 'a']
```

### every()
``every(callback(currentValue[, index[, array]])[, thisArg])``

проверяет, удовлетворяют ли все элементы массива условию, заданному в передаваемой функции. Метод возвращает ``true`` при любом условии для пустого массива. [Читать больше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Array/every)

```ts
const arr = [1, 2, 3]
console.log(arr.every((item) => item > 0)) // true

const arr_2 = []
console.log(arr_2.every((item) => item < 0)) // true т.к массив пустой
```

### fill()

``fill(value[, start = 0[, end = this.length]])``

заполняет все элементы массива от начального до конечного индексов (не включая его) одним значением [Читать больше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Array/fill)

```ts
const arr = new Array(3).fill(0)
console.log(arr) // [ 0, 0, 0 ]

const arr_2 = new Array(4).fill(1, 0, 2).fill(2, 2)
console.log(arr_2) // [ 1, 1, 2, 2 ]
```

### filter()

``filter(callback(currentValue[, index[, array]])[, thisArg])``

создаёт новый массив со всеми элементами, прошедшими проверку, задаваемую в передаваемой функции [Читать больше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Array/filter)

### find()

``find(callback(currentValue[, index[, array]])[, thisArg])``

возвращает значение первого найденного в массиве элемента, которое удовлетворяет условию переданному в функции. В противном случае возвращается ``undefined`` [Читать больше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Array/find)

### findIndex()

``findIndex(callback(currentValue[, index[, array]])[, thisArg])``

возвращает индекс первого удовлетворяющего условие проверяющей функции элемента в массиве. В противном случае возвращается -1. [Читать больше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Array/findIndex)

### findLast()

``findLast(callback(currentValue[, index[, array]])[, thisArg])``

возвращает значение первого найденного в массиве элемента с конца, которое удовлетворяет условию переданному в функции. В противном случае возвращается ``undefined`` [Читать больше](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/findLast)

### findLastIndex()

``findLastIndex(callback(currentValue[, index[, array]])[, thisArg])``

возвращает индекс первого с конца удовлетворяющего условие проверяющей функции элемента в массиве. В противном случае возвращается ``-1``. [Читать больше](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/findLastIndex)

### flat()

``flat(depth)``

возвращает новый массив, в котором все элементы вложенных подмассивов рекурсивно "подняты" на указанный уровень [Читать больше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Array/flat)

```ts
const arr2 = [1, 2, [3, 4, [5, 6]]];
arr2.flat();
// [1, 2, 3, 4, [5, 6]]

const arr3 = [1, 2, [3, 4, [5, 6]]];
arr3.flat(2);
// [1, 2, 3, 4, 5, 6]
```

### flatMap()

``flatMap(callback(currentValue[, index[, array]])[, thisArg])``

сначала применяет функцию к каждому элементу, а затем преобразует полученный результат в плоскую структуру и помещает в новый массив. Это идентично map функции, с последующим применением функции ``flat`` с параметром ``depth`` ( глубина ) равным ``1``, при этом аботает немного более эффективно [Читать больше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Array/flatMap)

```ts
let arr1 = [["it's","Sunny","in"],[""],["California"]]

arr1.flatMap((x) => x.split(" "));
// ["it's","Sunny","in", "", "California"]
```

### forEach()

``forEach(callback(currentValue[, index[, array]])[, thisArg])``

выполняет указанную функцию один раз для каждого элемента в массиве [Читать больше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach)

### includes()

``includes(value)``

определяет, содержит ли массив определенное значение, возвращая ``true`` или ``false`` [Читать больше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Array/includes)

### indexOf()

``indexOf(value[, fromIndex = 0])``

возвращает первый индекс, по которому данный элемент может быть найден в массиве или ``-1``, если такого индекса нет [Читать больше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Array/indexOf)

### join()

объединяет все элементы массива (или массивоподобного объекта) в строку [Читать больше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Array/join)

### keys()

возвращает новый итератор массива ``Array Iterator``, содержащий ключи каждого индекса в массиве [Читать больше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Array/keys)

```ts
const array1 = ["a", "b", "c"];
const iterator = array1.keys();

for (const key of iterator) {
  console.log(key); // 0 ...
}
```

### lastIndexOf()

``lastIndexOf(value[, fromIndex = 0])``

возвращает последний индекс, по которому данный элемент может быть найден в массиве или ``-1``, если такого индекса нет. Массив просматривается от конца к началу, начиная с индекса ``fromIndex`` [Читать больше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Array/lastIndexOf)

### map()

``map(callback(currentValue[, index[, array]])[, thisArg])``

создаёт новый массив с результатом вызова указанной функции для каждого элемента массива [Читать больше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Array/map)

### pop()

удаляет последний элемент из массива и возвращает его значение или ``undefined``, если массив пуст [Читать больше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Array/pop)

### push()

``push(element1, ..., elementN)``

добавляет один или более элементов в конец массива и возвращает новую длину массива [Читать больше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Array/push)

### reduce()

``reduce(callback(accumulator, currentValue[, index[, array]])[, initialValue])``

применяет функцию к каждому элементу массива (слева-направо), возвращая одно результирующее значение [Читать больше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce)

```ts
const array1 = [1, 2, 3, 4];

const sumWithInitial = array1.reduce(
  (accumulator, currentValue) => accumulator + currentValue,
  0,
);

console.log(sumWithInitial); // 10
```

### reduceRight()

``reduceRight(callback(accumulator, currentValue[, index[, array]])[, initialValue])``

применяет функцию к аккумулятору и каждому значению массива (справа-налево), сводя его к одному значению [Читать больше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Array/reduceRight)

### reverse()

переворачивает массив (изменяет его) [Читать больше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Array/reverse)

### shift()

удаляет первый элемент из массива и возвращает его значение [Читать больше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Array/shift)

### slice()

``slice([begin[, end = this.length]])``

возвращает новый массив, содержащий копию части исходного массива [Читать больше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Array/slice)

### some()

``some(callback(currentValue[, index[, array]])[, thisArg])``

проверяет, удовлетворяет ли хотя бы 1 элемент массива условию, заданному в передаваемой функции. Метод возвращает false при любом условии для пустого массива [Читать больше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Array/some)

```ts
const arr = [-1, -2, 3]
console.log(arr.some((item) => item > 0)) // true

const arr_2 = []
console.log(arr_2.some((item) => item < 0)) // false т.к массив пустой
```

### sort()

``sort(callback(currentValue, nextValue))``

возвращает отсортированный (измененный) массив. Порядок сортировки по умолчанию соответствует порядку кодовых точек Unicode [Читать больше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Array/sort)
- Если **callback(a, b) < 0**, сортировка поставит a по меньшему индексу, чем b, то есть, a идёт первым.
- Если **callback(a, b) === 0**, сортировка оставит a и b неизменными по отношению друг к другу, но отсортирует их по отношению ко всем другим элементам.
- Если **compareFunction(a, b) > 0**, сортировка поставит b по меньшему индексу, чем a.

### splice()

``splice(start, deleteCount, item1, item2, /* …, */ itemN)``

изменяет содержимое массива, удаляя существующие элементы и/или добавляя новые [Читать больше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Array/splice)

``start``

Отрицательный индекс отсчитывается от конца массива:
- если -array.length <= start < 0, то используется start + array.length.
    ```ts
    const myFish = ["angel", "clown", "drum", "mandarin", "sturgeon"];
    const removed = myFish.splice(-3, 1);

    console.log(myFish) // ["angel", "clown", "mandarin", "sturgeon"]
    console.log(removed) // ["drum"]
    ```
- eсли start < -array.length, то используется 0.
    ```ts
    const myFish = ["angel", "clown", "drum", "mandarin", "sturgeon"];
    const removed = myFish.splice(-7, 1);

    console.log(myFish) // ["clown", "drum", "mandarin", "sturgeon"]
    console.log(removed) // ["angel"]
    ```
- eсли start >= array.length, то ни один элемент не будет удален, но метод добавит столько элементов, сколько указано.
    ```ts
    const myFish = ["angel", "clown", "drum", "mandarin", "sturgeon"];
    const removed = myFish.splice(7, 1, "shark");

    console.log(myFish) // ["angel", "clown", "drum", "sturgeon", "shark"]
    console.log(removed) // []
    ```
- eсли start опущен (и splice() вызывается без аргументов), то ничего не удаляется. Это отличается от передачи значения undefined, которое преобразуется в 0.

``deleteCount``
- eсли deleteCount не задано или его значение больше или равно количеству элементов после позиции, указанной в start, то будут удалены все элементы от start до конца массива. Если требуется указать параметр itemN, то следует передать Infinity в качестве deleteCount, чтобы удалить все элементы после start, поскольку явное значение undefined преобразуется в 0.
- eсли deleteCount равно 0 или отрицательное, элементы не удаляются. В этом случае следует указать как минимум один новый элемент (см. ниже).
    ```ts
    var myFish = ["angel", "clown", "mandarin", "sturgeon"];
    var removed = myFish.splice(2, 0, "drum");

    console.log(myFish) // ["angel", "clown", "drum", "mandarin", "sturgeon"]
    console.log(removed) // []
    ```

### toLocaleString()

возвращает строковое представление элементов массива. Элементы преобразуются в строки с использованием своих собственных методов toLocaleString и эти строки разделяются локале-зависимой строкой (например, запятой «,») [Читать больше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Array/toLocaleString)

### toReversed()

возвращает новый массив с расположенными в обратном порядке элементами. копирующая версия [reverse()](#reverse) [Читать больше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Array/toReversed)

### toSorted()

``toSorted(callback(currentValue, nextValue))``

возвращает новый отсортированный массив. копирующая версия [sort()](#sort) [Читать больше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Array/toSorted)

### toSpliced()

``toSpliced(start, skipCount, item1, item2, /* …, */ itemN)``

удаляет существующие элементы и/или добавляет новые. копирующая версия [splice()](#splice) [Читать больше](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/toSpliced)

### toString()

возвращает строку, представляющую указанный массив и его элементы [Читать больше](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/toString)

### unshift()

``unshift(element1[, ...[, elementN]])``

добавляет один или более элементов в начало массива и возвращает новую длину массива [Читать больше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Array/unshift)

### values()

возвращает новый объект итератора массива ``Array Iterator``, содержащий значения для каждого индекса в массиве [Читать больше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Array/values)

```ts
const arr = ["w", "y", "k", "o", "p"];
const eArr = arr.values();

for (let letter of eArr) {
  console.log(letter); // 'w' ...
}
```

### with()

``with(index, value)``

возвращает новый массив, в котором элемент по указанному индексу заменён указанным значением. Является копирующей версией замены значения элемента с помощью скобочной записи [Читать больше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Array/with)

```ts
const arr = [1, 2, 3, 4, 5];
console.log(arr.with(2, 6)); // [1, 2, 6, 4, 5]
console.log(arr); // [1, 2, 3, 4, 5]
```

### [Symbol.iterator]()

возвращает новый объект итератора массива Array Iterator, содержащий значения для каждого индекса в массиве [Читать больше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Array/Symbol.iterator)

# Назовите способы преобразования массива в объект?

- Object.assign({}, arr)
- Object.fromEntries([[0, 0], [1, 1]])
