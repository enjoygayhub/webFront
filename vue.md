# Vue

## MVVM 和 MVC的区别

- MVC: MVC是应用最广泛的软件架构之一,一般MVC分为:Model(模型),View(视图),Controller(控制器)。 这主要是基于分层的目的,让彼此的职责分开.View一般用过Controller来和Model进行联系。Controller是Model和View的协调者,View和Model不直接联系。基本都是单向联系。

1. View传送指令到Controller。
2. Controller完成业务逻辑后改变Model状态。
3. Model将新的数据发送至View,用户得到反馈。

- MVVM: MVVM是把MVC中的Controller改变成了ViewModel。

View的变化会自动更新到ViewModel,ViewModel的变化也会自动同步到View上显示,通过数据来显示视图层。

区别:

- MVC中Controller演变成MVVM中的ViewModel
- MVVM通过数据来显示视图层而不是节点操作
- MVVM主要解决了MVC中大量的dom操作使页面渲染性能降低,加载速度变慢,影响用户体验


## vue 的响应式

数据发生变化后，会重新对页面渲染
1. 数据劫持 / 数据代理
2. 依赖收集
3. 发布订阅模式

## Vue3.0 中通过使用 Proxy 对比 vue2.0 版本通过 Object.defineProperty() 

更全面的响应式： Proxy 可以监听数组索引、属性添加、删除等更多操作，而 Object.defineProperty 只能监听属性的读写。
性能提升： Proxy 的底层实现更加高效，尤其是在处理深层嵌套的数据时。
代码更简洁： Proxy 的 API 更简洁，使得响应式系统的实现更加优雅

get：
当访问对象中的某个属性的时候，会触发track函数，这个track函数是用来收集依赖的，也就是收集被访问的key相关的副作用。
set：
当对象中某个属性值发生变化的时候，就会触发trigger函数，这个函数主要是用来触发每个依赖对应副作用的执行。


## v-if 和 v-show 的区别

- v-if：每次都会重新删除或创建元素来控制 DOM 结点的存在与否，会触发组件生命周期

- v-show:是切换了元素的样式 display:none，display: block，隐藏时仍在DOM树中

因而 v-if 有较高的切换性能消耗，v-show 有较高的初始渲染消耗，适用于条件频繁改变的情况。

## vue 中nextTick 的实现原理

nextTick 会将传入的回调函数推入到一个微任务队列中。回调函数会在 DOM 更新完成后才会执行。
确保 DOM 更新后操作: 很多时候，我们需要在 DOM 更新完成后才能获取最新的 DOM 结构，或者执行一些依赖于 DOM 的操作。nextTick 就提供了这样的机制。
避免数据和视图不同步: 如果在数据更新后立即操作 DOM，可能会导致数据和视图不同步的问题。

不同环境的实现:
Promise.resolve(): 利用 Promise 的特性，将回调函数包装成一个 Promise，然后调用 Promise.resolve()。
MutationObserver: 创建一个 MutationObserver 实例，观察一个元素的变动，当变动发生时，触发回调函数。
MessageChannel: 创建一个 MessageChannel，通过 postMessage 来传递消息，从而触发回调函数。

## 为什么 vue2.0中 组件中的 data 必须是函数

data 必须声明为返回一个初始数据对象的函数，因为组件可能被用来创建多个实例。如果 data 仍然是一个纯粹的对象，则所有的实例将共享引用同一个数据对象！。

## vue3.0 的生命周期函数

- onBeforeMount:
  > 在挂载开始之前被调用。
- onMounted:
  > 实例被挂载后调用。
- onBeforeUpdate:
  > 视图更新前
- onUpdated:
  > 视图更新后
- onBeforeUnmount:
  > 卸载前
- onUnmounted:
  > 销毁后。
- onActivated:
  > 被 `keep-alive` 缓存的组件激活时调用。
- onDeactivated:
  > 被 `keep-alive` 缓存的组件销毁时调用。
- onErrorCapture:
  > 捕获错误时调用。

## vue中key属性的作用

key 的作用主要是为了高效的更新虚拟 DOM

key 的特殊 attribute 主要用在 Vue 的虚拟 DOM 算法，在新旧 nodes 对比时辨识 VNodes。如果不使用 key，Vue 会使用一种最大限度减少动态元素并且尽可能的尝试就地修改/复用相同类型元素的算法。而使用 key 时，它会基于 key 的变化重新排列元素顺序，并且会移除 key 不存在的元素。

有相同父元素的子元素必须有独特的 key。重复的 key 会造成渲染错误。

## Vue的路由模式

> hash模式 与 history模式

- hash（即地址栏 URL 中的 # 符号）。

特点： hash 虽然出现在 URL 中，但不会被包括在HTTP内，hash模式每次页面切换其实切换的是#之后的内容，而#后内容的改变并不会触发地址的改变，
因此改变 hash 不会重新加载页面。每次hash发生变化时都会调用 onhashchange事件

优点：可以随意刷新

- history（利用了浏览器的历史记录栈）

特点：利用了 HTML5 History Interface 中新增的 pushState() 和 replaceState() 方法。
在当前已有的 back、forward、go的前进 后退控制页面。

## vue中router和route的区别

- route：当前激活的路由的信息对象。每个对象都是局部的，可以获取当前路由的 path, name, params, query 等属性。
- router：全局的 router 实例。通过 vue 根实例中注入 router 实例，然后再注入到每个子组件，从而让整个应用都有路由功能。其中包含了很多属性和对象（比如 history 对象），任何页面也都可以调用其 push(), replace(), go() 等方法。

## 路由的两种跳转方式

声明式：< router-link :to="…">< /router-link>

编程式：router.push(…) ，router.replace(location)，router.go(n)

参数传递方式：
第一种方法：params传参，url中会显示参数， 页面刷新数据不会丢失。www.123.com/#/test/1/2
第二种方法：query传参，url中会显示参数，例如www.123.com/#/test?type=1&arg=2


## vue3.0 setup api

### ref 和 reactive
ref: 用来给基本数据类型绑定响应式数据，访问时需要通过 .value 的形式， template中会自动解析,不需要 .value
reactive: 用来给 复杂数据类型 绑定响应式数据，直接访问即可

### computed
计算属性.通过proxy响应式，依赖收集，缓存数据，只有当依赖的响应式数据发生变化时，才会重新计算。类似于React中的 useMemo

### toRef、toRefs、toRaw
toRef(object,key) ：解构object中的key属性,如果object为响应式的，那么返回的结果也是响应式的
toRefs(object) ： 循环调用toRef
toRaw(object)：  将响应式对象修改为普通对象


### watch WatchEffect
watch(data,()=>{},{})： 监听响应式状态发生变化的，当响应式状态发生变化时，就会触发一个回调函数。
观测部分数据如下：
watch(() => info.obj, (newV, oldV) => {
  console.log(newV, oldV)
}, {
  deep: true
})

WatchEffect(function,options)：会立即执行传入的一个函数，同时响应式追踪其依赖，并在其依赖变更时重新运行该函数。
注意：异步方式创建的监听器需要手动执行卸载，停止监听。

### defineProps  withDefaults
子组件接受参数:
defineProps<{
  msg: string,
  list: number[]
}>()

带默认值：
withDefaults(defineProps<Props>(), {
  msg: '张麻子',
  list: () => [4, 5, 6]
})

### defineEmits defineExpose 
子组件接受父组件方法
const emits = defineEmits(['handleClick'])
子组件暴露自身方法和属性
defineExpose({
  name,
  handleClick
})

### 动态组件component :is
使用<component :is="tab[currentTab]"><component> 

### slot
  用于渲染模板化内容
### 异步组件 Suspense
  const Children = defineAsyncComponent(() => import('./Children.vue'))

1、使用 <Suspense></Suspense> 包裹所有异步组件相关代码
2、<template v-slot:default></template> 插槽包裹异步组件
3、<template v-slot:fallback></template> 插槽包裹渲染异步组件渲染之前的内容

### Teleport 
Teleport 是一种能够将我们的模板渲染至指定DOM节点，不受父级style、v-show等属性影响，但data、prop数据依旧能够共用的技术
主要解决的问题：因为Teleport节点挂载在其他指定的DOM节点下，完全不受父级style样式影响
使用：通过to 属性插入到指定元素位置，如 body，html，自定义className等等。

### keep-alive 缓存组件
初次进入时： onMounted> onActivated
退出后触发 deactivated
再次进入：只会触发 onActivated

事件挂载的方法等，只执行一次的放在 onMounted中；组件每次进去执行的方法放在 onActivated中

### v-model
const emit = defineEmits(['update:modelValue'])
子组件内可以emit('update:modelValue', 'new')更新数据

### 自定义指令

### 自定义hooks
获取宽高
```ts
import { onMounted, onUnmounted, ref } from "vue";

function useWindowResize() {
  const width = ref(0);
  const height = ref(0);
  function onResize() {
    width.value = window.innerWidth;
    height.value = window.innerHeight;
  }
  onMounted(() => {
    window.addEventListener("resize", onResize);
    onResize();
  });
  onUnmounted(() => {
    window.removeEventListener("resize", onResize);
  });
  return {
    width,
    height
  };
}

export default useWindowResize;

```

### v-bind CSS变量注入
```js
<script lang="ts" setup>
  import { ref } from 'vue'
  const color = ref('red')
</script>
<style scoped>
  span {
    /* 使用v-bind绑定组件中定义的变量 */
    color: v-bind('color');
  }  
</style>

```

## 兄弟组件之间通信

1. eventBus 自定义事件中心
2. 使用状态管理库
3. 父组件中转

##  React 和 Vue差异：

### 1. 虚拟 DOM

**React** 和 **Vue** 都使用虚拟 DOM 的概念来提高渲染效率。虚拟 DOM 是内存中的 DOM 树副本，用于减少实际 DOM 的更新次数。React 和 Vue 都通过比较虚拟 DOM 的差异来决定实际 DOM 的更新操作。

### 2. 批量更新

- **React** 通过 `batching` 来批量处理状态更新，减少不必要的渲染。React 18 引入了 `useTransition` 和 `useSyncExternalStore` 等 Hooks，进一步增强了批量更新的能力。
- **Vue** 默认使用异步更新队列，这意味着所有状态更新都会在下一个微任务的结尾批量处理。这有助于减少不必要的渲染，提高性能。

### 3. 更新机制

- **React** 使用了合成事件和虚拟 DOM 的更新机制。当状态或 props 发生变化时，React 会重新渲染组件，并通过 Diff 算法来确定需要更新的实际 DOM 节点。
- **Vue** 使用了观察者模式来实现响应式。当数据变化时，Vue 会自动更新依赖这些数据的视图。Vue 3.0 使用了 Proxy 对象来实现数据的响应式，这比 Vue 2.0 中的 Object.defineProperty 更高效。

### 4. 优化工具

- **React** 提供了 `React.memo` 和 `useMemo`、`useCallback` 等 Hooks 来帮助开发者优化性能。此外，还有 `StrictMode` 来帮助发现潜在的性能问题。
- **Vue** 也提供了类似的功能，如 `v-memo`（社区提供的解决方案）和 `computed` 属性来缓存计算结果。Vue 3.0 中的 `ref` 和 `reactive` API 也提供了更多的灵活性来优化性能。

### 5. 性能测试

- **React** 和 **Vue** 在大多数基准测试中表现相近。在某些场景下，Vue 的异步更新队列和更细粒度的依赖追踪可能会带来一些性能优势。
- **React** 在某些大规模应用中，尤其是涉及大量状态更新和复杂的组件树时，其批量更新和合成事件的优势可能会更加明显。

### 6. 生态系统

- **React** 和 **Vue** 都拥有丰富的生态系统和社区支持。React 的生态系统相对较大，有更多的第三方库和工具可供选择，这也可能会影响性能和开发效率。

### 7. 服务端渲染

- **React** 和 **Vue** 都支持服务端渲染（SSR）。React 的 SSR 支持较为成熟，而 Vue 3.0 通过官方的 `vue-server-renderer` 和社区提供的解决方案也有很好的 SSR 支持。

### 8. 框架特性

- **React** 的核心库相对较小，更多功能是通过社区提供的库来实现。这使得 React 可以保持轻量级，但也可能增加学习曲线和配置复杂度。
- **Vue** 的核心库集成了更多功能，如状态管理、路由等，这使得 Vue 在开箱即用方面表现更好，但也可能增加应用的初始加载时间。

### React、Vue2、Vue3的三种Diff算法

1. Diff 算法的基本原理
虚拟 DOM： 将真实的 DOM 结构抽象成一个 JavaScript 对象，这个对象就是虚拟 DOM。
差异对比： 比较新旧虚拟 DOM，找出其中的差异。
最小化更新： 根据差异，对真实的 DOM 进行最小化的更新。
2. React 的 Diff 算法
只对同层级的组件进行比较。
采用了一种类似于树形结构的递归算法，逐层比较子节点。
使用 key 属性来标识唯一节点，从而提高 diff 效率。
引入了 Fiber 架构，将 diff 过程分成多个子任务，使其能够在渲染过程中被打断和恢复。
3. Vue2 的 Diff 算法
双端比较： 从头尾两端同时进行比较，减少比较次数。
静态节点优化： 对静态节点进行缓存，避免重复 diff。
key 属性优化： 通过 key 属性来标识唯一节点，从而提高 diff 效率。
4. Vue3 的 Diff 算法
静态标记： 对静态节点进行缓存，避免重复 diff。
PatchFlag： 标记需要更新的节点，从而减少不必要的更新。
Block Tree： 将多个节点组成一个块，减少 diff 次数。