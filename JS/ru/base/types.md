# Типы данных в JavaScript

**Тип данных** - это набор контрактов, определяющих множество допустимых значений, набор операций над ними, а также способ хранения в памяти компьютера.

Примитивы:
- **number** (double (IEEE-754), Int32, SMI (small integers))
- **bigInt**
- **string**
- **boolean**
- **undefined**
- **null**
- **symbol** (unique unmutable identifyer)

Ссылочные:
- **object**

## Разница между null и undefined?

undefined - переменная объявлена, но не инициализирована
null - намеренно присвоеное пустое значение

# Определение типов

Тип можно определить с помощью оператора **typeof** => возвращает строку с названием типа

```ts
typeof 'test' // 'string
typeof 2 // 'number'
typeof NaN // 'number'
typeof Infinity // 'number'
typeof false // 'boolean'
typeof 1n // 'bigint'
typeof undefined // 'undefined'

// Особые случаи
typeof null // 'object'
// В самой первой реализации JavaScript значения хранились как тип + данные
// Тип определялся по нескольким битам: 000 — объект, 1 — число и т.д.
// Значение null на низком уровне представлялось как нулевой указатель (0x00) и в результате первые биты совпадали с object
typeof (() => {}) // 'function'
// В самой первой реализации JavaScript нужно было удобно различать: обычные объекты и вызываемые сущности (функции)
// Если бы было так: typeof (() => {}) === "object" - пришлось бы каждый раз дополнительно проверять, можно ли вызвать значение.
// Поэтому Если значение имеет внутренний метод [[Call]], то typeof возвращает "function"
```

# Преобразование типов

### Числовое преобразование

- **parseInt(string, radix)** - преобразует строку в целое число; radix - основание системы счисления; идет посимвольно, если первый символ не "-" и не число - NaN
```ts
parseInt('10.99') // 10
parseInt('1test0.99test') // 1
parseInt('test') // NaN
```
- **parseFloat(string)** - преобразует строку в целое или дробное число; идет посимвольно, если первый символ не "-", не "." и не число - NaN
```ts
parseFloat('10.99') // 10.99
parseFloat('1.09.8test.99test') // 1.09
parseFloat('test') // NaN
```
- **Number(value: string | null | undefined)** - преобразует значение в число, если это  строка и в строке есть любой не числовой символ или строка целиком не может быть интерпретирована как число - NaN
```ts
Number(true) // 1
Number(false) // 0
Number(null) // 0
Number(undefined) // NaN
Number('10.99') // 10.99
Number('56') // 56
Number('56lbs') // NaN
Number({}) // NaN
Number([]) // 0
// сложные значения сначала приводятся к примитиву посредством цепочки выполнения методов valueOf() затем toString()
([]).valueOf() => []
([]).toString() => ''
({}).valueOf() => {}
({}).toString() => '[object Object]'
``` 

### Строковые преобразования

- **String()** - преобразует значение в строку
- **toString()** - метод, который преобразует значение в строку
```ts
(2).toString() // '2'
(1n).toString() // '1'
(true).toString() // 'true'
({}).toString() // '[object Object]'
(null).toString() // ! TypeError
(undefined).toString() // ! TypeError
```
- **конкатенация** с строкой
```ts
2 + '' // '2'
```

### Логические преобразования

- **Boolean()**
0, '', false, NaN, null, undefined => false
[], {} => всегда true
- **!** (**отрицание**, преобразует операнд в boolean и возвращает противоположный boolean, т.е если значение true => false и наоборот) и **!!** (**двойное отрицание**, преобразует операнд в boolean)
```ts
!1 // false
!0 // true
!!null // false
!'false' // true
```