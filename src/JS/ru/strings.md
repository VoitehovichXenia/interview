# Методы строк в JavaScript?

## Статические методы

### fromCharCode()

``String.fromCharCode(num1[, ...[, numN]])``

num - числовые коды 

[Читать больше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/String/fromCharCode)

возвращает строку, созданную из указанной последовательности значений единиц кода UTF-16.

### fromCodePoint()

``String.fromCodePoint(num1[, ...[, numN]])``

[Читать больше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/String/fromCodePoint)

возвращает строку, созданную из указанной последовательности кодовых точек.

### raw()

``String.raw\`templateString\```

[Читать больше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/String/raw)

является теговой функцией для шаблонных строк;

```ts
let name = "Bob";
String.raw`Hi\n${name}!`; // 'Hi\nBob!`
```

## Методы строк

Deprecated:
- anchor() [Читать больше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/String/anchor)
- big() [Читать больше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/String/big)
    Аналог: [toUpperCase](#touppercase)
- blink() [Читать больше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/String/blink)
- bold() [Читать больше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/String/bold)
- fixed() [Читать больше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/String/fixed)
- fontcolor() [Читать больше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/String/fontcolor)
- fontsize() [Читать больше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/String/fontsize)
- italics() [Читать больше](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/)italics
- link() [Читать больше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/String/link)
- small() [Читать больше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/String/small)
- strike() [Читать больше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/String/strike)
- sub() [Читать больше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/String/sub)
- substr() [Читать больше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/String/substr)
- sup() [Читать больше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/String/sup)


### at()

``at(index)``

возвращает указанный символ из строки [Читать больше](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/at) 

### charAt()

``str.charAt(index)``

возвращает указанный символ из строки [Читать больше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/String/charAt)

### charCodeAt()

``charCodeAt(index)``

возвращает целое число от 0 до 65535 представляющее UTF-16 код символа по указанному индексу [Читать больше](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/charCodeAt)

### codePointAt()

``codePointAt(index)``

возвращает неотрицательное целое число, являющееся закодированным в UTF-16 значением кодовой точки [Читать больше](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/codePointAt)

```ts
"ABC".codePointAt(1); // 66
```

### concat()

``concat(string)``

объединяет текст из двух или более строк и возвращает новую строку [Читать больше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/String/concat)

### endsWith()

``endsWith(string)``

позволяет определить, заканчивается ли строка символами указанными в скобках, возвращая, соответственно, true или false [Читать больше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/String/endsWith)

### includes()

``includes(string)``

проверяет с учётом регистра, содержит ли строка заданную подстроку, и возвращает, соответственно true или false [Читать больше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/String/includes)

### indexOf()

``indexOf(string)``

возвращает индекс первого вхождения указанного значения в строку, на котором он был вызван, начиная с индекса fromIndex. Возвращает -1, если значение не найдено [Читать больше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/String/indexOf)

### isWellFormed()

``isWellFormed()``

Возвращает true если строка не содержит специальных unicode символов [Читать больше](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/isWellFormed)

### lastIndexOf()

``lastIndexOf(string)``

возвращает индекс последнего вхождения указанного значения в строку, на котором он был вызван, или -1, если ничего не было найдено [Читать больше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/String/lastIndexOf)

### localeCompare()

``localeCompare(string)``

возвращает число, указывающее, где должна находиться эта строка при сортировке (до (-1), после (1) или в том же самом месте (0), что и строка, переданная в качестве параметра) [Читать больше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/String/localeCompare)

```ts
const a = "réservé"; // With accents, lowercase
const b = "RESERVE"; // No accents, uppercase

console.log(a.localeCompare(b));// 1
```

### match()

``match(RegExp)``

возвращает получившиеся совпадения при сопоставлении строки с регулярным выражением [Читать больше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/String/match)

```ts
const str = "Смотри главу 3.4.5.1 для дополнительной информации";
const re = /смотри (главу \d+(\.\d)*)/i;
const found = str.match(re);

console.log(found);

// выведет [ 'Смотри главу 3.4.5.1',
//           'главу 3.4.5.1',
//           '.1',
//           index: 0,
//           input: 'Смотри главу 3.4.5.1 для дополнительной информации' ]

// 'Смотри главу 3.4.5.1' - это полное сопоставление
// 'главу 3.4.5.1' - первое значение, сопоставленное с группой "(главу \d+(\.\d)*)".
// '.1' - это последнее значение, сопоставленное с группой "(\.\d)".
// Свойство 'index' содержит значение (0) индекса совпадения
// относительно начала сопоставления
// Свойство 'input' содержит значение введённой строки.
```

### matchAll()

``matchAll(RegExp)``

возвращает итератор по всем результатам при сопоставлении строки с регулярным выражением [Читать больше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/String/matchAll)

```ts
const regexp = /t(e)(st(\d?))/g;
const str = "test1test2";

const array = [...str.matchAll(regexp)];

console.log(array[0]); // ["test1", "e", "st1", "1"]
```

### normalize()

возвращает форму нормализации Юникода данной строки [Читать больше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/String/normalize)

### padEnd()

``padEnd(number)``

заполняет строку указанной строкой (повторяя её необходимое количество раз) так, чтобы результирующая строка достигла указанной длины. Заполнение происходит с конца исходной строки [Читать больше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/String/padEnd)

### padStart()

``padStart(number)``

заполняет строку указанной строкой (повторяя её необходимое количество раз) так, чтобы результирующая строка достигла указанной длины. Заполнение происходит с начала исходной строки [Читать больше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/String/padStart)

### repeat()

``repeat(number)``

конструирует и возвращает новую строку, содержащую указанное количество соединённых вместе копий строки, на которой он был вызван [Читать больше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/String/repeat)

### replace()

``replace(regexp|substr, newSubStr|function[, flags])``

возвращает новую строку с некоторыми или всеми сопоставлениями с шаблоном, заменёнными на заменитель [Читать больше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/String/replace)

### replaceAll()

``replaceAll(substring|regexp, replacement)``

возвращает новую строку со всеми совпадениями pattern , который меняется на replacement [Читать больше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/String/replaceAll)

### search()

``search([regexp])``

выполняет поиск сопоставления между регулярным выражением и этой строкой [Читать больше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/String/search)

### slice()

``slice(startIndex, endIndex?)``

извлекает часть строки и возвращает новую строку без изменения оригинальной строки [Читать больше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/String/slice)

### split()

``split([separator[, limit]])``

разбивает строку на массив строк путём разделения строки указанной подстрокой[Читать больше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/String/split)

### startsWith()

``startsWith(string)``

помогает определить, начинается ли строка с символов указанных в скобках, возвращая, соответственно, true или false [Читать больше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/String/startsWith)

### substring()

``substring(startIndex, endIndex?)``

возвращает часть строки от начального индекса до конечного (не включая его), или, если конечный индекс не указан, — до конца строки [Читать больше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/String/substring)

### toLocaleLowerCase()

возвращает значение строки, на которой он был вызван, преобразованное в нижний регистр согласно правилам преобразования регистра локали [Читать больше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/String/toLocaleLowerCase)

### toLocaleUpperCase()

возвращает значение строки, на которой он был вызван, преобразованное в верхний регистр согласно правилам преобразования регистра локали [Читать больше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/String/toLocaleUpperCase)

### toLowerCase()

возвращает значение строки, на которой он был вызван, преобразованное в нижний регистр [Читать больше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/String/toLowerCase)

### toString()

возвращает строку, представляющую указанный объект [Читать дальше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/String/toString)

```ts
const x = new String("Hello");

console.log(x.toString()); // 'Hello'
```

### toUpperCase()

возвращает значение строки, на которой он был вызван, преобразованное в верхний регистр [Читать больше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/String/toUpperCase)

### toWellFormed()

возвращает строку, в которой все одиночные спец символы этой строки заменены символом замены Unicode U+FFFD [Читать больше](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/toWellFormed)\

### trim()

удаляет пробельные с имволы с начала и конца строки [Читать больше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/String/trim)

### trimEnd() trimRight()

удаляет пробельные символы с конца строки [Читать больше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/String/trimEnd)

### trimStart() trimLeft()

удаляет пробельные символы с начала строки [Читать больше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/String/trimStart)

### valueOf()

возвращает примитивное значение объекта String [Читать больше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/String/valueOf)

```ts
var x = new String("Привет, мир");
console.log(x.valueOf()); // Отобразит 'Привет, мир'
```

### [Symbol.iterator]()

Метод [@@iterator]() возвращает новый объект итератора Iterator, который проходит по кодовым точкам строкового значения, возвращая каждую кодовую точку в виде строкового значения [Читать больше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/String/Symbol.iterator)

```ts
var string = "ABC";

for (var v of string) {
  console.log(v);
}
// A
// B
// C
```