# Что такое Map?

Map - это коллекция пар «ключ-значение», похожая на объект, но позволяющая использовать ключи любого типа (объекты, функции, числа)

# Основные методы Map

## clear()

- очищает колекцю [Читать дальше](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map/clear)

## delete(key)

- удаляет свойство по ключу key, возвращает true если свойство удалено успешно и false если свойство не было найдено [Читать дальше](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map/delete)

## entries()

- возвращает иттератор с парами ключ значение вида ``[key, value]`` [Читать дальше](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map/entries)

```js
const map = new Map();

map.set("0", "foo");
map.set(1, "bar");

const iterator = map.entries();

console.log(iterator.next().value);
// Expected output: Array ["0", "foo"]
```

## forEach(callback(key, value, map))

- выполняет функцию callback для каждого элемента Map [Читать дальше](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map/forEach)

## get(key)

- возвращает значение по заданному key [Читать дальше](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map/get)

## has(key)

- возвращает true если свойство есть в Map [Читать дальше](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map/has)

## keys()

- возвращает итератор содержащий все keys [Читать дальше](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map/keys)

## set(key, value)

- записывает свойство в Map по ключу [Читать дальше](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map/set)

## values()

- возвращает итератор содержащий все values [Читать дальше](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map/keys)

# Разница между Object и Map?

Map и Object в JavaScript служат для хранения пар ключ-значение, но Map лучше подходит для частых обновлений, поддерживает ключи любого типа (не только строки/символы) и гарантирует порядок перебора. 

Object — это базовая структура с прототипом, тогда как Map — специализированная коллекция с удобными методами size, set, get. 

Основные отличия Map от Object:
1. Типы ключей: 
    - В Map ключами могут быть что угодно (объекты, функции, примитивы)
    - в Object — только строки или символы (Symbol).
2. Порядок элементов: 
    - Map перебирает данные в порядке их добавления
    - Object не гарантирует строгий порядок.
3. Размер: 
    - Количество элементов в Map легко узнать через свойство .size,
    - для Object нужно перебирать ключи вручную.
4. Итерация
    - Map — перебираемый объект (можно использовать for...of), 
    - по Object нужно итерироваться через Object.keys() или for...in.
5. Производительность:
    - Map оптимизирован для частого добавления и удаления пар ключ-значение.

Когда использовать:

Map:
- нужна динамическая коллекция
- ключи не строковые
- важен порядок
- часто добавляются/удаляются элементы.

Object: 
- статичная структура с фиксированным набором полей
- JSON-сериализация
- использование методов прототипа

# Что такое WeakMap?

WeakMap - это коллекция пар «ключ-значение» в JavaScript, где ключами могут быть только объекты (или незарегистрированные символы), а значения — любые данные. Главная особенность — «слабые» ссылки на ключи, позволяющие сборщику мусора автоматически удалять пару, если объект-ключ больше нигде не используется, предотвращая утечки памяти (т.е. если объект-ключ удаляется из основного кода, он удаляется и из WeakMap)

Доступные методы: Только [set](#setkey-value), [get](#getkey), [delete](#deletekey), [has](#haskey)

# Разница между Map и WeakMap

WeakMap
- ключи в WeakMap **должны быть объектами**, а не примитивными значениями
- если мы используем объект в качестве ключа и если больше нет ссылок на этот объект, то он будет удалён из памяти (и из объекта WeakMap) автоматически.
    ```js
    let john = { name: "John" };

    let weakMap = new WeakMap();
    weakMap.set(john, "...");

    john = null; // перезаписываем ссылку на объект
    // объект john удалён из памяти!
    ```

    В Map ключ-объект будет существовать до тех пор, пока существует Map. Он занимает место в памяти и не может быть удалён сборщиком мусора.

    ```js
    let john = { name: "John" };

    let map = new Map();
    map.set(john, "...");

    john = null; // перезаписываем ссылку на объект
    // объект john сохранён внутри объекта `Map`,
    // он доступен через map.keys()
    ```
- WeakMap не поддерживает перебор и методы keys(), values(), entries(), так что нет способа взять все ключи или значения из неё.