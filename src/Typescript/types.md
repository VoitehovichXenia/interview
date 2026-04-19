# Типы в TypeScript?

JS типы:
- ``number``
- ``bigint``
- ``string``
- ``boolean``
- ``undefined``
- ``null``

Специальные типы:
- ``void`` - отсутствие возвращаемого значения
- ``Array`` - обозначает массив (можно объявлять 2-мя способами)
    ```ts
    let a: Array<string> = ['test']
    let b: number[] = [1, 3]
    ```
- ``Tuples (Кортежи)`` - набор элементов, для которых уже заранее известен тип. В отличие от массивов кортежи могут хранить значения разных типов.
    ```ts
    let user: [string, number] = ['Kate', 24]
    ```
- ``Enum (перечисления)`` - набора именованных констант, определяемых примитивными типами, как number и string
    ```ts
    enum Citrus {
      Lemon = 2, // 2
      Orange = 4, // 4
      Lime = 6 // 6
    }
    ```
- ``symbol``
    ```ts
    let sym: symbol = Symbol();
    ```
- ``any`` - обозначает любой тип, отключает проверку типов
- ``unknown`` - обозначает любой тип, НЕ отключает проверку типов
- ``never`` - возвращаемый тип функций, которые генерируют или возвращают ошибку
    ```ts
    function error(message: string): never {
      throw new Error(message);
    }
    ```
- ``object`` - представляет собой любое непримитивное значение, но TypeScript не позволит обращаться к конкретным свойствам, так как он "не знает", какие ключи там есть

## Разница unknown и any

Главное отличие - **безопасность типов**

- **any** отключает проверку типов, позволяя делать с переменной что угодно
```js
let a: any = 1; a.toUpperCase(); // Ошибки нет, но упадет в runtime.
``` 
- **unknown** требует явной проверки типа перед использованием (это безопасный аналог any, предотвращающий ошибки во время выполнения (runtime))
```js
let u: unknown = "hi"; (u as string).toUpperCase(); // Требует приведения или проверки. 
```

## Разница между типами void, never и unknown

**void** - используется для функций, обозначает что функция ничего не возвращает

```ts
function printName(name: string): void {
  console.log(name)
}
```

**never** - используется для функций, обозначает что функция генерирует ошибку

```ts
function throwError(text: string): never {
  throw new Error(text)
}
```

**unknow** - тип, означающий что значением может быть любой тип, но требующий его проверки

```ts
let value: unknown
value = 5.345
(value as number).toFixed(1)
```

## Что такое перечисление (enum)

**Enum** - конструкция позволяющая определить набор именованных констант

1. Числовые enum
    - значение задается индексом
    ```ts
    enum Direction {
      Up, // 0
      Down, // 1
      Left, // 2
      Right // 3
    }

    let direction: Direction = Direction.Up

    enum Direction {
      Up = 2, // 2
      Down, // 3
      Left, // 4
      Right // 5
    }
    ```
2. Cтроковые enum
    ```ts
    enum Seasons {
      Winter = 'winter'
      Spring = 'spring'
      Summer = 'summer'
      Fall = 'fall'
    }

    let currentSeason: Seasons = Season.Winter
    ```

# Разница между типом (type) и интерфейсом (interface)?

## Interface

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

## Type

- нет слияния объявлений, типы не могут быть переопределены, это вызовет ошибку.
- расширяются через пересечение &.
- type позволяет создавать сложные типы, например, объединения 
    ```ts 
    type ID = string | number
    ```

# Разница между типами "Объединение" (|) и "Пересечение" (&)?

**Объединение (|)** - создает новый тип, который означает что значение может быть любым из перечесленных

```ts
type Roles = 'Admin' | 'User'

let currentUserRole: Roles = 'Admin'
currentUserRole = 'User'
```

**Пересечение (&)** - создает новый тип, который означает что значение должно соответствовать обоим типам

```ts
type Person = {
  name: string
  age: number
}

type Employee = Person & {
  company: string
  position: string
}

// TypeScript выдаст ошибку, так как отсутствует name и company
const developer: Employee = {
  name: 'Xaden',
  age: 23
}
```

# Что такое общие (обобщенные) типы (generic) в TypeScript?

Дженерики это типы нужные для описания похожих но все же отличающихся типов, позволяют не дублировать длинные конструкции

1. type generics
    ```ts
    type PaymentInfo<T> = {
      id: string
      amount: number
      currency: T
    }

    const paymentInfo_1: PaymentInfo<string> = {
      id: 'uuid_1',
      amount: 1200,
      currency: '$'
    }
    const paymentInfo_2: PaymentInfo<number> = {
      id: 'uuid_1',
      amount: 1200,
      currency: 1
    }

    // Можно задать тип по умолчанию
    type PaymentInfo<T = string> = { ... } // T — по умолчанию тип string

    const paymentInfo_1: PaymentInfo = { ... }
    const paymentInfo_2: PaymentInfo<number> = { ... }
    ```
2. interface generics
    ```ts
    interface PaymentInfo<T> {
      id: string
      amount: number
      currency: T
    }
    ```
3. function generics
    ```ts
    // Пример: есть такая перегрузка функции
    function getArray(arr: string[]): string[]
    function getArray(arr: number[]): number[] {
      console.log(arr)
      return arr
    }
    // если понадобиться добавить boolean, то по сути нужно писать еще одну перегрузку
    // с дженериком будет проще
    function getArray(arr: T[]): T[] {
      console.log(arr)
      return arr
    }

    getArray([1,2,3]) // number[]
    getArray(['1','2','3']) // string[]
    getArray([true, false]) // boolean[]
    ```

# Что такое UtilityTypes? Какие есть виды UtilityTypes?

**Utility Types (Утилиты типов) в TypeScript** — это встроенные обобщенные типы (generics)

Виды:
1. Модификация объектов
    - ``Omit<T, K>`` - исключает K из T
    - ``Pick<T, K>`` - берет только K из T
    - ``Required<T>`` - делает все ключи переданного типа required
    - ``Partial<T>`` - делает все ключи переданного типа optional
    - ``Readonly<T>`` - делает все ключи переданного типа немодифицируемыми
    - ``Record<K, T>`` - создает новый тип объекта, кде ключами будут K, а значениями T
2. Работа с типами объединений
    - ``Exclude<T, U>`` - исключает из T все значения которые можно присвоить типу U
    - ``Extract<T, U>`` - извлекает из T все значения которые можно присвоить типу U
    - ``NonNullable<T>`` - удаляет null и undefined из типа T
3. Строковые операции
    - ``Lowercase<T>``
    - ``Uppercase<T>``
    - ``Capitalize<T>``
    - ``Uncapitalize<T>``
4. Работа с функциями
    - ``ReturnType<T>`` - создает тип возвращаемого значения функции (внимание используется с ``typeof``)
    ```ts
    function getUser() {
      return {
        id: 1,
        name: "Alex",
        email: "alex@example.com"
      };
    }

    // 2. Извлекаем тип возвращаемого значения
    type User = ReturnType<typeof getUser>;
    ```
    - ``Parameters<T>`` - извлекает тип параметров функции и создает тип в виде кортежа (внимание используется с ``typeof``)
    ```ts
    function sendMessage(text: string, userId: number, isUrgent: boolean) {
      console.log(`Sending: ${text} to user ${userId}`);
    }

    // Извлекаем типы аргументов
    type SendMessageArgs = Parameters<typeof sendMessage>;

    /* 
      Результат будет кортежем:
      type SendMessageArgs = [text: string, userId: number, isUrgent: boolean]
    */

    // Теперь мы можем использовать это, например, для создания обертки:
    function logAndSend(...args: SendMessageArgs) {
      console.log("Logging arguments:", args);
      sendMessage(...args);
    }
    ```
    - ``Awaited<T>`` - используется для того чтобы получить возвращаемое значение из async функций
    ```ts
    async function fetchData() {
      return {
        id: 101,
        status: "success",
        data: [1, 2, 3]
      };
    }

    type FetchPromise = ReturnType<typeof fetchData>

    type ResponceData = Awaited<FetchPromise>
    /* 
      Результат:
      type ResponseData = {
        id: number;
        status: string;
        data: number[];
      }
    */
    ```

# Что такое narrowing в typescript

**Narrowing (сужение типов)** в TypeScript — это процесс уточнения типа переменной из более общего (например, string | number) до более конкретного (например, только string) внутри определенного блока кода

Операторы позволяющие осуществить narrowing:
- typeof
- instanceof
- in

```ts
// type predicates
// Они сообщают компилятору TypeScript, что если функция возвращает true, то переменная имеет указанный конкретный тип
function isString(a: string | number | boolean) a is string {
  return typeof a === 'string'
}
```