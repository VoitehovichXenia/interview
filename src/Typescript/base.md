# Что такое TypeScript?

**TypeScript** — это ЯП, разработанный Microsoft, который является надмножеством JavaScript, добавляющим статическую типизацию для раннего обнаружения ошибок.

# Основные компоненты TypeScript?

TypeScript состоит из трех ключевых компонентов:
- языка (синтаксис, аннотации типов),
- компилятора (преобразование TS в JS, проверка ошибок)
- языковой службы (поддержка в IDE: автодополнение, навигация)
- Система типов (Type System): 
  - базовые типы (number, string, boolean, array)
  - перечисления (enum)
  - кортежи (tuple)
  - объединения (union)
  - псевдонимы типов (type) и интерфейсы (interface)

# Назовите особенности TypeScript?

Ключевые особенности TypeScript:
- Статическая типизация
- Надмножество JavaScript - TypeScript включает все возможности JS, но добавляет свои.
- Транспиляция (Компиляция) (код TypeScript не выполняется браузерами напрямую; он компилируется специальным компилятором (tsc) в чистый JavaScript)
- Объектно-ориентированное программирование (ООП)
    - Поддержка классов, интерфейсов, наследования, полиморфизма и модификаторов доступа (private, public, protected).
- Интерфейсы (Interfaces) (строгая типизацию сложных структур данных)
- Generics (Обобщения) (возможность создавать компоненты, работающие с различными типами данных)

# Плюсы использования TypeScript?

- pаннее обнаружение ошибок
- улучшенная поддержка IDE (автокомплит)
- безопасный рефакторинг
- читаемость и документация
- поддержка современных стандартов (TypeScript позволяет использовать новые возможности ECMAScript, транспилируя их в совместимый JavaScript)

# Минусы использования TypeScript?

- дополнительный этап компиляции
- увеличение времени разработки
- сложность освоения
- проблемы с библиотеками (не все JavaScript-библиотеки имеют качественные определения типов (@types), что требует ручного описания типов)
- ложное чувство безопасности

# Типы в TypeScript?

- number
- bigint
- string
- boolean
- undefined
- null
- void - отсутствие конкретного типа, основное предназначение - явно указывать на то, что у функции или метода отсутствует возвращаемое значение
- Array
    ```ts
    let a: Array<string> = ['test']
    let b: number[] = [1, 3]
    ```
- Tuples (Кортежи) представляют набор элементов, для которых уже заранее известен тип. В отличие от массивов кортежи могут хранить значения разных типов.
    ```ts
    let user: [string, number] = ['Kate', 24]
    ```
- Enum - это конструкция, состоящая из набора именованных констант, именуемая списком перечисления и определяемая такими примитивными типами, как number и string
    ```ts
    enum Citrus {
      Lemon = 2, // 2
      Orange = 4, // 4
      Lime = 6 // 6
    }
    ```
- symbol
    ```ts
    let sym: symbol = Symbol();
    ```
- any
- unknown
- never: также представляет отсутствие значения и используется в качестве возвращаемого типа функций, которые генерируют или возвращают ошибку
    ```ts
    function error(message: string): never {
      throw new Error(message);
    }
    ```
- object - представляет собой любое непримитивное значение, но TypeScript не позволит обращаться к конкретным свойствам, так как он "не знает", какие ключи там есть. 

## Разница unknown и any

Главное отличие - безопасность типов

- **any** отключает проверку типов, позволяя делать с переменной что угодно
```js
let a: any = 1; a.toUpperCase(); // Ошибки нет, но упадет в runtime.
``` 
- **unknown** требует явной проверки типа перед использованием (это безопасный аналог any, предотвращающий ошибки во время выполнения (runtime))
```js
let u: unknown = "hi"; (u as string).toUpperCase(); // Требует приведения или проверки. 
```

# Разница между типом (type) и интерфейсом (interface)?

Interface
- слияние объявлений (Declaration Merging): Если определить интерфейс с одним именем дважды, они автоматически объединятся
    ```ts
    interface User {
      name: string
      age: number
    }
    interface User {
      email: string
    }
    // interface User {
    //   name: string
    //   age: number
    //   email: string
    // }
    ```
- расширяются через extends
    ```ts
    interface User {
      name: string
      age: number
    }

    interface Employee extends User {
      position: 'developer' | 'manager'
    }
    ```
- классы могут реализовывать интерфейсы (implements)
    ```ts
    interface Cat {
      meow: () => void;
    }

    class Tiger implements Cat {
      meow() {
        console.log('meow-meow-bark');
      }
    }
    ```

Type:
- нет слияния объявлений, типы не могут быть переопределены, это вызовет ошибку.
- расширяются через пересечение &.
- type позволяет создавать сложные типы, например, объединения 
    ```ts 
    type ID = string | number
    ```

# Что такое декораторы?

Декоратор — это функция, которая позволяет добавить или изменить поведение класса, метода, свойства или параметра.

```ts
// Декоратор класса
function Logger(target: Function) {
  console.log("Class created:", target.name);
}

@Logger
class User {}

// Декоратор метода
function Log(target: any, key: string, descriptor: PropertyDescriptor) {
  const original = descriptor.value;

  descriptor.value = function (...args: any[]) {
    console.log("Calling", key);
    return original.apply(this, args);
  };
}

class User {
  @Log
  sayHi() {
    console.log("Hi");
  }
}

// Декоратор свойства
function Readonly(target: any, key: string) {
  Object.defineProperty(target, key, {
    writable: false
  });
}

class User {
  @Readonly
  name = "Alice";
}
```

# Поддерживает ли TypeScript перегрузку функций?

**Перегрузка функции (function overloading)** — это возможность в программировании создавать несколько функций с одним и тем же именем, но разными наборами параметров (типами, количеством или порядком). Компилятор автоматически выбирает нужную версию функции при вызове, основываясь на переданных аргументах, что повышает читаемость кода.

```ts
// Сигнатуры (перегрузки)
function makeDate(timestamp: number): Date;
function makeDate(m: number, d: number, y: number): Date;

// Реализация
function makeDate(mOrTimestamp: number, d?: number, y?: number): Date {
  if (d !== undefined && y !== undefined) {
    return new Date(y, mOrTimestamp, d);
  } else {
    return new Date(mOrTimestamp);
  }
}

const d1 = makeDate(12345678); // OK
const d2 = makeDate(5, 5, 5);   // OK
```

# JSX в TypeScript

TypeScript имеет три JSX режима (задается в tsconfig.json через опцию "jsx"):

1. ``preserve`` - сохраняет JSX в выходном коде, который далее передаётся на следующий шаг трансформации.  
    - выходной код получит расширение .jsx.
2. ``react`` сгенерирует React.createElement, 
    - код на выходе получит расширение .js.
3. ``react-native`` - эквивалентен ``preserve`` в том смысле, что он сохраняет весь JSX
    - вывод будет иметь расширение файла .js.

Эти режимы влияют только на стадию генерации - проверка типов не изменяется.