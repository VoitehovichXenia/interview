# Функция и из чего она состоит

**Функция** - именованный, автономный блок кода, обладающий локальной областью видимости, предназначенный для выполнения конкретной задачи, который можно многократно вызывать в разных частях программы

У функции есть:
- имя (optional)
- параметры
- возвращаемое значение (по умолчанию undefined)

Все аргументы хранятся в псевдомассиве ``arguments``

Функция объявляется с помощью:
- ключевого слова **function**
- конструктора **new Function(params, code)**
- **arrow function** () => {}

Функции в JS являются объектами первого класса. Что такле объект первого класса - [читать здесь](../data_structures_and_types/object.md#объект-первого-класса)

# Разница между function declaration и function expression?

- **function declaration** - объявление в потоке кода
    ```ts
    function func (params) {}
    ```
    Создается до выполнения всего кода

- **function expression** - объявление в контексте выражения
    ```ts
    const func = function (params) {}
    ```
    Создается в момент выполнение кода

# Анонимные функции

**Анонимной** в JavaScript называют функцию с которой не связано никакое имя

```ts
function ask (question: string, yes: () => void, no: () => void) {
  if (confirm(question)) {
    yes()
  } else {
    no()
  }
}

ask(
  'Do you like cats?',
  function () { alert('Perfect ^_^')}, // анонимная функция
  function () { alert('How dare you!')}
)
```

### IIFE (immediately invoked function expression)

```ts
(function () {
  console.log('I was invoked');
})()
```

# Callback функции

**Callback функция** (функция обратного вызова) - это функция переданная в другую функцию в качестве аргумента, которая затем вызывается по завершении какого-либо действия

```ts
function printName (name: string, callback: () => any) {
  console.log(`Name: ${name}`);
  callback()
}
```

# Стрелочные функции

Синтаксис: () => {}

Отличия от традиционных функций:
- Нет ``this``
- Нет доступа к псевдомассиву ``arguments``
- Нельзя вызвать с помощью операторв ``new``
```ts
function sayHi(name) { console.log(`Hello ${name}!`) }
new sayHi('Jane Doe')

const arrowFunc = (name) => { console.log(`Hello ${name}!`) }
new arrowFunc('John Doe') // Error
```
- Не допускают дублирования имен параметров (традиционные же фунции допускают такое в нестрогом режиме)

# Рекурсия

**Рекурсия** - вызов функцией самой себя

```ts
let i = 0;

function count () {
  if (i === 3) {
    console.log('I\'m tired')
    return
  }

  console.log(`Counting... ${i}`)
  i++
  count()
}
```

# Каррирование функций

**Каррирование** - процесс создания новой функции путем фиксирования аргументов существующей

```ts
function getMessage(message: string) {
  return function (name) {
    console.log(`Hello, ${name}, we have a message for you: ${message}`)
  }
}

const fireMessage = getMessage('THE BUILDING IS ON FIRE!!!')
fireMessage('Kate'); // 'Hello, Kate, we have a message for you: THE BUILDING IS ON FIRE!!!'
fireMessage('John'); // 'Hello, John, we have a message for you: THE BUILDING IS ON FIRE!!!'
```

# Чистая функция

**Чистая функция** - функция которая всегда при одних и тех же аргументах возвращает одинаковый результат, а также не изменяет ничего снаружи себя

```ts
function clean (a: number, b: number) {
  return a + b;
}

let a = 1;
function dirty (b: number) {
  // Меняет внешнюю переменную
  a = Math.random()

  // Непредсказуемый результат из-за использования Math.random()
  return a + b
}

// dirty
// Меняет внешний объект
function (obj) {
  obj.x = 5
}

// dirty
// Меняет окружение вне функции - выводит в консоль
function () {
  console.log(5)
}
```

# HOC (High order function)

**HOC** - функция, которая или принимает другие функции как параметер или возвращает функцию

```ts
function calculate (a: number, b: number, callback: (result: number) => any): number {
  const sum = a + b;

  callback(sum);

  return
}

function calculate (a: number, b: number): (operation: 'print' | 'distract', num?: number) => void {
  let res = a + b;

  return function (operation, num = 0) {
    switch (operation) {
      case 'print':
        console.log('SUM: ', res)
        break;
      case 'distract':
        res -= num
        return res;
    }
  } 
}
```

# Замыкание

**Замыкание** - это функция со всеми внешними переменными доступными ей

![Closure](../../../../assets/JS/closure.png)

Все переменные функции - это свойства внутреннего объекта **лексического окружения** (Lexical Environment), который создается при запуске фунцции. Является скрытым и недоступным для прямого доступа.

Из функции можно обратиться к глобальным переменным, но поиск всегда начинается с собственного лексического окружения, и только затем если ее нет, ищет ее во внешнем объекте.

Сcылка на объект внешнего лексического окружения хранится в специальном внутреннем свойстве функции ``[[scope]]``

```ts
const a = 5;

function foo () {
  const a = 8;
  console.log(a)
}

foo() // 8
```

# This

**this** - это ссылка на объект в контексте которого выполняется функция или метод (тогда this равен объекту в контексте которого был вызван)

У стрелочных функций нет this. В стрелочных функциях this ссылается на this внешней функции.

По умолчанию для функций this ссылается на window, но мы можем задать его явно:

- **.bind(context)** - указывает ссылку на объект в контексте которого будет осуществлен вызов, при это НЕ вызывая функцию
    ```ts
    const car = {
      name: 'Audi',
      printName () {
        console.log(this.name)
      }
    }

    const car_2 = {
      name: 'Tesla'
    }
    car_2.printName = car.printName.bind(car_2)

    car.printName()
    car_2.printName()
    ```
- **.call(context, arg1, arg2 ...)** - первый аргумент указывает ссылку на объект в контексте которого будет осуществлен вызов, следующие аргументы передаются в функцию непосредственно, при этом происходит ВЫЗОВ функции
    ```ts
    const car = {
      name: 'Audi',
      getInfo (age) {
        console.log(`Car info: ${this.name} ${age} years old`)
      }
    }

    const car_2 = {
      name: 'Tesla'
    }

    car.getInfo(25)
    car.getInfo.call(car_2, 5)
    ```
- **.apply(context, [arg1, arg2 ...])** - первый аргумент указывает ссылку на объект в контексте которого будет осуществлен вызов, следующие аргументы передаются в виде массива в функцию непосредственно, при этом происходит ВЫЗОВ функции
    ```ts
    const car = {
      name: 'Audi',
      getInfo (age, nextServiceDate) {
        console.log(`Car info: ${this.name} ${age} years old, next service should be done in: ${nextServiceDate}`)
      }
    }

    const car_2 = {
      name: 'Tesla'
    }

    car.getInfo(25, '2026')
    car.getInfo.apply(car_2, [5, '2028'])
    ```

## Polyfills для bind call apply

### bind()

Идея
- сохранить контест вызова метода (оригинальную функцию)
- вернуть новую функцию в которой мы:
    - определим новый итоговый контекст (если передан в myBind - то оставляем, если нет - window)
    - в итоговом контексте временно создадим метод, который будет оригинальной функцией
    - вызовем метод с переданными аргументами и сохраним результат в переменную
    - удалим созданный метод
    - вернем результат выполнения
```ts
Function.prototype.myBind = function (context) {
  const originalFunc = this

  return function binded(args) {
    const finalContext = context ?? window

    const tempMethodSymbol = Symbol()

    finalContext[tempMethodSymbol] = originalFunc

    const result = finalContext[tempMethodSymbol](args)

    delete finalContext[tempMethodSymbol]

    return result
  }
}
```

### call()

Идея: 
- сделать точно также, только сразу вызывать фунцию а не возвращать ее

```ts
Function.prototype.myCall = function (context, ...args) {
  const originalFunc = this

  const finalContext = context ?? window

  const tempMethodSymbol = Symbol()

  finalContext[tempMethodSymbol] = originalFunc

  const result = finalContext[tempMethodSymbol](...args)

  delete finalContext[tempMethodSymbol]

  return result
} 
```

### apply()

Идея: 
- сделать точно также, только сразу вызывать фунцию а не возвращать ее
- дополнительно проверить переданны ли аргументы

```ts
Function.prototype.myApply = function (context, args) {
  const originalFunc = this

  const finalContext = context ?? window

  const tempMethodSymbol = Symbol()

  finalContext[tempMethodSymbol] = originalFunc

  const result = finalContext[tempMethodSymbol]( args ? ...args : undefined)

  delete finalContext[tempMethodSymbol]

  return result
} 
```