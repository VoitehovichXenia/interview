# Объект

**Объект** - тип данных, представляющий собой неупорядоченную коллекцию свойств (properties), где каждое свойство это пара ключ - значение

Под капотом в зависимости от движка и оптимизации представлен разными структурами данных, но если обобщить то это хэш-таблица

Имена свойств оформляются по [правилам переменных](../js.md#переменные), однако зарезервированные ключевые слова использовать разрешено. При этом есть специальное свойство ``__proto__`` которое всегда должно быть объектом (ссылкой на объект) и его лучше не модифицировать

## Объект первого класса

**Объект первого класса (англ. first-class object, first-class entity, first-class citizen)** - элементы, которые могут быть переданы как параметр, возвращены из функции, присвоены переменной.

## Объекты в JS по происхождению

**Нативные объекты (Native Objects)**— это встроенные в язык JavaScript компоненты, описанные в спецификации ECMAScript (например, Object, Array, Date, Math).

- Обычные объекты (Object)
- Массивы (Array)
- Функции (Function)
- Дата (Date)
- Коллекции (Map, Set)
- Регулярные выражения (RegExp)
- Math и JSON

**Хост-объекты (Host Objects)** — это объекты, предоставляемые средой выполнения (браузер или Node.js), такие как window, document, setTimeout, fetch. 

### Создание объекта

- object literal
- Конструктор Object()
- Object.create(prototype)
- Функция-конструктор (для создания множества однотипных объектов)
- Классы (ES6+)

```ts
// object literal
const obj = { key: 'value' };

// Конструктор Object()
const obj_2 = new Object();

// Object.create(prototype)
const animal = { jumps: false }
const obj_3 = Object.create(animal);

// Функция-конструктор
function Person (name, age) {
    this.name = name;
    this.age = age
}
const obj_4 = new Person('Kate', 20)

// Классы
class Employee {
    constructor(title, salary) {
        this.title = title
        this.salary = salary
    }
}
const obj_5 = new Employee('Senior frontend developer', 'NDA')
```

### Обращение к свойству / Вставка нового свойства

- через точку по имени свойства
```ts
const obj = { status: 'pending' }
console.log(obj.status)

// Вставка
obj.done = false
```
- через квадратные скобки (составные свойства, вычисляемые свойства, переменные)
```ts
const name = 'Julia'
const obj = { 'loading status': 'pending', admin_Julia: 'ready', admin_Jack: 'in-progress', text: 'Lorem ipsum' }

// Составное свойство
console.log(obj['loading status'])
// Вычисляемое свойство
console.log(obj[`admin_${name}`])

// Переменная
const key = 'text'
console.log(obj[key]);

// Вставка
obj['admin_Jane'] = 'ready'
```

### Удаление

- delete
```ts
const obj = { status: 'pending' }

delete obj.status
```

### Проверка существования свойства

- через undefined
    ```ts
    const obj = { status: 'pending' }

    console.log(obj.admin === undefined) // true
    ```
- через оператор in
    ```ts
    const obj = { status: 'pending' }

    console.log('admin' in obj) // false
    console.log('toString' in obj) // true
    ```
- через [hasOwnProperty](#hasownproperty)

# Методы объекта

## Статические

### assign()

``Object.assign(target, source);``

используется для копирования значений всех собственных перечисляемых свойств из одного или более исходных объектов в целевой объект. После копирования он возвращает целевой объект [Читать дальше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Object/assign)

```js
const target = { a: 1, b: 2 };
const source = { b: 4, c: 5 };

const returnedTarget = Object.assign(target, source);

console.log(target); // { a: 1, b: 4, c: 5 }
console.log(returnedTarget === target); // true
```

### create()

``Object.create(proto[, propertiesObject])``

создаёт новый объект с указанным прототипом и свойствами [Читать дальше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Object/create)

```js
const o = Object.create(Object.prototype, {
  // foo является рядовым 'свойством-значением'
  foo: { writable: true, configurable: true, value: "привет" },
  // bar является свойством с геттером и сеттером (свойством доступа)
  bar: {
    configurable: false,
    get: function () {
      return 10;
    },
    set: function (value) {
      console.log("Установка `o.bar` в", value);
    },
  },
})
```

### defineProperties()

``Object.defineProperties(obj, props)``

``props`` - объект, свойства которого представляют собой дескрипторы для создаваемых или изменяемых свойств. 

Дескрипторы свойств обладают следующими дополнительными ключами:
- **configurable** (``true`` - тип этого дескриптора свойства может быть изменён и если свойство может быть удалено из содержащего его объекта; по умолчанию ``false``)
- **enumerable** (``true`` - свойство можно увидеть через перечисление свойств содержащего его объекта; по умолчанию ``false``)
- **value** (значение, по умолчанию ``undefined``)
- **writable** (``true`` - значение может быть изменено с помощью оператора присваивания, по умолчанию ``false``)
- **get** (функция, используемая как геттер свойства, либо ``undefined``, если свойство не имеет геттера, по умолчанию ``undefined``)
- **set** (функция, используемая как сеттер свойства, либо ``undefined``, если свойство не имеет сеттера, по умолчанию ``undefined``)

определяет новые или изменяет существующие свойства, непосредственно на объекте, возвращая этот объект [Читать дальше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperties)

```js
Object.defineProperties(obj, {
  property1: {
    value: true,
    writable: true,
  },
  property2: {
    value: "Hello",
    writable: false,
  },
  // и т.д.
});
```

### defineProperty()

```js
Object.defineProperty(object1, "property1", {
  value: 42,
  writable: false,
});
```

определяет новое или изменяет существующее свойство объекта и возвращает этот объект [Читать дальше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty)

### entries()

метод возвращает массив собственных перечисляемых свойств указанного объекта в формате ``[key, value]`` [Читать дальше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Object/entries)

### freeze()

замораживает объект, это значит, что он:
- предотвращает добавление новых свойств к объекту,
- удаление старых свойств из объекта
- изменение существующих свойств или значения их атрибутов перечисляемости, настраиваемости и записываемости

Это работает на поверхностном уровне, т.е. не действует на вложенные объекты. В "use strict" при попытке изменить замороженный объект => TypeError [Читать дальше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Object/freeze)

### fromEntries()

преобразует список пар ключ-значение в объект [Читать дальше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Object/fromEntries)

### getOwnPropertyDescriptor()

возвращает дескриптор свойства для собственного свойства (то есть такого, которое находится непосредственно в объекте, а не получено через цепочку прототипов) переданного объекта [Читать дальше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertyDescriptor)

```js
const o = { bar: 42 };
const d = Object.getOwnPropertyDescriptor(o, "bar");
console.log(d) // { configurable: true, enumerable: true, value: 42, writable: true }
```

### getOwnPropertyDescriptors()

возвращает все собственные дескрипторы свойств данного объекта [Читать дальше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertyDescriptors)

### getOwnPropertyNames()

возвращает массив со всеми свойствами (независимо от того, перечисляемые они или нет), найденными непосредственно в переданном объекте [Читать дальше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertyNames)

```js
const object1 = { a: 1, b: 2, c: 3 };

console.log(Object.getOwnPropertyNames(object1)); // ["a", "b", "c"]
```

### getOwnPropertySymbols()

возвращает массив всех символьных свойств, найденных непосредственно на переданном объекте [Читать дальше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertySymbols)

```js
const object1 = {};
const a = Symbol("a");
const b = Symbol.for("b");

object1[a] = "localSymbol";
object1[b] = "globalSymbol";

const objectSymbols = Object.getOwnPropertySymbols(object1);

console.log(objectSymbols.length); // 2
```

### getPrototypeOf()

возвращает прототип (то есть, внутреннее свойство [[Prototype]]) указанного объекта [Читать дальше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Object/getPrototypeOf)

### groupBy()

группирует элементы заданного итерируемого объекта в соответствии со строковыми значениями, возвращаемыми callback [Читать дальше](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/groupBy)

```js
const inventory = [
  { name: "asparagus", type: "vegetables", quantity: 9 },
  { name: "bananas", type: "fruit", quantity: 5 },
  { name: "goat", type: "meat", quantity: 23 },
  { name: "cherries", type: "fruit", quantity: 12 },
  { name: "fish", type: "meat", quantity: 22 },
];

const result = Object.groupBy(inventory, ({ quantity }) =>
  quantity < 6 ? "restock" : "sufficient",
);
console.log(result.restock); // [{ name: "bananas", type: "fruit", quantity: 5 }]
```

### hasOwn()

возвращает true, если указанный объект имеет указанное свойство в качестве собственного свойства. Если свойство наследуется или не существует, метод возвращает false [Читать дальше](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/hasOwn)

### is()

``Object.is(value1, value2);``

определяет, являются ли два значения одинаковыми значениями [Читать дальше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Object/is)

```js
Object.is("foo", "foo"); // true
Object.is(window, window); // true

Object.is("foo", "bar"); // false
Object.is([], []); // false

var test = { a: 1 };
Object.is(test, test); // true

Object.is(null, null); // true

// Специальные случаи
Object.is(0, -0); // false
Object.is(-0, -0); // true
Object.is(NaN, 0 / 0); // true
```

### isExtensible()

определяет, является ли объект расширяемым (то есть, можно ли к нему добавлять новые свойства) [Читать дальше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Object/isExtensible)

### isFrozen()

определяет, был ли объект заморожен [Читать дальше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Object/isFrozen)

### isSealed()

определяет, является ли объект запечатанным [Читать дальше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Object/isSealed)

### keys()

возвращает массив из собственных перечисляемых свойств переданного объект [Читать дальше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Object/keys)

### preventExtensions()

предотвращает добавление новых свойств к объекту (то есть, предотвращает расширение этого объекта в будущем) [Читать дальше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Object/preventExtensions)

### seal()

запечатывает объект, предотвращая добавление новых свойств к объекту и делая все существующие свойства не настраиваемым [Читать дальше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Object/seal)

### setPrototypeOf()

устанавливает прототип (то есть, внутреннее свойство ``[[Prototype]]``) указанного объекта в другой объект или null. Очень ресурсозатратный мето [Читать дальше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Object/setPrototypeOf)

### values()

возвращает массив значений перечисляемых свойств объекта [Читать дальше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Object/values)

## Методы экземпляра

### hasOwnProperty()

возвращает true только если свойство принадлежит самому объекту, а не его прототипу [Читать дальше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Object/hasOwnProperty)

```ts
const obj = { status: 'pending' }

console.log(obj.hasOwnProperty('status')) // true
console.log(obj.hasOwnProperty('toString')) // false
```

### isPrototypeOf()

проверяет, входит ли объект в цепочку прототипов другого объекта [Читать дальше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Object/isPrototypeOf)

### propertyIsEnumerable()

возвращает логическое значение, указывающее, является ли указанное свойство перечисляемым [Читать дальше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Object/propertyIsEnumerable)

### toLocaleString()

```js
({ a: 'a' }).toLocaleString(); // '[object Object]'
```
возвращает строку, представляющую объект [Читать дальше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Object/toLocaleString)

### toString()

возвращает строку, представляющую объект [Читать дальше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Object/toString)

### valueOf()

возвращает примитивное значение указанного объекта [Читать дальше](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Object/valueOf)

# Разница между Object.freeze() и Object.seal()?

Основная разница заключается в возможности редактирования существующих свойств. 

Object.seal() (запечатывание) запрещает добавление/удаление свойств, но позволяет менять значения существующих.
- Предотвращает добавление новых свойств.
- Предотвращает удаление существующих свойств.
- Позволяет изменять значения существующих свойств.
- Делает все свойства неконфигурируемыми (configurable: false).

Object.freeze() (замораживание) делает объект полностью неизменяемым: запрещены добавление, удаление и изменение значений свойств. 
- Выполняет все действия seal().
- Запрещает изменение значений существующих свойств (writable: false).
- Делает объект полностью иммутабельным (неизменяемым). 

```javascript
const userSeal = { name: 'Ivan' };
Object.seal(userSeal);
userSeal.name = 'Dmitry'; // Работает
userSeal.age = 25;       // Не работает
console.log(userSeal);    // { name: 'Dmitry' }

const userFreeze = { name: 'Ivan' };
Object.freeze(userFreeze);
userFreeze.name = 'Dmitry'; // Не работает
userFreeze.age = 25;       // Не работает
console.log(userFreeze);   // { name: 'Ivan' }
```

# Плюсы и минусы иммутабельности? Как достичь иммутабельности в JS?

**Неизменяемым (англ. immutable)** называется объект, состояние которого не может быть изменено после создания. Результатом любой модификации такого объекта всегда будет новый объект, при этом старый объект не изменится.

Плюсы иммутабельности:
- предсказуемость (данные не изменяются неожиданно в других частях программы)
- производительность UI в React/Redux (сравнение ссылок)
- легкая отладка (проще отслеживать изменения состояния (time-travel debugging))
- отсутствие побочных эффектов (side-effects) при передаче объектов

Минусы иммутабельности:
- расход памяти (новые копии объектов, при необходимости изменений)
- производительность (дополнительные расходы на копирование больших структур данных)
- сложность синтаксиса (работа с глубоко вложенными объектами)

## Как достичь иммутабельности в JavaScript

1. **Spread-оператор** - создает поверхностную копию
        ```javascript
        const user = { name: 'Ivan', age: 25 };
        const updatedUser = { ...user, age: 26 }; // Новый объект
        ```
2. **Методы массивов**, не мутирующие оригинал (map, filter, reduce, slice, concat).
        ```javascript
        const list = [1, 2, 3];
        const newList = [...list, 4]; // Добавление
        const filteredList = list.filter(item => item !== 2); // Удаление
        ```
3. **Object.freeze()** — замораживает объект, делая его свойства неизменяемыми (поверхностная заморозка).
```javascript
const config = Object.freeze({ url: 'localhost' });
// config.url = 'site.com'; // В строгом режиме (strict mode) вызовет ошибку
```
4. Библиотеки для работы с Immutable данными (Immer.js, Immutable.js)
```js
// Пример с Immer
const nextState = produce(baseState, draft => { draft.user.age = 30; })
```
5. **Object.assign()** — для создания новых объектов на основе старых.
```javascript
const newUser = Object.assign({}, oldUser, { name: 'New' });
```

# Как можно создать объекты с приватными свойствами и методами в JavaScript?

В JavaScript объекты с приватными свойствами и методами создаются через классы (ES 2020+) с использованием префикса # перед именем. 

## Приватные поля класса (#field)

```javascript
class Animal {
  #name; // Приватное свойство

  constructor(name) {
    this.#name = name;
  }

  getName() {
    return this.#name; // Доступ разрешен внутри
  }
}
const cat = new Animal("Cat");
console.log(cat.getName()); // "Cat"
// console.log(cat.#name); // Ошибка: Private field '#name' must be declared in an enclosing class
```

## Приватные методы класса (#method)

Префикс # делает метод доступным только внутри класса, идеально для служебных функций.

```javascript
class MyClass {
  #privateMethod() {
    return "Секрет";
  }

  publicMethod() {
    return this.#privateMethod();
  }
}
```

## Замыкания (до ES2020/для функционального стиля):

```javascript
function User(name) {
  let _name = name; // Защищенная переменная
  this.getName = function() {
    return _name;
  };
}
const user = new User("Alex");
console.log(user._name); // undefined
console.log(user.getName()); // "Alex"
```

# Разница между глубокой (deep) и поверхностной (shallow) копиями объекта? Как сделать каждую из них?

Поверхностная копия (shallow) копирует только примитивы (значения), а объекты и массивы внутри копируются по ссылке.

Способы:
- Object.assign():
    ```js
    const copy = Object.assign({}, original);
    ```
- Spread-оператор (...)
    ```js
    const copy = { ...original };
    ```

Глубокая копия (deep) рекурсивно копирует все вложенные структуры, создавая полностью независимый объект. 

Способы:
- ``structuredClone()`` (современный, нативный способ):
    ```javascript
    const deep = structuredClone(original);
    ```
- JSON.parse(JSON.stringify()) (не копирует функции/undefined):
    ```javascript
    const deep = JSON.parse(JSON.stringify(original));
    ```
- библиотеки (например, lodash.clonedeep).

Пример функции осуществляющей глубокое копирование
```js
function deepClone(obj, target = {}) {
    if (obj === null || typeof obj !== 'object') {
        return obj
    }

    if (obj instanceof Date) {
        return new Date(obj)
    }

    if (obj instanceof RegExp) {
        return new RegExp(obj.source, obj.flags)
    }

    if (obj instanceof Set) {
        const result = new Set()
        obj.forEach(value => {
            result.add(deepClone(value));
        });
    }

    if (obj instanceof Map) {
        const result = new Map()
        obj.forEach((value, key) => {
            result.set(key, deepClone(value));
        });
    }

    const result = Array.isArray(obj) ? [] : {}
    Object.keys(obj).forEach(key => {
        result[key] = deepClone(obj[key])
    })

    return result
}
```

# Зачем нужен конструктор Proxy?

Объект Proxy «оборачивается» вокруг другого объекта и может перехватывать (и, при желании, самостоятельно обрабатывать) разные действия с ним, например чтение/запись свойств и другие.

```js
let proxy = new Proxy(target, handler);
```

- **target** – это объект, для которого нужно сделать прокси
- **handler** – конфигурация прокси: объект с «ловушками» («traps»): методами, которые перехватывают разные операции, например, ловушка get – для чтения свойства из target, ловушка set – для записи свойства в target и так далее.

При операциях над proxy, если в handler имеется соответствующая «ловушка», то она срабатывает, и прокси имеет возможность по-своему обработать её, иначе операция будет совершена над оригинальным объектом target.

Список методов ловушек:
| Внутренний метод | Ловушка | Что вызывает |
[[Get]] | get | чтение свойства |
[[Set]] | set | запись свойства |
[[HasProperty]] | has	| оператор in |
[[Delete]] | deleteProperty | оператор delete |
[[Call]] | apply | вызов функции |
[[Construct]] | construct | оператор new |
[[GetPrototypeOf]] | getPrototypeOf | Object.getPrototypeOf |
[[SetPrototypeOf]] | setPrototypeOf | Object.setPrototypeOf |
[[IsExtensible]] | isExtensible | Object.isExtensible |
[[PreventExtensions]] | preventExtensions | Object.preventExtensions |
[[DefineOwnProperty]] | defineProperty | Object.defineProperty, Object.defineProperties |
[[GetOwnProperty]] | getOwnPropertyDescriptor | Object.getOwnPropertyDescriptor, for..in, Object.keys/values/entries |
[[OwnPropertyKeys]] | ownKeys | Object.getOwnPropertyNames, Object.getOwnPropertySymbols, for..in, Object.keys/values/entries |

## Инварианты

JavaScript налагает некоторые условия (инварианты) на реализацию внутренних методов и ловушек.

Большинство из них касаются возвращаемых значений, например метод [[Set]] должен возвращать true, если значение было успешно записано, иначе false.

Применение ловушки get (реализация «значения по умолчанию»)
```js
let numbers = [0, 1, 2];

// прокси перезаписывает переменную
numbers = new Proxy(numbers, {
  get(target, prop) {
    if (prop in target) {
      return target[prop];
    } else {
      return 0; // значение по умолчанию
    }
  }
});

console.log( numbers[1] ); // 1
console.log( numbers[123] ); // 0 (нет такого элемента)
```

Валидация с ловушкой «set» (например, нужно сделать массив исключительно для чисел, если в него добавляется значение иного типа, то это должно приводить к ошибке)
```js
let numbers = [];

numbers = new Proxy(numbers, { // (*)
  set(target, prop, val) { // для перехвата записи свойства
    if (typeof val == 'number') {
      target[prop] = val;
      return true;
    } else {
      return false;
    }
  }
});

// встроенная функциональность массива по-прежнему работает
// pначения добавляются методом push
numbers.push(1); // добавилось успешно
numbers.push(2); // добавилось успешно
console.log("Длина: " + numbers.length); // 2

numbers.push("тест"); // TypeError (ловушка set на прокси вернула false)
```

Подробнее про другие ловушки - [читать здесь](https://learn.javascript.ru/proxy)
