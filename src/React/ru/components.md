# Стадии жизненного цикла компонента в React?

1. Initialization (создание Virtual DOM-узла)
2. Mounting (встраивание в реальный DOM)
3. Updating (обновление):
    - при изменении ``props``
    - при изменении состояния (useState, useReducer, setState)
    - ``forceUpdate`` (только в классовых компонентах)
    - изменении контекста на который подписан компонент
4. Unmounting (уничтожение DOM и Virtual DOM узлов)

# Методы жизненного цикла компонента в React?

## initialization

constructor(props) - конструктор, в котором происходит начальная инициализация компонента

## mounting

- static getDerivedStateFromProps(props, state)
    не имеет доступа к текущему объекту компонента (то есть обратиться к объкту компоненту через this) 
    возвращает объект для обновления объекта state или значение null, если нечего обновлять.
- render() - рендеринг компонента
- componentDidMount() - после того, как компонент встроился в DOM

## update

- static getDerivedStateFromProps(props, state) 
- shouldComponentUpdate(nextProps, nextState) - 
    вызывается каждый раз при обновлении объекта props или state
    возвращает boolean который говорит будет ли перерисовка или нет
- getSnapshotBeforeUpdate(prevProps, prevState)
    вызывается перед обновлением компонента
    позволяет компоненту получить информацию из DOM перед возможным обновлением
- componentDidUpdate(prevProps, prevState, snapshot)
    вызывается после обновления компонента (если shouldComponentUpdate возвращает true).
- componentDidCatch(errorString, errorInfo)
    особый метод, т.к. он позволяет реагировать на любые неперехваченные ошибки в любом из дочерних компонентов.

## unmounting

- componentWillUnmount() - вызывается перед удалением элемента из DOM

# PureComponent

**React.PureComponent** это базовый класс для классовых компонентов в React, оптимизирующий производительность за счет автоматического пропуска re-renders, если новые props и state поверхностно равны старым. В отличие от обычного Component, он автоматически реализует метод shouldComponentUpdate() с проверкой изменений.

## Классовый React.PureComponent

```jsx
import { PureComponent, useState } from 'react';

class Greeting extends PureComponent {
  render() {
    return (
      <h3>
        Hello{this.props.name && ', '}
        {this.props.name}!
      </h3>
    );
  }
}

export default function MyApp() {
  const [name, setName] = useState('');
  const [address, setAddress] = useState('');

  return (
    <>
      <label>
        Name{': '}
        <input
          value={name}
          onChange={(e) =>
            setName(e.target.value)
          }
        />
      </label>
      <Greeting name={name} />
    </>
  );
}
```

## Функциональный аналог

```jsx
import { memo, useState } from 'react';

// Обернуть еомпонент в memo
const Greeting = memo(function Greeting({ name }) {
  return (
    <h3>
      Hello{name && ', '}
      {name}!
    </h3>
  );
});

export default function MyApp() {
  const [name, setName] = useState('');
  const [address, setAddress] = useState('');
  return (
    <>
      <label>
        Name{': '}
        <input
          value={name}
          onChange={(e) =>
            setName(e.target.value)
          }
        />
      </label>
      <Greeting name={name} />
    </>
  );
}
```

# Компонент высшего порядка (Higher-Order Component/HOC)

**HOC** - это функция, которая принимает компонент и возвращает новый компонент.

```jsx
// Эта функция принимает компонент...
function withSubscription(WrappedComponent, selectData) {
  // ...и возвращает другой компонент...
  return class extends React.Component {
    constructor(props) {
      super(props);
      this.handleChange = this.handleChange.bind(this);
      this.state = {
        data: selectData(DataSource, props)
      };
    }

    componentDidMount() {
      // ...который подписывается на оповещения...
      DataSource.addChangeListener(this.handleChange);
    }

    componentWillUnmount() {
      DataSource.removeChangeListener(this.handleChange);
    }

    handleChange() {
      this.setState({
        data: selectData(DataSource, this.props)
      });
    }

    render() {
      return <WrappedComponent data={this.state.data} {...this.props} />;
    }
  };
}
```

# Разница между классовым и функциональным компонентами?

- **Синтаксис**: 
```jsx
// Функциональные
function Component (props) {
  return <div>Hello {props.name}!</div>
}
// Классовые
class ComponentClass extends React.component {
  constructor (props) {
    this.name = props.name
  }
  render () {
    return <div>Hello {this.name}!</div>
  }
}
```
- **Состояние** (State)
    Функциональные используют хук useState,
    Классовые — this.state
- **Жизненный цикл**
    Функциональные используют хук useEffect,
    Классовые — методы (например, componentDidMount, componentDidUpdate, componentWillUnmount).
- **this** - не используется в функциональных компонентах.
- Производительность и размер - функциональные компоненты обычно легче, лаконичнее и проще тестируются. 

# Разница между управляемыми (controlled) и не управляемыми (uncontrolled) компонентами?

**Управляемые компоненты** - cостояние хранится в React (в state), а данные обновляются через обработчики событий (например, ``onChange``).

**Неуправляемые компоненты** - Состояние хранится в самом DOM (внутри элемента), а React обращается к нему только при необходимости с помощью ``ref``.

# Разница между элементом и компонентом?

**React-элемент** — это легкий, неизменяемый объект, описывающий, что показать на экране (виртуальный DOM),
- это простое описание (объект). 
- нельзя изменить после создания.
- Синтаксис
    ```jsx
    const el = <Welcome name="Dmitry" />
    ```

**Компонент** — это функция или класс, который принимает props и возвращает элементы.
- это функция/класс, создающая это описание.
- может управлять своим состоянием (state) и жизненным циклом.
- Синтаксис
    ```jsx
    function Welcome(props) { ... }
    ```

# Разница между компонентом и контейнером?

## Презентационные (UI-компоненты, "глупые" компоненты)

- отвечают за внешний вид (HTML/CSS)
- обычно не имеет состояния, или только UI-состояние.
- получает данные через props.
- минимальная логика, только для отображения.

## Контейнеры («умные» компоненты). 

- управляют логикой
- управляет state или подключается к Redux/Context
- загружает данные (API-запросы) и обновляет их.
- часто являются родителями для презентационных компонентов.

С появлением ``hooks`` разделение стало менее жестким. «Умную» логику теперь можно добавить в любой функциональный компонент, что снизило необходимость в явном паттерне «контейнер-презентация»

# Разница между состоянием(state) и пропсами(props)?

## props

- передаются компоненту извне (родительским компонентом)
- являются неизменяемыми (readonly) внутри компонента

## состояние (state)

- управляется внутри компонента
- является изменяемым и используется для динамического обновления данных

# Как React обрабатывает, или ограничивает использование пропсов определенного типа?

- PropTypes
- Typescript
- React.ReactNode
- ReactElement

## Типизация пропсов через PropTypes

- PropTypes.any
- PropTypes.bool
- PropTypes.number
- PropTypes.string
- PropTypes.func
- PropTypes.array
- PropTypes.object
- PropTypes.symbol
- PropTypes.node (что-то, что можно отрендерить)
- PropTypes.element (React элемент)
- PropTypes.instanceOf(Class) (экземпляр определенного класса)
- PropTypes.oneOf(['Option1', 'Option2']) (одно из указанных значений)
- PropTypes.oneOfType([PropTypes.string, PropTypes.number]) (один из указанных типов)
- PropTypes.arrayOf(PropTypes.number) (массив элементов определенного типа)
- PropTypes.objectOf(PropTypes.number) (объект со значениями определенного типа)
- PropTypes.shape({ name: PropTypes.string, age: PropTypes.number }) (объект с заданной структурой)
- PropTypes.exact({ name: PropTypes.string, age: PropTypes.number }) (объект с точно заданной структурой (дополнительные свойства запрещены))

## Типизация children

- React.ReactNode
- ReactElement (только JSX (React) элементы и компоненты) - это более ограниченный тип по сравнению с React.ReactNode.

# Что такое «бурение пропсов» (Prop Drilling)? Как его избежать?

**Prop Drilling (Бурение пропсов)** — это ситуация в React, когда данные (props) передаются от родительского компонента к глубоко вложенному дочернему через ряд промежуточных компонентов, которым эти данные не нужны. Это усложняет поддержку кода и делает компоненты слишком зависимыми друг от друга. 

Как избежать Prop Drilling:
- [Context API](./base.md#что-такое-контекст-context)
- **Композиция компонентов** (Children) можно передать сам компонент вместо передачи пропсов вниз
- **Управление состоянием (State Management)**: Использование библиотек вроде Redux, MobX или Zustand позволяет хранить данные в глобальном хранилище и получать доступ к ним из любого компонента.
- **Хуки (Hooks)**: Создание собственных хуков для получения данных там, где они нужны. 
