## React
* Компоненты, свойства: Props, State
* Метод setState
* Особенности синтаксиса JSX
* Что такое keys в React и чем их важность?
* Презентационные компоненты и компоненты контейнеры
* Что такое refs?
* React router, компонент withRouter
* Компоненты высшего порядка
* Однонаправленный поток данных, работа с событиями в React
* Жизненный цикл компонента, в каких методах жизненного цикла стоит выполнять ajax запросы? В каких стоит "обновлять state на основе props" ?
Оптимизации производительности. 
* Hooks
* articles

#### Компоненты, свойства: Props, State.
TODO

#### Метод setState.

TODO

#### Особенности синтаксиса JSX.

TODO

#### Что такое keys в React и чем их важность?

TODO

#### Презентационные компоненты и компоненты контейнеры.

Изначально в мире React существовало условное разделение на компоненты содержащие логику, обращение к api, state и 
компоненты занимающиеся отрисовкой (презинтационные компоненты). Это было удобно для консолидации стейта в о дном месте, например для форм, а также для 
шеринга логики между разными презентационными компонентами (или view), например для логики оплаты которую могут использовать разные страницы, попапы или виджеты.
Также для удобства использования контенеров без создания дополнительных компонентов прослоек часто используются *HOC* или *render props*. После появления *hooks*
появился еще один способ шарить логику между компонентами, благодаря написанию кастомных хуков. 
```javascript
const MyComponent = () => {
  const [time] = useClock();
  const [x, y] = useMouseMove();
  // do something with those values
  return (
    <>
      <div>Current time: {time}</div>
      <div>Mouse coordinates: {x},{y}</div>
    </>
  );
}
```

#### Что такое refs?

TODO

#### React router, компонент withRouter

TODO

#### Компоненты высшего порядка.

TODO

#### Однонаправленный поток данных, работа с событиями в React.

TODO

### Жизненный цикл компонента, в каких методах жизненного цикла стоит выполнять ajax запросы? В каких стоит "обновлять state на основе props»?

В данный момент существует последовательность жизненных циклов React указанная на рисунке. 
![merge](/img/react_lifecycle1.PNG)  
[ссылка на диаграмму](https://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/)  

**ajax** запросы можно выполнять в методе *componentDidMount()* или в хуке *useEffect*.  

#### Оптимизация
  Для разного синтаксиса сущуствуют различные методы отптимизация производительности React приложений, направленные в основном для
уменьшения количества перерендеринга страниц. 
В классовых компонентах стоит упомянуть:
* **React.PureComponent** - реализует *shouldComponentUpdate()* c поверхностным сравнением props, state (shallow prop and state comparison). 
* **shouldComponentUpdate(nextProps, nextState)** - дает возможность сравнить this.props, this.state с nextProps, nextState и в случае если перерендер не нужен 
вернуть false. 
* **componentDidUpdate(prevProps, prevState, snapshot)** - метода дает возможность сранить prevProps, prevState c this.props, this.state и в случае необходимости сделать запрос по api или this.setState(). 

Для фнкциональных компонентов:  
* Второй аргумент в *useEffect*, следящий за переменными от которых зависит рендер (подробнее в разделе hooks).  
* Второй аргумент в *useMemo*, *useCallback* также следящий за изменениями в переменных от которых зависит рендер. 

Для мониторинга частей рендеринга удобно использовать `React DevTools -> Profiler -> Highlight updates when components render`.  

### Hooks


 **useState** - `const [state, setState] = useState(initialState)` возвращает значение с состоянием и функцию для его обновления. Функция setState используется для обновления состояния. Она принимает новое значение состояния и ставит в очередь повторный рендер компонента. `setState(newState)`  

  **useEffect** - `useEffect(didUpdate)` Функция, переданная в useEffect, будет запущена после того, как рендер будет зафиксирован на экране. Применяется для подписок, таймеров, логирования, обращения к DOM или API (для api лучше использовать мотоды в библиотрках mobX, redux thunk и т.д.). 
  Второй аргумент useEffect отвечает за условия обновления,
  ```
  useEffect(() => { 
    return () => {
      subscription.unsubscribe();
    }; 
  }, [props.source] )
  ```
  Сработает только при изменении *source*, если передать пустой массив [ ] сработает один раз.
  Возвращенная функция (clean-up function) вызывается перед извлечением компонента из UI. 

  **useContext** - `const value = useContext(MyContext)` принимает объект контекста (значение, возвращённое из React.createContext) и возвращает текущее значение контекста для этого контекста. Текущее значение контекста определяется пропом value ближайшего <MyContext.Provider>

  **useRef** - `const refContainer = useRef(initialValue)` возвращает изменяемый ref-объект. Дает доступ к DOM. Если вы передадите React объект рефа с помощью подобного выражения `<div ref={myRef}/>`, React установит собственное свойство .current на соответствующий DOM-узел при каждом его изменении. Получить доступ к объекту - `myRef.current`

  **useCallback** - Возвращает мемоизированный колбэк.
  ```javascript
  const memoizedCallback = useCallback(
    () => {
        doSomething(a, b);
    },
    [a, b],
  )
  ```
  изменяется только, если изменяются значения одной из зависимостей.

  **useMemo** - `const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b])` Аналогично useCallback но возвращает значение вычислений а не саму функцию.

  **useReducer** - `const [state, dispatch] = useReducer(reducer, initialArg, init)` Принимает редюсер типа (state, action) => newState и возвращает текущее состояние в паре с методом dispatch. Работает по принципу редакса.
  
  
  ### Articles 
  
[How to Reuse Logic with React Hooks](https://rafaelquintanilha.com/how-to-reuse-logic-with-react-hooks/)
