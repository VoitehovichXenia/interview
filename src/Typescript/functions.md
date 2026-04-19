# Что такое декораторы?

Декоратор — это функция, которая позволяет добавить или изменить поведение класса, метода, свойства или параметра.

```ts
// Декоратор класса
function CreatedAt(constructor: Function) {
  constructor.prototype.createdAt = new Date().toLocaleString()
}

interface UserService {
  createdAt: string
}

@CreatedAt
class UserService {
  constructor() {
    console.log(`User service is created`)
  }
}

const userService = new UserService()
console.log(userService.createdAt)
```

```ts
// Декоратор метода
// target - прототип
// key - название метода
// descriptor - объект описывающий поведение метода
// function LogMethod(target: any, key: string, descriptor: PropertyDescriptor) {

// в TS 5+ изменились аргументы
// originalMethod - оригинальная функция
// context - объект который содержит информацию о методе (name, static, private)
function LogMethod(originalMethod: any, context: ClassMethodDecoratorContext) {
  return function(this: object, ...args: any[]) {
    console.log(`Calling ${String(context.name)} method with ${args.length ? args.toString() : 'undefined'} arguments`)
    const result = originalMethod.apply(this, args)
    console.log(`Result is: ${result}`)
    return result;
  }
}

class Calculator {
  @LogMethod
  sum(a: number, b: number): number {
    return a + b
  }
}

const calculator = new Calculator();
calculator.sum(5, 10); 
```

```ts
// Декоратор свойства
function Readonly<T, V>(_value: undefined, context: ClassFieldDecoratorContext<T, V>) {
  context.addInitializer(function (this: any) {
    const { name } = context;
    const initialValue = this[name]

    Object.defineProperty(this, name, {
      value: initialValue,
      writable: false
    })
  })
}

class User {
  @Readonly
  name: string = "Alice";
}

const user = new User()
conaole.log(user.name)
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

# Как TypeScript поддерживает необязательные и дефолтные параметры в функции?

**Необязательные параметры**
- обозначаются знаком ``?``
- тип параметра автоматически становится ``T | undefined``

**Дефолтный параметер**
- дефолтное значение задается знаком ``=``

```ts
// age - необязательный параметер
function printPerson(name: string, age?: number) {}
// filter - default параметер
function getFilteredOperations(filter: 'All' | 'Successfull' | 'Failed' = 'All') {}
```