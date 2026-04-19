# Vue

## vue 的响应式原理

1. 数据劫持 / 数据代理
2. 依赖收集
3. 发布订阅模式

## Vue3.0 中通过使用 Proxy 对比 vue2.0 版本通过 Object.defineProperty() 

更全面的响应式： Proxy 可以监听数组索引、属性添加、删除等更多操作，而 Object.defineProperty 只能监听属性的读写。
性能提升： Proxy 的底层实现更加高效，尤其是在处理深层嵌套的数据时。
代码更简洁： Proxy 的 API 更简洁，使得响应式系统的实现更加优雅

get：
当访问对象中的某个属性的时候，会触发track函数，这个track函数是用来收集依赖的。
set：
当对象中某个属性值发生变化的时候，就会触发trigger函数。


## v-if 和 v-show 的区别

- v-if：每次都会重新删除或创建元素来控制 DOM 结点的存在与否，会触发组件生命周期

- v-show:是切换了元素的样式 display:none，display: block，隐藏时仍在DOM树中

因而 v-if 有较高的切换性能消耗，v-show 有较高的初始渲染消耗，适用于条件频繁改变的情况。

## vue 中nextTick 

nextTick 会将传入的回调函数推入到一个微任务队列中。回调函数会在 DOM 更新完成后才会执行。
确保在 DOM 更新完成后才能获取最新的 DOM 结构，或者执行一些依赖于 DOM 的操作。
避免数据和视图不同步。


## 为什么 vue2.0中 组件中的 data 必须是函数

因为组件可能被用来创建多个实例。如果 data 仍然是一个纯粹的对象，则所有的实例将共享引用同一个数据对象！。

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

不使用 key，Vue 会使用一种最大限度减少动态元素并且尽可能的尝试就地修改/复用相同类型元素的算法。而使用 key 时，它会基于 key 的变化重新排列元素顺序，并且会移除 key 不存在的元素。


## Vue的路由模式


- hash（即地址栏 URL 中的 # 符号）。

特点： hash 虽然出现在 URL 中，每次页面切换其实切换的是#之后的内容，不会触发地址的改变，
因此改变 hash 不会重新加载页面。每次hash发生变化时都会调用 onhashchange事件

优点：可以随意刷新

- history（利用了浏览器的历史记录栈）

特点：利用History的 pushState() 和 replaceState() 方法。
在当前已有的 back、forward、go的前进 后退控制页面。


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
  slot 就是让父组件可以往子组件里 “塞内容” 的口子，实现内容分发、模板复用。
  
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
directives: {
  myDir: {
    // 1. 绑定（只执行一次）
    created(el, binding) {},
    // 2. 挂载到 DOM
    mounted(el, binding) {},
    // 3. 组件更新
    updated(el, binding) {},
    // 4. 卸载
    unmounted(el, binding) {}
  }
}
Vue3 钩子：created/mounted/updated/unmounted（更贴近生命周期）
适合：聚焦、拖拽、权限、节流、复制、懒加载等

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

1. eventBus事件中心（vue3废弃）
2. 使用全局状态
3. 父组件中转
4. Provide/Inject → Vue3 推荐、跨层级

##  React 和 Vue 事件绑定的差异：

Vue：
  基于原生 DOM 事件
  使用 addEventListener 直接绑定
  支持强大的事件修饰符
   this 自动指向组件实例
  事件对象就是原生 event
  灵活：可委托、可直接绑定
  
React：
  自己实现合成事件系统（SyntheticEvent）
  全部事件委托到 root 节点（React17+）
  不使用原生 onclick 等属性
  须手动绑定 this 或箭头函数
  事件对象是合成的，非原生
  目的：跨平台、统一机制、优化更新

### React、Vue2、Vue3的三种Diff算法

都是同层比较，不跨层级 Diff（时间复杂度从 O (n³) → O (n)）
都用 key 判定节点是否可复用
都是虚拟 DOM 对比，只把差异更新到真实 DOM

1. 基本原理
虚拟 DOM： 将真实的 DOM 结构抽象成一个 JavaScript 对象，这个对象就是虚拟 DOM。
差异对比： 比较新旧虚拟 DOM，找出其中的差异。
最小化更新： 根据差异，对真实的 DOM 进行最小化的更新。

2. React 的 Diff 算法
单指针从左到右遍历 + key 索引表查找

3. Vue2 的 双端比较

核心策略：首尾双指针+ 4 种命中 + 查找复用

过程：
新旧数组各有 头指针、尾指针
依次尝试 4 种匹配：
旧头 ↔ 新头
旧尾 ↔ 新尾
旧头 ↔ 新尾
旧尾 ↔ 新头
命中一种就移动指针，继续比较
都没命中：遍历旧列表查 key，能复用就移动，不能就新建
最后处理剩余节点（新增 / 删除）

优点：对逆序、首尾操作优化很好，移动次数少，DOM 操作较少
缺点：中间乱序较多时，遍历查找 key 效率低 O (n²)


4. Vue3 的 Diff 算法 最长递增子序列（最强）

核心策略：前置相同节点 + 后置相同节点 + 中间乱序用 LIS 最小化移动

步骤
从前向后比，直到节点不同
从后向前比，直到节点不同
中间剩下的就是乱序片段 
建立旧节点 key → index 的映射表
生成新序列在旧序列中的位置序列
求这个序列的 最长递增子序列 LIS
LIS 里的节点不动，其他节点移动 / 新增

优点
乱序场景性能大幅提升，DOM 移动次数最少，理论最优，整体更稳定、高效