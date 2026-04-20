# Какие элементы ООП поддерживаются в TypeScript ?

  - Абстракция(с помощью абстрактных классов)
  - Инкапсуляция(с помощью private, public, protected)
  - Наследование(с помощью extends )
  - Полиморфизм(переопределение методов)
    ```ts
    interface Shape {
        getArea(): number;
    }

    // 2. Реализация полиморфизма через классы
    class Circle implements Shape {
        constructor(private radius: number) {}
        
        // Переопределение (реализация) метода
        getArea(): number {
            return Math.PI * this.radius ** 2;
        }
    }

    class Rectangle implements Shape {
        constructor(private width: number, private height: number) {}
        
        // Переопределение (реализация) метода
        getArea(): number {
            return this.width * this.height;
        }
    }
    ```

# Модификаторы доступа в TypeScript?

**private** - доступен только внутри класса

**public** - доступен внутри и вне класса

**protected** - доступен только классу и производным от него

```ts
class Person {
  private _name: string;
  private _age: number;
  
  constructor(name: string, age: number) {
    this._name = name;
    this._age = age;
  }

  protected printPerson() {
    console.log(`${this._name}, ${this._age} years old`)
  }
}

class Employee extends Person {
  protected company: string;
  
  constructor(name: string, age: number, company: string) {
    super(name, age)
    this.company = company;
  }

  public printEmployee() {
    // console.log(this._name) // ошибка так как свойство приватное
    this.printPerson()
    console.log(`Company: ${this.company}`)
  }
}

const developer = new Employee('Xaden', 23, 'team lead')

// console.log(developer._name) // Ошибка так как свойство приватное
// console.log(developer.company) // Ошибка так как свойство защищенное
developer.printEmployee()
```

# Что такое геттеры и сеттеры в TypeScript?

Это методы класса помогающие получать/задавать значения приватным свойствам. 

Особенности:
- если для свойства определен только сеттер, то оно считается недоступным для чтения
- если для свойства задан только геттер, то оно считается недоступным для изменения
- сеттеры не имеют возвращаемого значения

```ts
class User {
  private _name: string;

  constructor(name: string) {
    this._name = name;
  }

  get name() { return this._name }
  set name(name: string) { 
    if (name.length < 2) {
      console.log('Name should have at least 2 letters')
      return
    }
    this._name = name;
  }
}
```

# Разница между абстрактным классом (abstract class) и интерфейсом (interface)?

Абстрактный класс 
- описывает не только структуру, но и реализацию
- может задавать private, public и protected свойства
- класс может наследовать только 1 абстрактный класс

Интерфейс
- описывает только структуру
- все описанное является публичным
- класс может реализовывать несколько интерфейсов

```ts
// Интерфейс: описывает только контракт
interface Drivable {
  drive(): void;
}

// Абстрактный класс: контракт + реализация
// не можем создать напрямую объект абстрактного класса
abstract class Vehicle implements Drivable {
  private 
  constructor(public name: string) {} // Есть конструктор
  
  abstract drive(): void; // Абстрактный метод

  stop() { // Реализованный метод
    console.log(`${this.name} stopped`);
  }
}

class Car extends Vehicle {...}
```
