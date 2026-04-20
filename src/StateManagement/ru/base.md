# Что такое Flux?

Архитектура **Flux** - это паттерн управления состоянием, разработанный Facebook для создания масштабируемых и управляемых приложений на React. Он был создан в ответ на проблемы, возникающие при управлении состоянием в сложных приложениях.

Основные компоненты архитектуры Flux:

1. **Действия (Actions)**: объекты, которые описывают события или изменения, происходящие в приложении

```jsx
{ type: "ADD_TO_CART", payload: product }
```

2. **Диспетчер (Dispatcher)**: функция принимающая действия (actions) и передающая их зарегистрированным обработчикам (stores).

```jsx
dispatch({ type: "ADD_TO_CART", payload })
```

3. **Хранилища (Stores)**: содержат состояние приложения и логику его обновления.

```jsx
{
  user: {...},
  posts: [...],
  cart: [...]
}

function cartReducer(state, action) {
  switch (action.type) {
    case "ADD_TO_CART":
      return [...state, action.payload];
    default:
      return state;
  }
}
```

4. **Представления (Views)**:  компоненты React отображающие состояние приложения и реагирующие на его изменения.

5. **Единство данных (Unidirectional Data Flow)**: Flux использует однонаправленный поток данных, где изменения состояния могут происходить только путем действий, передаваемых через диспетчер и обрабатываемых хранилищами. Это упрощает отслеживание и отладку потоков данных в приложении.

![Flux architecture scheme](../../../assets/State%20management/flux.png)

# Что такое Redux? Ключевые принципы Redux?

**Redux** — это JS библиотека, представляющая предсказуемый контейнер состояния для JS приложений, который централизует управление данными в одном месте — Store.

Ключевые принципы Redux:
1. Единственный источник истины (Single Source of Truth) - Store
2. Состояние доступно только для чтения (State is Read-Only) - нельзя напрямую менять данные (например, state.user = 'New'), данные изменить только с помощью reducer
3. Изменения вносятся чистыми функциями (Changes are Made with Pure Functions): Чтобы указать, как дерево состояний преобразуется, пишутся reducers. Редьюсер принимает текущее состояние и action, а возвращает новый объект состояния (иммутабельность).
4. Однонаправленный поток данных (Unidirectional Data Flow): Данные двигаются только в одном направлении: Action -> Reducer -> Store -> View. 

Основные компоненты:
- Store
- Action: Объект с типом (type) и данными, описывающий намерение изменить данные.
- Reducer: Функция (state, action) => newState. 

# Плюсы и минусы Redux?

Плюсы
- состояние доступно в любом компоненте
- состояние предсказуемо, так как иммутабельно, изменить его можно только через reducers
- однонаправленный поток данных

Минусы:
- много шаблонного кода
- избыточность для небольших приложений
- увеличивает размер бандла

# Разница между Redux и Flux?

Redux
- один store
- данные store иммутабельны
- нет центрального dispatcher, вместо него логика реализуется в многих reducers

Flux
- может быть много store
- данные ммогут быть мутабельны
- изменения store происходят через центральный dispather
- вместо reducers используются callbacks

# Разница между React State и Redux State?

React State:
- локален для компонента
- обновляется напрямую через setState
- происходит перерендер компонента

Redux state
- является глобальным и доступен в любой части приложения без прокидывания props
- обновляется только по цепочке action => reducer
- контролирует перерендеры

# Что такое Reselect и как он работает?

**Reselect** - это библиотека для создания мемоизированных селекторных функций. Селекторы Reselect могут использоваться для эффективного вычисления производных данных из Redux store.

## Зачем нужен Reselect?

Допустим есть следующий state:
```jsx
{
  posts: [...],
  filter: "active"
}
```

Нужно получить из него посты:
```jsx
const visiblePosts = posts.filter(...)
```

Если делать это прямо в компоненте:
- пересчёт на каждый render
- новые ссылки => лишние ререндеры

## Что такое селектор в Reselect

Селектор — функция, которая берёт state и возвращает вычисленные данные
```jsx
const selectPosts = state => state.posts;
```

## Как работает Reselect

- запоминает прошлые входные значения
- если они НЕ изменились  => возвращает кеш
- если изменились => пересчитывает

```jsx
import { createSelector } from 'reselect';

const selectPosts = state => state.posts;
const selectFilter = state => state.filter;

const selectVisiblePosts = createSelector(
  [selectPosts, selectFilter],
  (posts, filter) => {
    console.log("recomputing...");
    return posts.filter(p => p.status === filter);
  }
);

selectVisiblePosts(state); // считает
selectVisiblePosts(state); // НЕ считает (кеш)
```

Связь с React
```jsx
// если селектор возвращает тот же reference => компонент НЕ ререндерится
const posts = useSelector(selectVisiblePosts);
```

## Reselect vs useMemo

| Критерий | Reselect | useMemo |
| -------- | -------- | ------- |
| Где используется | вне компонента | внутри |
| Scope	| глобальный | локальный |
| Повторное использование	| да | нет |
| Подходит для Redux | да | нет |

# Как решить что выбрать Context API или State manager?

Выбор между Context API и State manager зависит от частоты обновлений данных и сложности приложения. 

- Context API подходит для редких обновлений (темы, локаль, пользователь) и малых приложений.
- State-менеджеры лучше для частых обновлений, сложной логики и большой вложенности, так как оптимизируют рендер.

# Что такое Redux Thunk/Saga и для чего он нужен?

Redux Thunk и Redux Saga — это вспомогательные библиотеки (middleware) для Redux, которые нужны для управления side effects. По умолчанию Redux умеет обрабатывать только **синхронное** обновление данных, а эти инструменты позволяют внедрить логику для **асинхронных** операций.

## Redux Thunk

Подключение:
```js
import { createStore, applyMiddleware } from 'redux';
import thunk from 'redux-thunk';

const store = createStore(rootReducer, applyMiddleware(thunk));
```

Использование:
```js
// Создаем функцию ation creator
export const fetchUsers = async (dispatch) => {
  dispatch({ type: 'FETCH_USERS_REQ' })

  try {
    const res = await fetch('https://jsonplaceholder.typicode.com/users')
    const data = await res.json()
    dispatch({ type: 'FETCH_USERS_SUCCESS', payload: data })
  } catch (err) {
    dispatch({ type: 'FETCH_USERS_ERROR', payload: err.message })
  }
}
```

## Redux Saga

Основан на работе с [функциями генераторами](../../JS/ru/base/functions.md#что-такое-функции-генераторы-и-для-чего-они-используются)

Использование:
```js
import { call, put, takeEvery } from 'redux-saga/effects';

function* fetchUsersSaga() {
  try {
    const res = yield call(() => {
      return fetch('https://jsonplaceholder.typicode.com/users')
        .then(res => res.json())
    })
    yield put({ type: 'FETCH_USERS_SUCCESS', payload: res })
  } catch (e) {
    yield put({ type: 'FETCH_USERS_ERROR', message: e.message })
  }
}

// Saga watcher
function* watchFetchUsers() {
  yield takeEvery('USERS_FETCH_REQUESTED', fetchUsersSaga)
}

// Компонент просто делает dispatch({ type: 'USERS_FETCH_REQUESTED' }). Сага "видит" это событие, останавливает выполнение на yield call до получения ответа и затем продолжает работу
```

# Разница между MobX и Redux?

Redux
- обновляет store только через цепочку action => reducer

MobX

```jsx
import { makeAutoObservable } from "mobx";
import { observer } from "mobx-react-lite";
// 1. Создаем стор
class CounterStore {
  count = 0; // State

  constructor() {
    makeAutoObservable(this); // Делает всё магически реактивным
  }

  // Action
  increment() {
    this.count++;
  }
}

const myCounter = new CounterStore();

// 2. Оборачиваем компонент в observer
const CounterView = observer(() => (
  <div>
    <p>Счет: {myCounter.count}</p>
    <button onClick={() => myCounter.increment()}>+1</button>
  </div>
));
```

# Какие библиотеки для управления состоянием вы знаете, кроме Redux?

1. Zustand
2. MobX
3. TanStack Query (бывший React Query) / RTK Query
