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

## vue 双向数据绑定原理

> 双向数据绑定主要是通过使用数据劫持和发布订阅者模式来实现的。

> vue2.0 版本通过 Object.defineProperty() 方法来对 Model 数据各个属性添加访问器属性，以此来实现数据的劫持，因此当 Model 中的数据发生变化的时候，我们可以通过配置的 setter 和 getter 方法来实现对 View 层数据更新的通知。

> 数据在 html 模板中一共有两种绑定情况，一种是使用 v-model 来对 value 值进行绑定，一种是作为文本绑定，在对模板引擎进行解析的过程中。

> 如果遇到元素节点，并且属性值包含 v-model 的话，我们就从 Model 中去获取 v-model 所对应的属性的值，并赋值给元素的 value 值。然后给这个元素设置一个监听事件，当 View 中元素的数据发生变化的时候触发该事件，通知 Model 中的对应的属性的值进行更新。

> 如果遇到了绑定的文本节点，我们使用 Model 中对应的属性的值来替换这个文本。对于文本节点的更新，我们使用了发布订阅者模式，属性作为一个主题，我们为这个节点设置一个订阅者对象，将这个订阅者对象加入这个属性主题的订阅者列表中。当 Model 层数据发生改变的时候，Model 作为发布者向主题发出通知，主题收到通知再向它的所有订阅者推送，订阅者收到通知后更改自己的数据。

在 Vue3.0 中通过使用 Proxy 对对象进行代理，从而实现数据劫持。使用 Proxy 的好处是它可以完美的监听到任何方式的数据改变

## v-if 和 v-show 的区别

- v-if：每次都会重新删除或创建元素来控制 DOM 结点的存在与否，会触发组件生命周期

- v-show:是切换了元素的样式 display:none，display: block

因而 v-if 有较高的切换性能消耗，v-show 有较高的初始渲染消耗，适用于条件频繁改变的情况。

## 为什么 vue2.0中 组件中的 data 必须是函数

当一个组件被定义，data 必须声明为返回一个初始数据对象的函数，因为组件可能被用来创建多个实例。如果 data 仍然是一个纯粹的对象，则所有的实例将共享引用同一个数据对象！通过提供 data 函数，每次创建一个新实例后，我们能够调用 data 函数，从而返回初始数据的一个全新副本数据对象。

简而言之，就是 data 中数据可能会被复用，要保证不同组件调用的时候数据是相同的。

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

- hash（即地址栏 URL 中的 # 符号)。比如这个 URL：www.123.com/#/test，hash 的值为 #/test。

特点： hash 虽然出现在 URL 中，但不会被包括在HTTP内，hash模式每次页面切换其实切换的是#之后的内容，而#后内容的改变并不会触发地址的改变，
因此改变 hash 不会重新加载页面。
每次hash发生变化时都会调用 onhashchange事件

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

## 为什么需要虚拟DOM，它有什么好处?

​    Web界面由DOM树(树的意思是数据结构)来构建，当其中一部分发生变化时，其实就是对应某个DOM节点发生了变化，虚拟DOM就是为了**解决浏览器性能问题**而被设计出来的。若一次操作中有10次更新DOM的动作，虚拟DOM不会立即操作DOM，而是将这10次更新的diff内容保存到本地一个JS对象中，最终将这个JS对象一次性attach到DOM树上，再进行后续操作，避免大量无谓的计算量。用JS对象模拟DOM节点的好处是，页面的更新可以先全部反映在JS对象(虚拟DOM)上，操作内存中的JS对象的速度显然要更快，等更新完成后，再将最终的JS对象映射成真实的DOM，交由浏览器去绘制。



## vue3.0 setup api

### ref 和 reactive
ref: 用来给基本数据类型绑定响应式数据，访问时需要通过 .value 的形式， template中会自动解析,不需要 .value
reactive: 用来给 复杂数据类型 绑定响应式数据，直接访问即可

### toRef、toRefs、toRaw
toRef(object,key) ：结构object中的key属性,如果object为响应式的，那么返回的结果也是响应式的
toRefs(object) ： 循环调用toRef
toRaw(object)：  将响应式对象修改为普通对象
computed: 得到计算属性

### watch WatchEffect
watch(data,()=>{},{})： 监听响应式状态发生变化的，当响应式状态发生变化时，就会触发一个回调函数。
观测部分数据如下：
watch(() => info.obj, (newV, oldV) => {
  console.log(newV, oldV)
}, {
  deep: true
})

WatchEffect：会立即执行传入的一个函数，同时响应式追踪其依赖，并在其依赖变更时重新运行该函数。
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