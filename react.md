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

HOC 不应该修改传入组件，而应该使用组合的方式，通过将组件包装在容器组件中实现功能

几点约定和注意：

- 透传与自身无关的props

- 最大化可组合性

- 包装显示名称方便调试

- 不要在render中使用HOC，会使diff不同，影响性能，子组件状态丢失

- 返回的组件中没有被包装组件的静态方法，需要手动复制出来

- Refs 不会被传递，会指向容器组件本身

### JSX

- jsx会被编译成React.createElement()语法
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

### react api

 React.Component基类中包含了生命周期。state与props

组件实例被创建并插入 DOM 中时，其生命周期调用顺序如下：

- constructor()  ： 初始化内部state，避免将porps的值赋值给state
- render()： 纯函数，返回boolean 或null时不渲染
- componentDidMount()： 网络请求数据，添加订阅

触发更新后，生命周期为

+ shouldComponentUpdate
+ render
+ componentDidUpdate： 首次渲染不会执行

卸载时 

- componentWillUnmount ：清除 timer，取消网络请求或清除订阅

调用 `forceUpdate()` 将致使组件调用 `render()` 方法，此操作会跳过该组件的 `shouldComponentUpdate()`。但其子组件会触发正常的生命周期方法，避免使用该api

## webpack

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

## [context API](https://www.smashingmagazine.com/2020/01/introduction-react-context-api/#top)

创建：React.createContext(); </ThemeContext.Provider>包裹使用数据的组件

使用方法：

+ 函数式组件，使用useContext钩子
+ 类组件，使用该`Class.contextType`方法，限制只能使用一个上下文。
+ 类组件，使用 <Context.Consumer>包裹

