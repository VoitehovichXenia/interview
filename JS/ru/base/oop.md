# Что такое ООП и ФП? Чем ООП отличается от ФП?

**ООП (объектно ориентированное программирование)** - это методология программирования, в которой программа строится как совокупность объектов, взаимодействующих друг с другом

Противоположность является **функциональное или процедурное программирование (ФП)** - парадигма программирования построенная на использовании чистых функций, иммутабельности и отсутствия скрытого состояния

# Принципы ООП

1. **Инкапсуляция**
    процесс сокрытия части реализации интерфейса от пользователя
2. **Наследование**
    возможность наследовать свойства и методы от других классов
3. **Полиморфизм**
    многообразие форм объекта передаваемого в метод, или возможность одинаковой интерпретации объектов разных классов
4. **Абстракция**
    выделение значимых характеристик объекта и игнорирование незначительных деталей.

# Принцип SOLID

S - **single responisibility** (1 класс ответственен за 1 сущность)
O - **open-closed** (класс должен быть открыт к расширению, но закрыт для модификации)
L - **liskov substitution** (если объект X является дочерним от Y, то при замене всех Y на X не должно произойти ничего критического)
I - **interface segregation** (каждый класс должен делать только то что нужно от него)
D - **dependency invertion** (модули верхнего уровня не должны зависеть от модулей нижнего уровня)

# Принцип DRY и KISS

**DRY** - Don't repeat yourself
**KISS** - Keep it simple stupid

# Класс

**Класс** - это модель, описывающая абстрактный тип данных и его частичную или полную реальзацию

**Метакласс** - класс экземпляры которого сами являются классами

**Суперкласс** - наивысший класс, от которого происходит наследование

Нет прямого способа удалить объект класса, для этого ему присваивается null

# Стили ООП в JS (условные)

## Функциональный стиль

### Публичные, приватные и защищенные свойства (Инкапсуляция)

**Публичные свойства** - доступные снаружи объекта, свойства и методы записанные в this

**Приватные свойства** - доступные только внутри объекта для его методов, это локальные переменные, параметры конструктора, вложенные функции

```ts
function CoffeMachine (power) { // аргумент power - приватное свойство
  this.waterAmount = 0; // свойство waterAmount - публичное свойство
  const WATER_HEAT_CAPACITY = 4200; // переменная WATER_HEAT_CAPACITY - приватное свойство

  function getBoilTime () { // функция getBoilTime - приватное свойство
    return this.waterAmount * WATER_HEAT_CAPACITY * 80 / power
  }

  this.run = function () { // метод run - публичное свойство
    setTimeout(onReady, getBoilTime.call(this))
  }

  function onReady () { // функция onReady - приватное свойство
    console.log('Coffee is ready')
  }
} 
```

**Защищенные свойства** - технически публичное свойство, предназначенное для внутреннего использования в самом объекте а также в дочерних классах, обычно имя такого свойства начинается с "_"

### Геттеры и сеттеры

Это функции используемые для управляемого доступа к состоянию объекта

```ts
function CoffeMachine (power) { // аргумент power - приватное свойство
  // ...
  // setter
  this.setWaterAmount = function (amount) {
    if (amount < 0) {
      throw new Error('Water amount should be a positive number')
    }
    if (amount > capacity) {
      throw new Error(`Water amount shouldn\'t be greater than ${capacity}`)
    }

    waterAmount = amount
  }

  // getter
  this.getWaterAmount = function () {
    return waterAmount
  }
} 
```

### Наследование

```ts
// Родительский класс
function Machine(power) {
  this.power = power
  let enabled = false

  this.enable = function() {
    enabled = true
  }
  this.disable = function() {
    enabled = false
  }
}
// Дочерний класс
function CoffeMachine(power) {
  Machine.apply (this, arguments)

  let waterAmount = 0
  this.setWaterAmount = function (amount) {
    if (amount < 0) {
      throw new Error('Water amount should be a positive number')
    }
    if (amount > capacity) {
      throw new Error(`Water amount shouldn\'t be greater than ${capacity}`)
    }

    waterAmount = amount
  }
}
```

Наследник не имеет доступа к приватным свойствам родителя, для того чтобы он имел доступ необходимо перенести свойство в защищенное (перенести его в this)

Переопределение методов родителя

```ts
function CoffeMachine(power) {
  Machine.apply (this, arguments)

  const parentEnable = this.enable;
  this.enable = function () {
    parentEnable.call(this)
    this.run()
  }
}
```

### Chaining

Это вызов нескольких методов применяемых к одному и тому же объекту вызываемых по цепочке

Реализация:

```ts
function Cat() {
  this.requestFeeding = function () {
    console.log('Please feed me!')
    return this
  }
  this.requestStroking = function () {
    console.log('Please stroke me!')
    return this
  }
}

const kot = new Cat()

kot.requestFeeding().requestStroking()
```

## Прототипный стиль

**Прототипный стиль** - объявление классов через псевдоклассы (состоит из функции конструктора и методов записанных в прототип)

Ключевые понятия:
- **Прототип** - это объект от которого наследуются свойства и методы
- ``__proto__`` - это свойство объекта наследника хранящее в себе ссылку на объект прототип
- ``prototype`` - это свойство функции конструктора класса, хранящее в себе ссылку на объект прототип

Если в объекте есть специальная ссылка ``__proto__``, то при чтении свойства объекта, если оно отсутсвует в данном объекте, то оно ищется в объекте на который ссылается ``__proto__``

Методы работы с ``__proto__``:
- Object.getPrototypeOf(obj) - возвращает ``__proto__``
- Object.setPrototypeOf(obj) - задает ``__proto__``
- Object.prototype = Object.create(/\*прототип\*/) - задает прототип для функции конструктора

По умолчанию прототип любого объекта - **Object**

У каждой функции по умолчанию уже есть свой ``prototype`` такого вида:
```ts
function Animal () {}

Animal.prototype = {
  constructor: Animal
}
```

При задавании ``prototype`` вручную можно потерять ``constructor``, поэтому его тоже необходимо задавать

```ts
function Rabbit () {}

Rabbit.prototype = Object.create(Animal)
Rabbit.prototype.constructor = Rabbit
```

### Для чего используется ключевое слово new?

Оно выделяет память для объекта в куче (``heap``), вызывает **функцию конструктор** для инициализации его данных и возвращает ссылку на созданный объект

### Объектная обертка (Wrapper Object)

**Объектная обертка (Wrapper Object)** — это специальный тип объекта, который служит оболочкой для примитивного типа данных (строка, число, boolean, symbol, bigint). 

В JS все сущности наследуются от Object.

Примитивы не являются объектами, но методы берут из соответствующих прототипов (``Number.prototype``, ``String.prototype``, ``Boolean.prototype``)

Если обратиться к свойству или методу примитива, то под капотом будет создан объект соответствующего прототипа, далее будет проведена операция со своиством или методом и далее объект будет удален


### Прототипное Наследование

- напрямую через запись в ``__proto__``
```ts
Rabbit.prototype.__proto__ = Animal.prototype
```
- через ``Object.create()``
```ts
Rabbit.prototype = Object.create(Animal)
```

Обязательно восстановить конструктор
```ts
Rabbit.prototype.constructor = Rabbit
```

### Расширение метода родителя

```ts
Rabbit.prototype.run = function () {
  Animal.prototype.apply(this, arguments)
  this.jump()
}
```

Таким образом можно расширить нативные JS объекты (``Array``, ``Function``, ``Object`` ...)

### Почему расширение нативных JavaScript-объектов это плохая практика?

Потому что:
- может вызвать конфликты с будущим JavaScript
- может вызвать конфликты между библиотеками
- В for..in цикле могут появлятся эти новые свойства и вызывать неожиданное поведение
- Глобальные сайд-эффекты: модификации работают везде: в собственном коде, в чужом коде, в сторонних библиотеках
- Сложнее дебажить
- ломает оптимизации движка

Когда допустимо - **полифилы**

Альтернатива
- Утилиты
    ```ts
    function sum(arr) {
      return arr.reduce((a, b) => a + b, 0)
    }
    ```
- Обёртки
    ```ts
    class MyArray extends Array {
      sum() {}
    }
    ```

# Как создать объект без прототипа? Object.create()

**Object.create()** - метод создающий объект с указанным прототипом

```ts
const obj = Object.create(null)
// У объекта не будет доступа к методам Object
```

# Как проверить принадлежит ли свойство самому объекту или прототипу? Разница hasOwnProperty и instanceOf и in

## Оператор in 

Проверяет есть ли свойство в объекте и цепочке его прототипов. Возвращает true, даже если свойство не находится в объекте непосредственно

```ts
const obj = { test: 'test' }

console.log('test' in obj) // true
console.log('toString' in obj) // true
```

## Метод hasOwnProperty(name)

Проверить принадлежит ли свойство самому объекту или прототипу можно с помощью ``hasOwnProperty(name)``. Возвращает true только если свойство принадлежит самому объекту

```ts
const animal = { test: 'test' }

animal.hasOwnProperty('test') // true
animal.hasOwnProperty('toString') // false
```

## Оператор instanceOf

``instanceOf`` позволяет проверить к какому классу принадлежит объект

Пример ``obj instanceof Array``
Проверяет по цепочке прототипов:
1. получает ``obj.__proto__``
2. сравнивает ``obj.__proto__`` с ``Array.prototype``
3. если не совпадает, тогда заменяет ``obj`` на ``obj.__proto__`` и повторяет шаг 2 до тех пор пока не найдется совпадение (true) или пока цепочка прототипов не закончится (false)

```ts
// Для всех примитивов выдает false
6 instanceof Number // false
```
