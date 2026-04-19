# Что такое TypeScript?

**TypeScript** — это ЯП, разработанный Microsoft, который является надмножеством JavaScript, добавляющим статическую типизацию для раннего обнаружения ошибок.

# Основные компоненты TypeScript?

TypeScript состоит из ключевых компонентов:
- языка (синтаксис, аннотации типов),
- компилятора (преобразование TS в JS, проверка ошибок)
- языковой службы (поддержка в IDE: автодополнение, навигация)

# Назовите особенности TypeScript?

Ключевые особенности TypeScript:
- Статическая типизация
- Надмножество JavaScript - TypeScript включает все возможности JS, но добавляет свои.
- Транспиляция (Компиляция) (код TypeScript не выполняется браузерами напрямую; он компилируется специальным компилятором (tsc) в чистый JavaScript)
- Объектно-ориентированное программирование (ООП) - Поддержка классов, интерфейсов, наследования, полиморфизма и модификаторов доступа (private, public, protected).
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

# Типы typescript файлов

- **.ts** - содержат исполняемый код, аннотации типов и компилируются в JS
- **.d.ts (declaration files)** - содержат только описания типов (интерфейсы, структуры) и нужны для типизации JS-библиотек или глобальных переменных, не генерируя JS-код при компиляции

# Что такое директивы с тремя наклонными чертами (Triple-Slash Directives), их типы?

Директивы с тремя наклонными чертами (Triple-Slash Directives) в TypeScript — это однострочные комментарии, содержащие XML-теги, которые используются компилятором (tsc) для:
  - управления процессом компиляции
  - подключения зависимостей и типов

Основные типы директив:
- ``/// <reference path="..." />``
    - необходимо включить другой файл TypeScript (.ts или .d.ts) в процесс компиляции. 
    - помогает разрешать зависимости между файлами
- ``/// <reference types="..." /> ``
    - cсылка на типы из конкретного пакета (обычно из node_modules/@types). 
    - позволяет подключить декларации типов из внешних библиотек, не импортируя их явно в коде.
- ``/// <reference lib="..." />`` 
    - ссылка на встроенные библиотечные файлы TypeScript (например, lib="es2015"), которые определяют типы для окружения (DOM, ES6 и т.д.).
- ``/// <reference no-default-lib="true"/>``
    - помечает файл как библиотеку по умолчанию
    - компилятор интерпретирует это как то, что этот файл не стоит включать в компиляцию
- ``/// <amd-module />``: 
    - Используется при компиляции в модули AMD (Asynchronous Module Definition, устаревший формат, ныне используется ESM), позволяя задать имя модуля, отличное от имени файла.
    ```ts
    /// <amd-module name="NamedModule"/>
    export class C {}

    // js
    define("NamedModule", ["require", "exports"], function (require, exports) {
      var C = (function () {
        function C() {}
        return C;
      })();
      exports.C = C;
    });
    ```
- ``/// <amd-dependency />``: 
    - Позволяет добавить не-TS зависимость (например, CSS или другой JavaScript-модуль), которая должна быть загружена в AMD-модуль.
- ``preserve="true"``
    - параметр предотвращающий удаление директивы компилятором из выходных данных.

# Что такое внешние объявления переменных (ambient declaration) в TypeScript?

Внешнее объявление вводит переменную в область видимости TypeScript, но никак не влияет на скомпилированный JavaScript код.

Например, по умолчанию компилятор TypeScript выдаст ошибку при использовании необъявленных переменных. Для добавления общих переменных, определенных браузером, можно использовать внешние определения.

```ts
declare var document; // var document: any;

document.title = 'Hello'
```

# Для чего в TypeScript используют ключевое слово declare?

declare используется для объявления внешних переменных (ambient declaration)

Основные сценарии использования declare:

## Глобальные переменные

```ts
declare global {
  interface Window {
    dataLayer?: object[]
  }
}
```

## Внешние библиотеки (JS)

```ts
declare const lib: any;

const result = lib.calculate(34, 56)
```

## Файлы определений (.d.ts)

В .d.ts файлах все объявления верхнего уровня неявно считаются или явно помечаются declare для описания форм существующих объектов

```ts
// global.d.ts
declare function add(a:number, b:number): number
// Можно и просто так
function add(a:number, b:number): number
```

## Работа с некодовыми файлами

```ts
// .d.ts
declare "*.png" {
  const value: string;
  export default value;
}

// другой файл
import logo from './pic.png'
```

## Расширение модулей

```ts
import 'shapes-module';

declare module 'shapes-module' {
  export interface Square {
    side: number;
  }
}
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

# Разница между внутренним (Internal Module) и внешним модулями (External Module)?

Разница заключается в видимости для модульной системы

1. **Внутренние модули (Namespases)**
    - используются для логической группировки кода внутри одного файла или нескольких файлов, которые в итоге объединяются в единый глобальный объект.
    - доступны во всем проекте, один и тот же namespace может быть объявлен в разных файлах несколько раз, а компилятор объединит их
    ```ts
    namespace Validation {
      export interface StringValidator {
          isValid(s: string): boolean;
      }
    }
    // Использование:
    let myValidator: Validation.StringValidator;
    ```
2. **Внешние модули (ESM)**
    - код внутри модуля доступен только локально или там куда был импортирован

Пример использования namespases

```ts
// Shapes.ts
namespase Shapes {
  export class Square {
    constructor(public side: number) {}
  }
}
// ShapesHelper.ts
/// <reference path="Shapes.ts">
namespace Shapes {
  export class Circle {
    constructor(public radius: number) {}
  }
}
// app.ts
/// <reference path="Shapes.ts">
/// <reference path="ShapesHelper.ts">

const mySquare = new Shapes.Square(5)
const myCircle = new Shapes.Circle(10)

console.log(mySquare.side) // 5
```

# Для чего в TypeScript используется NoImplicitAny?

**NoImplicitAny** - параметр конфигурации компилятора запрещающий использовать тип any

# Какие области видимости доступны в TypeScript?

- глобальная
- функциональная (переменные внутри функции)
- блочная (переменные внутри блоков кода if/else, try/catch ...)

# Что такое .map файл, как и зачем его использовать?

**Файлы .map (Source Maps)** в TypeScript — это JSON файлы с картами источников, связывающие скомпилированный JavaScript-код с исходным TypeScript-кодом.

Зачем использовать:
- Отладка (Debugging) - можно устанавливаеть точки останова (breakpoints) непосредственно в исходном TypeScript-файле
- Читаемость - без карт инструментов мы видим сгенерированный JS, а с ними — привычный TS-код
- Логирование ошибок: Ошибки (stack traces) в консоли указывают на номера строк в исходном TS-файле

# Можно ли использовать TypeScript в серверной разработке?

Да, так как в серверной части используются чаще CommonJS модули, необходимо просто настроить компилятор, который будет преобразовывать код в валидный JS

```json
{
  "compilerOptions": {
    "target": "es5",
    "module": "commonjs",
    "declaration": true,
    "outDir": "build"
  }
}
```

# Как вы отлавливаете ошибки в TypeScript коде?

1. базовый try/catch
2. кастомные ошибки
    ```ts
    class ApiError extends Error {
      consructor(public status: number, message: string) {
        super(message)
      }
    }

    if (!res.ok) {
      throw new ApiError(res.status, 'Request failed');
    }

    catch(e) {
      if (e instanceof ApiError) {
        console.log(e.status)
      }
    }
    ```
3. result pattern
    ```ts
    type Result<T> = { status: 'success', data: T } | {status: 'error', error: Error }

    type User = { id: string, name: string }

    const handleFetch = async(): Promise<Result<User>> => {
      try {
        const res = await fetch('url')
        if (!res.ok) throw new Error(`Request failed with ${res.status}`)
        return { status: 'success', data: await res.json()}
      } catch (err) {
        return { status: 'error', error: err as Error}
      }
    }

    const result = await handleFetch()

    if (result.status === 'success') {
      console.log(result.data)
    } else if (result.error) {
      console.error(result.error)
    }
    ```
4. [type narrowing](./types.md#что-такое-narrowing-в-typescript)
5. Never для "невозможных состояний"
    ```ts
    function assertNever(x: never): never {
      throw new Error('Unexpected case');
    }

    switch (status) {
      case 'ok':
      case 'error':
        break;
      default:
        assertNever(status);
    }
    ```
- [Error boundaries (React уровень)](../React/ru/components.md#что-такое-предохранители-error-boundaries)

# Как и для чего используется ключевое слово keyof?

Ключевое слово ``keyof`` — это оператор, который берет тип объекта и создает из его ключей Union-тип (объединение строк или чисел).

```ts
interface User {
  id: string
  name: string
  email: string
}

type UserKeys = keyof User
```

Примеры использования в реальных кейсах:
1. Создание своих UtilityTypes
    ```ts
    type Stringify<T> = {
      [Key in keyof T]: string
    }

    type StringifiedUser = Stringify<User>
    ```
2. безопасная работа с объектами
    ```ts
    function getObjProperty<T, K extends keyof T>(obj: T, name: K) {
      return obj[name]
    }
    const person = { name: 'Xaden', age: 23 }
    getObjProperty(person, 'name') // Ok
    getObjProperty(person, 'position') // Error, так как такого ключа нет
    ```