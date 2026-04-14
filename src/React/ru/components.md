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

- ``constructor(props)`` - конструктор, в котором происходит начальная инициализация компонента

## mounting

- static ``getDerivedStateFromProps(props, state)``
    не имеет доступа к текущему объекту компонента (то есть обратиться к объкту компоненту через this) 
    возвращает объект для обновления объекта state или значение null, если нечего обновлять.
- ``render()`` - рендеринг компонента
- ``componentDidMount()`` - после того, как компонент встроился в DOM

### Как реализовать однократное выполнение операции при начальном рендеринге?

- ``componentDidMount()`` для классовых коспонентов
- ``useEffect(() => { /* side effect */ }, [])`` с пустым сассивом зависимостей для функциональных коспонентов

### Какие типы данных может возвращать render?

- JSX-элементы (компилируется в React.createElement())
- Массивы 
    ```jsx
    render () {
      return (
        [<li key="1">1</li>, <li key="2">2</li>]
      )
    }
    ```
- Фрагменты: ``<React.Fragment>...</React.Fragment>`` или ``<>...</>``
- Строки и числа (React отобразит их как текстовые узлы внутри DOM-элементов)
- Boolean или null / undefined: Используются для условного рендеринга. Если рендер возвращает null, false или undefined, React ничего не отрисует, но жизненный цикл компонента продолжит работать.
- [Порталы (Portals)](#что-такое-портал-portal)

## update

- static ``getDerivedStateFromProps(props, state) ``
- ``shouldComponentUpdate(nextProps, nextState)``
    вызывается каждый раз при обновлении объекта props или state
    возвращает boolean который говорит будет ли перерисовка или нет
    true (по умолчанию) - React выполняет перерендер компонента.
- ``getSnapshotBeforeUpdate(prevProps, prevState)``
    вызывается перед обновлением компонента
    позволяет компоненту получить информацию из DOM перед возможным обновлением
- ``componentDidUpdate(prevProps, prevState, snapshot)``
    вызывается после обновления компонента (если shouldComponentUpdate возвращает true).
- ``componentDidCatch(errorString, errorInfo)``
    особый метод, т.к. он позволяет реагировать на любые неперехваченные ошибки в любом из дочерних компонентов.

## unmounting

- ``componentWillUnmount()`` - вызывается перед удалением элемента из DOM

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

# Что такое инверсия наследования (Inheritance Inversion)?

**Инверсия наследования (Inheritance Inversion, или II)** в React — это паттерн компонентов высшего порядка (HOC), при котором HOC-функция возвращает класс, расширяющий (наследующий) обернутый компонент (WrappedComponent). В отличие от обычного наследования, здесь HOC управляет компонентом, получая доступ к this.state, this.props и методу render(). 

HOC может читать, модифицировать состояние, вызывать методы жизненного цикла или переопределять render() WrappedComponent.

```jsx
function IIHOC(WrappedComponent) {
  return class extends WrappedComponent {
    render() {
      const original = super.render();

      return (
        <div style={{ border: '2px solid red' }}>
          {original}
        </div>
      );
    }
  };
}
```

Сравнение с обычным HOC (Props Proxy):
- HOC просто передаёт пропсы
- НЕ лезет внутрь компонента

```jsx
function withLogger(WrappedComponent) {
  return function(props) {
    console.log(props);
    return <WrappedComponent {...props} />;
  }
}
```

Реальный пример:

```jsx
// Есть некий компонент кнопки
class Button extends React.Component {
  state = { count: 0 };

  handleClick = () => {
    this.setState({ count: this.state.count + 1 });
  };

  render() {
    return (
      <button onClick={this.handleClick}>
        Clicked: {this.state.count}
      </button>
    );
  }
}

// Inheritance inversion
function withDebug(WrappedComponent) {
  return class extends WrappedComponent {
    componentDidMount() {
      console.log('State:', this.state);
    }

    render() {
      const original = super.render();

      return (
        <div>
          <h3>Debug mode</h3>
          {original}
        </div>
      );
    }
  };
}

const DebugButton = withDebug(Button);
```

# Что такое портал (Portal)?

**React Portal** - это механизм, позволяющий рендерить дочерние компоненты в DOM-узел, находящийся вне иерархии родительского компонента

```jsx
// Классовые компоненты
ReactDOM.createPortal(child, container).

// Функциональные компоненты
import { createPortal } from 'react-dom';
// ...
<div>
  <p>This child is placed in the parent div.</p>
  {createPortal(
    <p>This child is placed in the document body.</p>,
    document.body
  )}
</div>
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

# Разница между createElement() и cloneElement()?

``React.createElement()`` и ``React.cloneElement()`` создают React-элементы, но с разными целями: первый создает элемент «с нуля» (аналог JSX), а второй — клонирует существующий элемент, позволяя переопределить props и детей.

```jsx
const element = React.createElement('h1', {className: 'title'}, 'Hello, world!');

const clonedElement = React.cloneElement(element, {className: 'subtitle'});
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

# Что такое предохранители (Error Boundaries)?

**Предохранители (Error Boundaries)** — это компоненты React, которые отлавливают ошибки JavaScript в любом месте деревьев их дочерних компонентов, сохраняют их в журнале ошибок и выводят запасной UI вместо рухнувшего дерева компонентов.

Классовый компонент является предохранителем, если он включает хотя бы один из следующих методов жизненного цикла static ``getDerivedStateFromError()`` или ``componentDidCatch()``.

```jsx
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }

  static getDerivedStateFromError(error) {
    // Update state so the next render will show the fallback UI.
    return { hasError: true };
  }

  componentDidCatch(error, errorInfo) {
    // You can also log the error to an error reporting service
    logErrorToMyService(error, errorInfo);
  }

  render() {
    if (this.state.hasError) {
      // You can render any custom fallback UI
      return <h1>Something went wrong.</h1>;
    }

    return this.props.children; 
  }
}
```

# Разница между состоянием(state) и пропсами(props)?

## props

- передаются компоненту извне (родительским компонентом)
- являются неизменяемыми (readonly) внутри компонента

## состояние (state)

- управляется внутри компонента
- является изменяемым и используется для динамического обновления данных
- задается this.setState() для классовых компонентов useState() для функциональных
    ```jsx
    class Example extends React.Component {
      constructor(props) {
        super(props);
        this.state = { counter: 3 };
      }

      tick() {
        this.setState({ counter: 4 });
      }
    }
    ```

### Зачем в setState() нужно передавать функцию?

для гарантированного получения актуального предыдущего состояния при асинхронных или множественных обновлениях подряд

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

# Что такое поднятие состояния вверх (Lifting State Up)?

**Поднятие состояния вверх (Lifting State Up)** - это принцип в React, при котором состояние, необходимое нескольким компонентам, перемещается в их ближайшего общего предка. Вместо локального хранения данных в дочерних компонентах, родитель управляет состоянием и передает его вниз через props, обеспечивая синхронизацию. 

# Что такое сhildren? Как работает пропс children в React?

``children`` в React — это специальное свойство, которое автоматически содержит всё содержимое, вложенное между открывающим и закрывающим тегами компонента. 

- Если внутри нет ничего, ``props.children`` будет ``undefined``.
- Если элементов несколько, ``props.children`` будет массивом.
- Для манипуляции с детьми (например, изменения или фильтрации) можно использовать ``React.Children.map`` или ``React.Children.forEach``

# Что такое распределенный компонент?

В контексте React термин «распределенный компонент» используется для определения React Server Components (Серверных компонентов)

React Server Components - это современный подход (React 18+), при котором компоненты рендерятся на сервере, а не в браузере клиента, и их результат (UI) отправляется на клиент без отправки их JavaScript-кода.

Ограничения Server Components
- нет state
- нет effects
- нельзя использовать browser APIs
- нельзя подписки (events)
- async только на сервере

Таким образом:

Server Component 
- загружает данные 
- выполняет манипуляции с данными (фильтрация на сервере)
- передает готовый результат + props => client component

Client component:
- оживляет компонент добавляя state, events и др.

Пример - лента постов:
```jsx
// Server component
export default async function PostFeed({ searchParams }) {
  const posts = await fetch("https://api.com/posts").then(r => r.json());

  const filtered = searchParams.q
    ? posts.filter(p => p.title.includes(searchParams.q))
    : posts;

  return (
    <div>
      <h1>Posts</h1>

      {/* pass the data to the client component */}
      <PostListClient posts={filtered} />
    </div>
  );
}

// Client component:
"use client";

import { useState, useEffect } from "react";

export default function PostListClient({ posts }) {
  const [likes, setLikes] = useState({});
  const [query, setQuery] = useState("");

  // browser API example
  useEffect(() => {
    localStorage.setItem("likes", JSON.stringify(likes));
  }, [likes]);

  const handleLike = (id) => {
    setLikes(prev => ({
      ...prev,
      [id]: !prev[id],
    }));
  };

  const filtered = posts.filter(p =>
    p.title.toLowerCase().includes(query.toLowerCase())
  );

  return (
    <div>
      <input
        value={query}
        onChange={(e) => setQuery(e.target.value)}
        placeholder="Search..."
      />

      <ul>
        {filtered.map(post => (
          <li key={post.id}>
            {post.title}

            <button onClick={() => handleLike(post.id)}>
              {likes[post.id] ? "❤️" : "🤍"}
            </button>
          </li>
        ))}
      </ul>
    </div>
  );
}
```

# Как передавать пропсы в React Router?

В React Router 5 для передачи дополнительных пропсов можно использовать render:

```jsx
<Route path="/example" render={(props) => <Component {...props} customProp="value" />} />
```

Для React Router 6 есть несколько способов:

1. Прямая передача через element
```jsx
<Routes>
  <Route 
    path="/profile" 
    element={<Profile name="Иван" age={25} />} 
  />
</Routes>
```
2. Передача пропсов при переходе (``useNavigate``)
Вы можете передать данные при навигации через state, которые затем читаются через хук useLocation.
```jsx
// Отправка
const navigate = useNavigate();
navigate('/profile', { state: { name: "Иван" } });

// Получение (в компоненте Profile)
const location = useLocation();
const { name } = location.state;
```
3. Использование URL-параметров (``useParams``)
Для динамических данных, таких как ID, используйте параметры маршрута.
```jsx
// Маршрут
<Route path="/user/:id" element={<User />} />

// Компонент
const { id } = useParams(); // URL: /user/123 -> id = "123"
```
