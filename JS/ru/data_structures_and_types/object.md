# Объект

**Объект** - тип данных, представляющий собой неупорядоченную коллекцию свойств (properties), где каждое свойство это пара ключ - значение

Под капотом в зависимости от движка и оптимизации представлен разными структурами данных, но если обобщить то это хэш-таблица

Имена свойств оформляются по [правилам переменных](../js.md#переменные), однако зарезервированные ключевые слова использовать разрешено. При этом есть специальное свойство ``__proto__`` которое всегда должно быть объектом (ссылкой на объект) и его лучше не модифицировать

## Объект первого класса

**Объект первого класса (англ. first-class object, first-class entity, first-class citizen)** - элементы, которые могут быть переданы как параметр, возвращены из функции, присвоены переменной.

## Объекты в JS по происхождению

**Нативные объекты (Native Objects)**— это встроенные в язык JavaScript компоненты, описанные в спецификации ECMAScript (например, Object, Array, Date, Math).

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
- через hasOwnProperty (возврвщвет true только если свойство принадлежит самому объекту, а не его прототипу)
    ```ts
    const obj = { status: 'pending' }

    console.log(obj.hasOwnProperty('status')) // true
    console.log(obj.hasOwnProperty('toString')) // false
    ```