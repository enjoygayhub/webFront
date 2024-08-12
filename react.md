# react

### state

+ 不能直接修改，需要使用setState（）

+ 因为 `this.props` 和 `this.state` 可能会异步更新，不要依赖他们的值来更新下一个状态。可以让 `setState()` 接收一个函数而不是一个对象。这个函数用上一个 state 作为第一个参数，将此次更新被应用时的 props 做为第二个参数

  ```jsx
  this.setState((prestates,preprops) => ({state: prestates+preprops}));
  ```

+ state更新会被合并，可以单独更新

+ `setState()` 并不总是立即更新组件。它会批量推迟更新。在调用 `setState()` 后立即读取 `this.state` 成为了隐患。为了消除隐患，使用 `componentDidUpdate` 或者 `setState` 的回调函数（`setState(updater, callback)`），这两种方式都可以保证在应用更新后触发。

### 事件处理

  函数式事件回调中this指向问题，使用箭头函数来解决。可以在定义时使用箭头函数+赋值          callback= ()=>{}，或者在传参时使用（）=>{this.callback}

###  阻止渲染

隐藏组件，可以让 `render` 方法直接返回 `null`，而不进行任何渲染。<strong>不会影响组件的生命周期</strong>

### key

列表中应当添加唯一的key字段，否则在react diff的过程中性能降低。

好的经验法则是：在 `map()` 方法中的元素需要设置 key 属性。

当使用数组的下标作位key时，组件进行重新排序时，会导致key的变动，会出现无法预期的变动。

### 表单

input textarea 可以通过value={this.state.value} onChange={this.handleChange}实现双向绑定

select 中类似，可以通过<select multiple={true} value={['B', 'C']}>，传入数组

### Error Boundary

错误边界是一种 React 组件，这种组件**可以捕获发生在其子组件树任何位置的 JavaScript 错误，并打印这些错误，同时展示降级 UI**，而并不会渲染那些发生崩溃的子组件树。错误边界在渲染期间、生命周期方法和整个组件树的构造函数中捕获错误。

错误边界**无法**捕获以下场景中产生的错误：

- 事件处理 （例如onclick）
- 异步代码（例如 `setTimeout` 或 `requestAnimationFrame` 回调函数）
- 服务端渲染
- 它自身抛出来的错误（并非它的子组件）

### Refs

避免使用 refs 来做任何可以通过声明式实现来完成的事情。使用情况：

- 管理焦点，文本选择或媒体播放。

- 触发强制动画。

- 集成第三方 DOM 库。

tips：

- 当 `ref` 属性用于 HTML 元素时，构造函数中使用 `React.createRef()` 创建的 `ref` 接收底层 DOM 元素作为其 `current` 属性。
- 当 `ref` 属性用于自定义 class 组件时，`ref` 对象接收组件的挂载实例作为其 `current` 属性。
- **不能在函数组件上使用 `ref` 属性**，因为他们没有实例。可以在函数组件内部使用useRef。
- 回调形式使用Ref，ref = {callback}，在回调函数中设置引用
- 可使用React.forwardRef，将ref引用传递给子组件。

### Fragment

Fragments 允许你将子列表分组，而无需向 DOM 添加额外节点。简写<> </> 来声明 Fragments。

key是唯一支持的属性。

### HOC高阶组件

**高阶组件是参数为组件，返回值为新组件的函数。**

1.状态管理:使用Redux。

2.渲染增强:使用 HOC 来添加特定的渲染逻辑，如添加额外的 UI 组件或修改现有组件的输出。
3.性能优化:使用 HOC 来实现如懒加载、记忆化等性能优化技术。
4权限控制:使用 HOC 来控制组件的可见性，只允许特定的用户访问某些组件。
5日志记录和调试:使用 HOC 来记录组件的生命周期事件或添加调试信息。

HOC 不应该修改传入组件，而应该使用组合的方式，通过将组件包装在容器组件中实现功能

几点约定和注意：

- 透传与自身无关的props

- 最大化可组合性，增加props或减少

- 包装显示名称方便调试

- 不要在render中使用HOC，会使diff不同，影响性能，子组件状态丢失

- 返回的组件中没有被包装组件的静态方法，需要手动复制出来

- Refs 不会被传递，会指向容器组件本身

## 非受控组件使用

例如倒计时组件，初始状态由父组件通过props传递，显示的内容只与自身状态有关。此时这个倒计时组件为不完全受控，父组件传递给子组件的prop变更后，子组件的状态并不会改变。

解决方案：

+ 将状态抽离子组件，完全由父组件控制，但这违背了初始意愿，我们想让子组件来负责自己的计时逻辑。

+ 子组件使用key，当父组件改变传递的初始计时值时，给子组件一个新的key，得到新组件，更改计时。

+ 使用getDerivedStateFromProps，注意该周期函数与componentWillRecieveProps不同，该周期函数每次触发更新都会调用，而不仅仅时props改变时调用。

  ```jsx
  static getDerivedStateFromProps(props, state) {
      //子组件state中新增flag作位替换count的标志,对比props中的flag，不相同时则取新的count继续倒计时
      if (props.flag !== state.flag) {
        return {
          flag: props.flag, // 设置新的标识
          count: props.count
        };
      }
      return null;
    }
  ```

+ 使用 memoization，仅在输入变化时，重新计算 `render` 需要使用的值

  ```jsx
  import memoize from "memoize-one";
  
  class Example extends Component {
    // state 只需要保存当前的 filter 值：
    state = { filterText: "" };
  
    // 在 list 或者 filterText 变化时，重新运行 filter：
    filter = memoize(
      (list, filterText) => list.filter(item => item.text.includes(filterText))
    );
  
    handleChange = event => {
      this.setState({ filterText: event.target.value });
    };
  
    render() {
      // 计算最新的过滤后的 list。
      // 如果和上次 render 参数一样，`memoize-one` 会重复使用上一次的值。
      const filteredList = this.filter(this.props.list, this.state.filterText);
  
      return (
        <Fragment>
          <input onChange={this.handleChange} value={this.state.filterText} />
          <ul>{filteredList.map(item => <li key={item.id}>{item.text}</li>)}</ul>
        </Fragment>
      );
    }
  }
  ```


### JSX
jsx是一种语法扩展，类html的模板语言，需要babel转义为js语法
- jsx会被编译成React.createElement()语法，返回React Element的js对象

```js
//createElement函数入参，type标时节点类型，config对象表示属性键值对，children对象记录组件标签之间的嵌套内容
export function createElement(type, config, children){}

```
- 一个模块中导出许多组件时，可使用点语法获得组件
- 自定义组件必须以大写字母开头
- 大括号{}中可以嵌入JavaScript表达式，if 和for 不是表达式，所以必须在jsx以外使用
- props 默认值为true
- render中可以return 一个数组
- 可以把回调函数作为 `props.children` 进行传递，来重复生产组件
- `false`, `null`, `undefined`, and `true` 是合法的子元素。但并不会被渲染

```jsx
shouldComponentUpdate(nextProps, nextState){}
//手动判断是否更新dom
```

### portals
Portal 提供了一种将子节点渲染到存在于父组件以外的 DOM 节点的优秀的方案。

当父组件有 `overflow:hidden` 或 `z-index` 样式时，但你需要子组件能够在视觉上“跳出”其容器。例如，对话框、悬浮卡以及提示框。

可以通过Portal 进行事件冒泡，捕获兄弟节点的事件

```jsx
ReactDOM.createPortal(child, container)
```

### render prop

render prop 是一个用于告知组件需要渲染什么内容的函数 prop。

可以将相同的行为封装复用，渲染不同的内容。

### react API

 React.Component基类中包含了生命周期。state与props

组件实例被创建并插入 DOM 中时，其生命周期调用顺序如下：

- constructor()  ： 初始化内部state，避免将props的值赋值给state
- render()： 纯函数，返回boolean 或null时不渲染
```jsx
// element需要渲染的元素，container挂在的目标容器真实的dom，
ReactDOM.render(elements,container,[callback])
```
- componentDidMount()： 网络请求数据，添加订阅

触发更新后，生命周期为

+ shouldComponentUpdate
+ render
+ componentDidUpdate： 首次渲染不会执行

卸载时 

- componentWillUnmount ：清除 timer，取消网络请求或清除订阅

调用 `forceUpdate()` 将致使组件调用 `render()` 方法，此操作会跳过该组件的 `shouldComponentUpdate()`。但其子组件会触发正常的生命周期方法，避免使用该api

## hooks

### react hooks 的原理是什么

React Hooks 是 React 16.8 引入的一个新特性，它允许在函数组件中使用状态和其他 React 功能，而无需编写类组件。它的实现原理是通过利用 JavaScript 的闭包和函数式编程的思想来实现。

每个 Hook 都是一个函数，它可以对组件的状态进行操作或者访问 React 的其他功能。当组件渲染时，React 会根据每个 Hook 调用的顺序来维护内部的状态并执行相应的操作。

使用 Hook 的过程中，React 维护了每个组件的“Hook 状态链”，它是一个单向链表结构，存储着所有使用 Hook 的状态信息。每次组件更新时，React 会检查使用的 Hook 是否发生变化，并根据变化来更新状态链中的对应状态。

### useState

```jsx
const [state, setState] = useState(initialState);
```

state：当前状态值

setState：更新state的函数

initialState：初始状态值

### useEffect

useEffect 是 React Hooks 中的一个非常重要的 Hook，用于处理副作用操作，如数据获取、订阅或手动更改 DOM。useEffect 允许你在函数组件中执行类似于类组件中的生命周期方法

副作用函数:
你提供一个函数作为第一个参数，这个函数将在渲染后执行。
你可以在这个函数中执行任何副作用操作，如数据获取、设置定时器、订阅等。
清理函数:
如果副作用函数返回一个函数，这个返回的函数将在组件卸载时执行，用于清理副作用。
这可以用于取消网络请求、清除定时器、取消订阅等。
依赖数组:
第二个参数是一个依赖数组，它告诉 React 哪些值的变化会导致副作用函数重新运行。
如果依赖数组为空（[]），副作用函数仅在组件挂载时运行一次，并在组件卸载时清理。

### useLayoutEffect 与 useEffect 的区别

useLayoutEffect 在浏览器进行布局和绘制之前同步执行，会阻塞浏览器绘制。
用于读取 DOM 布局信息（如元素尺寸），确保某些副作用在浏览器更新 UI 之前发生

### useReducer
useReducer 是 React Hooks 中的一个 Hook，用于在函数组件中管理复杂的状态逻辑。它类似于 Redux 的 reducer，但适用于函数组件。

reducer 函数:
你提供一个 reducer 函数，这个函数接收当前的状态和动作，并返回新的状态。
dispatch 函数:
你提供一个 dispatch 函数，这个函数用于分发动作，并触发状态更新。
initialState:
你提供一个初始状态值。
state:
你获取当前的状态值。
dispatch:
你获取一个用于分发动作的函数。



### useMemo
useMemo 接收一个函数和一个依赖数组作为参数。它会在每次渲染时调用该函数，并根据依赖数组中的值来判断是否需要重新计算函数的结果。

函数:
你提供一个函数，该函数返回一个需要缓存的值。
这个函数会在每次渲染时被调用，但返回值会被缓存。
依赖数组:
一个数组，包含函数依赖的值。
如果依赖数组中的值发生改变，useMemo 会重新运行函数并更新缓存的值。
如果依赖数组中的值没有改变，useMemo 将返回上次计算的结果，从而避免了不必要的计算。

### useCallback

useCallback 用于返回一个被优化过的函数引用，这个函数只有在依赖项发生变化时才会重新创建。
适用场景：
当你需要传递一个函数作为 prop 到子组件，并且这个函数的依赖项很少改变时。
当函数作为回调被频繁创建时，使用 useCallback 可以避免不必要的重新渲染。


### useContext

创建：React.createContext(); </ThemeContext.Provider>包裹使用数据的组件

使用方法：

+ 函数式组件，使用useContext钩子
+ 类组件，使用该`Class.contextType`方法，限制只能使用一个上下文。
+ 类组件，使用 <Context.Consumer>包裹

### 类组件和函数组的区别，什么场景下使用

- 类组件使用 ES6 的 class 关键字定义，需要继承 React.Component。内置的生命周期方法和状态管理功能。类组件可以通过 this.refName 访问 DOM 节点或子组件的引用。类组件可以通过继承扩展其他类组件，便于代码复用和重用。

- 函数组件使用普通的函数定义，函数的返回值是 JSX。通过 useState 和 useEffect 等 Hooks 来管理状态和副作用。
使用场景：复杂的状态管理、生命周期方法、直接访问 Refs，组件需要扩展其他类组件时使用类组件。
对于简单的 UI 组件，只需要少量的状态管理或副作用操作时使用函数组件。

### React 组件中绑定一个事件跟直接操作 DOM 绑定一个事件有什么差别

React 中，事件处理程序通常作为属性（props）附加到 JSX 元素上，通过 React 的合成事件系统自动完成，使用箭头函数来自动绑定 this，当组件卸载时，相关的事件监听器会被自动清理，避免内存泄漏。

React 使用合成事件来提供一致性的事件处理机制，同时通过事件代理的方式提高了性能。通过使用合成事件，React 简化了事件处理的复杂性，并自动管理了事件监听器的生命周期，使得开发者可以专注于业务逻辑的实现。

### hooks 使用的时候有注意些什么
React Hooks 的规则限制

React Hooks 虽然为函数组件带来了极大的灵活性，但同时也有一些使用规则，其中一条就是 不能在表达式、条件语句或循环中定义 Hooks。

原因分析
Hooks 执行顺序： Hooks 的执行顺序是固定的，并且依赖于它们在函数组件中的调用顺序。如果在表达式或条件语句中定义 Hooks，它们的执行顺序就会变得不确定，这可能会导致不可预期的行为和错误。
保证组件的稳定性: Hooks 的规则是为了保证组件的稳定性和可预测性。如果允许在表达式中定义 Hooks，那么组件的行为就会变得难以理解和调试。

1. 只能在顶层调用 Hooks:
不能在一个循环、条件语句或嵌套函数内部调用 Hooks。
2. Hooks 必须始终在组件的最外层调用，以确保每次渲染时 Hooks 被调用的顺序相同。
只在 React 函数中调用 Hooks:
3. Hooks 只能在 React 的函数组件或自定义 Hooks 中使用。
不要在类组件、普通的 JavaScript 函数或其他库中使用 Hooks。
4. 不要在自定义 Hooks 之外的地方返回 Hooks:
如果你创建了一个自定义 Hook，那么这个 Hook 应该只在其他 Hook 内部被调用，而不是在组件外部。
自定义 Hooks 应该遵循 use 的命名约定，比如 useCustomHook()。
5. 不要在条件语句中调用 Hooks:
不要在 if 语句内调用 Hooks，因为这会导致某些渲染中 Hooks 被跳过。
如果某些逻辑只有在满足特定条件时才执行，应该将这些逻辑放在单独的函数中，并在组件的最外层调用。
6. 不要在事件处理器中直接调用 Hooks:
事件处理器中直接调用 Hooks 会导致 Hooks 每次事件触发时重新创建，这可能导致不一致的状态或副作用行为。
如果需要在事件处理器中使用状态，应该在组件的顶级作用域中定义状态变量，并在事件处理器中引用这些变量。
7. 使用 useEffect 清理副作用:
当你的组件卸载时，或者在特定依赖项改变之前，必须清理副作用。
使用 useEffect 的清理函数来确保副作用正确清理，避免内存泄漏。
8. 遵守依赖数组的规则:
当你使用 useEffect 或 useCallback 时，依赖数组中的值必须是可控的。
如果依赖项可能变化，确保将其包含在依赖数组中，否则使用闭包来保持值的不变性。
9. 不要滥用 useState 和 useEffect:
尽管可以多次调用 useState 和 useEffect，但这并不意味着你应该过度使用它们。
保持组件简洁，合理组织状态和副作用，避免不必要的复杂性。
10. 避免使用闭包中的状态:
当你在事件处理器中使用状态时，确保不是通过闭包间接引用状态，而是直接引用状态变量。
闭包可能会导致状态更新不一致。
11. 使用最新的 Hook 实例:
如果你使用 useRef 或 useMemo 来缓存值，确保在这些值发生变化时能够获取到最新实例。


### React Diff 的算法
React 使用一种称为“深度优先搜索”的算法来遍历虚拟 DOM 树。在遍历过程中，React 会对节点进行比较，以确定是否需要更新。

React 会从根节点开始，递归地比较虚拟 DOM 树中的节点
结点类型:
如果两个元素的类型不同（例如 <div> 和 <span>），则会删除旧元素并创建新元素。
key 属性:
如果元素有 key 属性，React 会使用 key 来追踪元素的位置。
当移动带有 key 的元素时，React 会尝试重用这些元素，而不是创建新的元素。
属性和子元素:
对于相同的元素类型，React 会比较属性和子元素。
如果属性发生变化，React 会更新这些属性。
如果子元素发生变化，React 会递归地进行同样的比较。
文本节点:
对于文本节点，React 仅比较文本内容。
如果内容发生变化，React 会更新文本内容。

### 实现一个useState
```jsx
const useState = defaultValue => {
    const value = useRef(defaultValue);
    
    const setValue = newValue => {
        if (typeof newValue === 'function') {
            value.current = newValue(value.current);
        } else {
            value.current = value;
        }
    }
    
    //  触发组件的重新渲染
    dispatchAction();
    
    return [value, setValue];
}
```

### fiber

Fiber 架构的核心思想是将 React 的渲染过程分为一系列可中断的工作单元，每个工作单元称为一个 “Fiber”。Fiber 本质上是一个对象，它表示了一个虚拟的 DOM 节点，并包含了该节点的状态、属性以及其他相关信息。

主要组成部分
Fiber 节点:
每个 Fiber 节点代表了虚拟 DOM 树中的一个节点。
Fiber 节点包含有关该节点的详细信息，如类型、属性、状态等。
工作队列:
Fiber 架构使用一个工作队列来管理需要执行的工作。
每个更新或渲染任务都被放入队列中，然后根据优先级进行调度。
优先级调度:
React 使用优先级调度算法来决定何时执行队列中的任务。
任务可以根据其重要性被赋予不同的优先级。
增量渲染:
React 使用增量渲染技术来分批处理队列中的任务。
这样可以在每个时间片中只处理一部分任务，从而避免长时间的阻塞。

### hooks性能优化
1. 使用 useMemo 和 useCallback
3. 使用 useRef ，避免在每次渲染时创建新的对象
5. 使用 React.memo 和 shouldComponentUpdate
6. 使用 useEffect 控制副作用
7. 使用 useContext
8. 代码分割和懒加载 lazy(()=>import())
10. 使用 useDebounce，对值及事件处理函数进行防抖，避免状态频繁变动，优化渲染次数
11. 使用 useImmer

## [redux](https://www.redux.org.cn)

### 基本概念

state：数据对象；

action：简单的对象，用来描述state发生了什么变更。

reducer：reducer 只是一个接收 state 和 action，并返回新的 state 的函数

store：只能有一个。创建store就是把所有reducer给它。export const store = createStore(rootReducers，initstate)

`store.dispatch()`是组件发出action的唯一方法。

### 原则：

- 单一数据源 ：单一的state tree
- 只读State ： 只能触发action来改变
- 纯函数执行修改：可以通过combineReducers将各个小的reducer合并

展示组件用于描述结构，容器组件中使用connect将state映射到props

## router

react-router 路由拦截

<Prompt message={（location，action）=>{}} when= {}>

Prompt 可以获得到路由跳转event事件，location.pathname可得到将要跳转的路由

when得到true时则渲染Prompt组件，启用拦截

message 得到true则表示允许跳转，即允许跳转，得到false表示不允许，得到字符串表示提示信息，会唤起浏览器的confirm提示。


