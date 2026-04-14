# Hooks

**Hooks** - функции появившиеся в React 16.8, которые позволяют использовать состояние и другие возможности React без написания классов.

## Правила хуков

- должны вызываться только на верхнем уровне
- нельзя вызывать внутри циклов, условий или вложенных функций
- нельзя использовать в классовых компонентах

# Преимущества хуков

- Переиспользуемость логики за счет кастомных хуков
- Лаконичный код (компоненты с хуками обычно занимают меньше строк, чем аналогичные классовые компоненты)
- Лучшая читаемость (логика, находится в одном месте (useEffect), а не размазана по componentDidMount и componentWillUnmount)
- Отказ от this: Избавляет от путаницы с привязкой контекста this в JavaScript. 

# Недостатки хуков:

- **Строгие правила (Rules of Hooks)**
- **Сложность с замыканиями (Closures)**: Эффекты и колбэки "захватывают" старые значения (state/props), что часто приводит к устаревшим данным, если не указать правильные зависимости.
- **Сложные массивы зависимостей** (управление зависимостями в useEffect, useCallback, useMemo может быть утомительным и вызывать бесконечные циклы рендера при ошибках)
- **Трудности при переходе с классов**

# Разница между useEffect() и componentDidMount()?

**componentDidMount()**
- используется в классовых компонентах
- вызывается 1 раз после mounting

**useEffect(callback, [dependencies])**
- используется в функциональных компонентах
- вызывается после mounting и после каждого update (``componentDidMount`` + ``componentDidUpdate``)
- чтобы работал как ``componentDidMount()`` необходимо передать пустой dependency array

# Что такое useReducer()?

**useReducer()** - хук, который возвращает массив с двумя элементами.

- Первый элемент массива - состояние (store либо state)
- второй элемент - функция изменения состояния (dispatch)

```jsx
const [store, dispatch] = useReducer(...);
```

## Когда использовать useReducer

- ecть несколько useState => можно заменить их единственным useReducer.
- если одно состояние зависит от другого, это с большой вероятностью работа для useReducer. Все зависимости одного состояния от другого лучше описывать в редукторе (reducer)
```jsx
const [state1, setState1] = useState();
const [state2, setState2] = useState();

// В данном случае лучше использовать useReducer
useEffect(() => {
  if (state1 === any) setState2();
}, []);

import { useReducer } from 'react';

function reducer(state, action) {
  if (action.type === 'incremented_age') {
    return {
      age: state.age + 1,
    };
  }
  if (action.type === 'decremented_age') {
    return {
      age: (state.age - 1) || 0,
    };
  }
  throw Error('Unknown action.');
}

function MyComponent() {
  const [state, dispatch] = useReducer(reducer, {
    age: 42,
  });

  function handleIncrementAge() {
    dispatch({ type: 'incremented_age' });
    // ...
  }
  function handleIDecrementAge() {
    dispatch({ type: 'decremented_age' });
    // ...
  }

  return (
    <>
      <span>Age: {state.age}</span>
      <button onClick={handleIncrementAge}>+</button>
      <button onClick={handleIDecrementAge}>-</button>
    </>
  )
}
```

# Расскажите о хуках useCallback(), useMemo(), useImperativeHandle(), useLayoutEffect()?

**useCallback()** - мемоизирует функцию, чтобы её ссылка не менялась между рендерами.

```jsx
const handleClick = useCallback(() => {
  doSomething(value);
}, [value]);
```

Зачем:
- предотвращает лишние ререндеры дочерних компонентов
- стабилизирует зависимости в useEffect

Когда нужен:
- передаёшь callback в memo-компоненты
- функция в dependency array

Важно:
- сам по себе не ускоряет, может даже замедлить (из-за overhead)
- нужен только если есть referential equality проблема

**useMemo()** - мемоизирует результат вычисления (значение)

```jsx
const sortedList = useMemo(() => heavySort(data), [data]);
```

Зачем:
- избегает дорогих вычислений на каждом рендере

Когда нужен:
- expensive computations (фильтрация, сортировка)
- стабилизация объектов/массивов

**useLayoutEffect()** - синхронный side эффект, который выполняется после DOM-изменений, но ДО отрисовки браузером

```jsx
useLayoutEffect(() => {
  measureDOM();
});
```

Зачем:
- измерение layout (width, height)
- синхронные DOM-изменения без "мигания"

**useImperativeHandle()** - это хук в React, который позволяет управлять тем, какие методы и свойства будут доступны родителю при использовании ref на дочернем компоненте.

Он используется в паре с ``forwardRef``, чтобы настроить внешний интерфейс доступа к внутренним функциям/значениям компонента.

```jsx
useImperativeHandle(ref, createHandle, dependencies?)

useImperativeHandle(ref, () => ({
  // методы, которые будут доступны извне
  someMethod() {
    // ...
  },
}), [dependencies]);
```

Когда нужен:
- ограничить доступ к внутренним функциям компонента (инкапсуляция).
- экспонировать методы дочернего компонента родителю (например, focus, reset, scrollToTop и др.).

Важно:
- useImperativeHandle работает только внутри компонентов, обёрнутых в forwardRef.

Пример: доступ к методу focus из родителя
```jsx
import React, { useRef, useImperativeHandle, forwardRef } from 'react';

const CustomInput = forwardRef((props, ref) => {
  const inputRef = useRef<HTMLInputElement>(null);

  useImperativeHandle(ref, () => ({
    focus: () => {
      inputRef.current?.focus();
    },
    clear: () => {
      inputRef.current!.value = '';
    }
  }));

  return <input ref={inputRef} {...props} />;
});

export default function App() {
  const inputRef = useRef<{ focus: () => void; clear: () => void }>(null);

  return (
    <div>
      <CustomInput ref={inputRef} />
      <button onClick={() => inputRef.current?.focus()}>Фокус</button>
      <button onClick={() => inputRef.current?.clear()}>Очистить</button>
    </div>
  );
}
```

# Какие хуки были добавлены в React Router версии 5?

- useHistory(): Возвращает объект history, используемый для навигации (переход на другие страницы, push, replace, goBack).
- useLocation(): Возвращает текущий объект location, похож на useState, обновляется при смене URL.
- useParams(): Возвращает объект с парами ключ/значение из динамических параметров URL (например, :id в /user/:id).
- useRouteMatch(): Позволяет проверить, соответствует ли текущий URL маршруту, и получить данные match без рендеринга <Route>. 

# Разница между memo и useMemo?

**React.memo** — это компонент высшего порядка (HOC) для кэширования компонента целиком, чтобы избежать перерендера при неизменных props.

- Оборачивает компонент целиком
    ```jsx
    const MyComponent = React.memo(() => { ... })
    ```
- Перерисовывает компонент только если изменились props.
- Используется для оптимизации производительности функциональных компонентов, предотвращая лишние рендеры.

**useMemo** — это React-хук для кэширования результата вычислений внутри компонента, чтобы не пересчитывать сложные данные при каждом рендере. 

- Используется внутри функционального компонента
    ```jsx
    const memoizedValue = useMemo(() => compute(a, b), [a, b])
    ```
- Запоминает результат вычислений (значение) между перерисовками.
- Пересчитывает значение только тогда, когда меняются зависимости в dependency array ([a, b])
