# react

## hooks
### react hooks 的原理是什么

通过 Fiber 节点上的单向 Hook 链表 + 闭包 + 双阶段 Dispatcher，按固定调用顺序将状态与副作用 “挂” 在组件上，实现函数组件的状态持久化与生命周期能力。

每个函数组件对应一个 Fiber 节点，其 memoizedState 指向一条 单向 Hook 链表，存储所有 Hook 的状态。
// 单个 Hook 对象（简化）
const hook = {
  memoizedState: null,  // 存储状态（useState值、useEffect回调等）
  queue: null,          // 更新队列（setState的更新任务）
  next: null            // 指向下一个 Hook，形成链表
}
首次渲染（mount）：按调用顺序创建 Hook 节点，串联成链表。

1. 只能在顶层调用 Hooks:  
2. 不要在条件循环语句中调用 Hooks: 
3. 不要在事件处理器中直接调用 Hooks:

### React Diff 的算法

对虚拟 DOM 树做分层同层比较 + 基于 key 的元素复用 + 只做最小必要更新，

1只做同层比较
2类型不同：直接重建
3类型相同：对比属性和文本
4子列表：用 key 匹配复用节点

### fiber

把原来同步、一次性、阻塞式的虚拟 DOM 渲染，拆分成可中断、可恢复、可调度的 “增量更新任务”，让浏览器主线程不被长时间阻塞，从而保证交互流畅。

Fiber 本质是一个链表结构的fiber树，替代了原来的递归虚拟 DOM：每个 Fiber 包含：
stateNode：对应 DOM 或组件实例
child：第一个子节点
sibling：兄弟节点
return：父节点
effectTag：更新标记（插入、更新、删除等）
expirationTime：优先级

### hooks性能优化
 1. useMemo：缓存计算结果（避免重复计算）
2. useCallback：缓存函数引用（避免子组件不必要重渲染）
3. React.memo：浅比较 props，缓存组件
4. useRef：保存不变引用，不触发重渲染
5. useTransition/useDeferredValue 实现调度优化


### useEffect

useEffect 是 React Hooks 中的一个非常重要的 Hook，用于处理副作用操作，如数据获取、订阅或手动更改 DOM。
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
useMemo 接收一个函数和一个依赖数组作为参数。

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

使用方法：使用useContext


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


## class react

### state

+ 不能直接修改，需要使用setState（）

+ 因为 `this.props` 和 `this.state` 可能会异步更新，不要依赖他们的值来更新下一个状态。可以让 `setState()` 接收一个函数而不是一个对象。这个函数用上一个 state 作为第一个参数，将此次更新被应用时的 props 做为第二个参数

  ```jsx
  this.setState((prestates,preprops) => ({state: prestates+preprops}));
  ```

+ state更新会被合并，可以单独更新

+ `setState()` 并不总是立即更新组件。它会批量推迟更新。在调用 `setState()` 后立即读取 `this.state` 成为了隐患。为了消除隐患，使用 `componentDidUpdate` 或者 `setState` 的回调函数（`setState(updater, callback)`），这两种方式都可以保证在应用更新后触发。

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


### portals
Portal 提供了一种将子节点渲染到存在于父组件以外的 DOM 节点的优秀的方案。

当父组件有 `overflow:hidden` 或 `z-index` 样式时，但你需要子组件能够在视觉上“跳出”其容器。例如，对话框、悬浮卡以及提示框。

可以通过Portal 进行事件冒泡，捕获兄弟节点的事件

```jsx
ReactDOM.createPortal(child, container)
```


### react 生命周期

 React.Component基类中包含了

组件实例被创建并插入 DOM 中时，其生命周期调用顺序如下：

- constructor()  ： 初始化内部state，避免将props的值赋值给state
- render()： 纯函数，返回boolean 或null时不渲染
- componentDidMount()： 网络请求数据，添加订阅

触发更新后，生命周期为

+ shouldComponentUpdate
+ render
+ componentDidUpdate： 首次渲染不会执行

卸载时 

- componentWillUnmount ：清除 timer，取消网络请求或清除订阅

调用 `forceUpdate()` 将致使组件调用 `render()` 方法，此操作会跳过该组件的 `shouldComponentUpdate()`。但其子组件会触发正常的生命周期方法，避免使用该api

