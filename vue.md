# Vue

- [Vue](#vue)
  - [MVVM 和 MVC的区别](#mvvm-和-mvc的区别)
  - [vue 的优点](#vue-的优点)
  - [vue 的响应式原理](#vue-的响应式原理)
  - [vue 双向数据绑定原理](#vue-双向数据绑定原理)
  - [Object.defineProperty 介绍](#objectdefineproperty-介绍)
  - [使用 Object.defineProperty() 来进行数据劫持有什么缺点](#使用-objectdefineproperty-来进行数据劫持有什么缺点)
  - [v-if 和 v-show 的区别](#v-if-和-v-show-的区别)
  - [为什么 vue 组件中的 data 必须是函数](#为什么-vue-组件中的-data-必须是函数)
  - [vue 的生命周期函数](#vue-的生命周期函数)
  - [vue 的 activated 和 deactivated 钩子函数](#vue-的-activated-和-deactivated-钩子函数)
  - [Vue中父子组件生命周期执行顺序](#vue中父子组件生命周期执行顺序)
  - [nextTick 用法](#nexttick-用法)
  - [vue中key属性的作用](#vue中key属性的作用)
  - [Vue中key属性用index为什么不行](#vue中key属性用index为什么不行)
  - [Vue的路由模式](#vue的路由模式)
  - [vue中\$router和\$route的区别](#vue中router和route的区别)
  - [Vue diff算法详解](#vue-diff算法详解)
  - [移动端适配的方法](#移动端适配的方法)
  - [rem 原理](#rem-原理)
  - [rem 和 em 的区别](#rem-和-em-的区别)
  - [移动端 300ms 延迟的原因以及解决方案](#移动端-300ms-延迟的原因以及解决方案)
  - [Vue 和 React 数据驱动的区别](#vue-和-react-数据驱动的区别)

## MVVM 和 MVC的区别

- MVC: MVC是应用最广泛的软件架构之一,一般MVC分为:Model(模型),View(视图),Controller(控制器)。 这主要是基于分层的目的,让彼此的职责分开.View一般用过Controller来和Model进行联系。Controller是Model和View的协调者,View和Model不直接联系。基本都是单向联系。



1. View传送指令到Controller。
2. Controller完成业务逻辑后改变Model状态。
3. Model将新的数据发送至View,用户得到反馈。

- MVVM: MVVM是把MVC中的Controller改变成了ViewModel。

View的变化会自动更新到ViewModel,ViewModel的变化也会自动同步到View上显示,通过数据来显示视图层。



MVVM和MVC的区别:

- MVC中Controller演变成MVVM中的ViewModel
- MVVM通过数据来显示视图层而不是节点操作
- MVVM主要解决了MVC中大量的dom操作使页面渲染性能降低,加载速度变慢,影响用户体验

## vue 的优点

- 轻量级框架
- 简单易学
- 双向数据绑定
- 组件化
- 视图，数据，结构分离
- 虚拟 DOM
- 运行速度更快

## 4大框架对比

组件嵌套组件 多次，传递数据的方法；对于angular是依赖注入；vue用过previde() and inject()

React --Context API；Ember：services

渲染元素：angular ：增量dom；react vue：虚拟dom，保存完整的副本；

## vue 的响应式原理

数据发生变化后，会重新对页面渲染，这就是 Vue 响应式

想完成这个过程，我们需要：

- 侦测数据的变化
- 收集视图依赖了哪些数据
- 数据变化时，自动“通知”需要更新的视图部分，并进行更新

对应专业俗语分别是：

数据劫持 / 数据代理
依赖收集
发布订阅模式

## vue 双向数据绑定原理

> vue 通过使用双向数据绑定，来实现了 View 和 Model 的同步更新。vue 的双向数据绑定主要是通过使用数据劫持和发布订阅者模式来实现的。
>
> 首先我们通过 Object.defineProperty() 方法来对 Model 数据各个属性添加访问器属性，以此来实现数据的劫持，因此当 Model 中的数据发生变化的时候，我们可以通过配置的 setter 和 getter 方法来实现对 View 层数据更新的通知。
>
> 数据在 html 模板中一共有两种绑定情况，一种是使用 v-model 来对 value 值进行绑定，一种是作为文本绑定，在对模板引擎进行解析的过程中。
>
> 如果遇到元素节点，并且属性值包含 v-model 的话，我们就从 Model 中去获取 v-model 所对应的属性的值，并赋值给元素的 value 值。然后给这个元素设置一个监听事件，当 View 中元素的数据发生变化的时候触发该事件，通知 Model 中的对应的属性的值进行更新。
>
> 如果遇到了绑定的文本节点，我们使用 Model 中对应的属性的值来替换这个文本。对于文本节点的更新，我们使用了发布订阅者模式，属性作为一个主题，我们为这个节点设置一个订阅者对象，将这个订阅者对象加入这个属性主题的订阅者列表中。当 Model 层数据发生改变的时候，Model 作为发布者向主题发出通知，主题收到通知再向它的所有订阅者推送，订阅者收到通知后更改自己的数据。

## Object.defineProperty 介绍

> Object.defineProperty 函数一共有三个参数，第一个参数是需要定义属性的对象，第二个参数是需要定义的属性，第三个是该属性描述符。
>
> 一个属性的描述符有一下属性，分别是
> value 属性的值，
> writable 属性是否可写，
> enumerable 属性是否可枚举，
> configurable 属性是否可配置修改。
> get 当访问该属性时，会调用此函数
> set 当属性值被修改时，会调用此函数。

## 使用 Object.defineProperty() 来进行数据劫持有什么缺点

有一些对属性的操作，使用这种方法无法拦截，比如说通过下标方式修改数组数据或者给对象新增属性，vue 内部通过重写函数解决了这个问题。

在 Vue3.0 中已经不使用这种方式了，而是通过使用 Proxy 对对象进行代理，从而实现数据劫持。使用 Proxy 的好处是它可以完美的监听到任何方式的数据改变，唯一的缺点是兼容性的问题，因为这是 ES6 的语法。

## v-if 和 v-show 的区别

- v-if：每次都会重新删除或创建元素来控制 DOM 结点的存在与否

- v-show:是切换了元素的样式 display:none，display: block

因而 v-if 有较高的切换性能消耗，v-show 有较高的初始渲染消耗

## 为什么 vue 组件中的 data 必须是函数

<!-- 一个组件被复用多次的话，也就会创建多个实例。本质上，这些实例用的都是同一个构造函数，如果data是对象的话，对象属性引用类型，会影响到所有的实例，为了保证组件不同的实例之间的data互不冲突，data必须是一个函数。 -->

当一个组件被定义，data 必须声明为返回一个初始数据对象的函数，因为组件可能被用来创建多个实例。如果 data 仍然是一个纯粹的对象，则所有的实例将共享引用同一个数据对象！通过提供 data 函数，每次创建一个新实例后，我们能够调用 data 函数，从而返回初始数据的一个全新副本数据对象。

简而言之，就是 data 中数据可能会被复用，要保证不同组件调用的时候数据是相同的。

## vue 的生命周期函数

- beforeCreate:
  > 在实例初始化之后，数据观测 (data observer) 和 event/watcher 事件配置之前被调用。
  >
  > 在new一个vue实例后，只有一些默认的生命周期钩子和默认事件，其他的东西都还没创建。在beforeCreate生命周期执行的时候，data和methods中的数据都还没有初始化。不能在这个阶段使用data中的数据和methods中的方法
- created:
  > 在实例创建完成后被立即调用。在这一步，实例已完成以下的配置：数据观测 (data observer)，property 和方法的运算，watch/event 事件回调。然而，挂载阶段还没开始，\$el property 目前尚不可用。
  >
  > data 和 methods都已经被初始化好了，如果要调用 methods 中的方法，或者操作 data 中的数据，最早可以在这个阶段中操作
- beforeMount:
  > 在挂载开始之前被调用：相关的 render 函数首次被调用。
  >
  > 执行到这个钩子的时候，在内存中已经编译好了模板了，但是还没有挂载到页面中，此时，页面还是旧的
- mounted:
  > 实例被挂载后调用，这时 el 被新创建的 vm.\$el 替换了。如果根实例挂载到了一个文档内的元素上，当 mounted 被调用时 vm.\$el 也在文档内。
  >
  > 执行到这个钩子的时候，就表示Vue实例已经初始化完成了。此时组件脱离了创建阶段，进入到了运行阶段。如果我们想要通过插件操作页面上的DOM节点，最早可以在这个阶段中进行
- beforeUpdate:
  
  > 当执行这个钩子时，页面中的显示的数据还是旧的，data中的数据是更新后的， 页面还没有和最新的数据保持同步
- updated:
  
  > 页面显示的数据和data中的数据已经保持同步了，都是最新的
- beforeDestroy:
  
  > Vue实例从运行阶段进入到了销毁阶段，这个时候上所有的 data 和 methods，指令，过滤器……都是处于可用状态，还没有真正被销毁
- destroyed:
  
  > 这个时候上所有的 data 和 methods，指令，过滤器……都是处于不可用状态，组件已经被销毁了。
- activated:
  
  > 被 `keep-alive` 缓存的组件激活时调用。
- deactivated:
  
  > 被 `keep-alive` 缓存的组件停用时调用。

## vue 的 activated 和 deactivated 钩子函数

```html
<keep-alive>
  <component :is="view"></component>
</keep-alive>
```

`keep-alive`包裹动态组件时，会缓存不活动的组件实例，而不是销毁它们。

当组件在 `<keep-alive>` 内被切换，它的 `activated` 和 `deactivated` 这两个生命周期钩子函数将会被对应执行。

- `activated`在`keep-alive`组件激活时调用，该钩子函数在服务器端渲染期间不被调用。
- `deactivated`在`keep-alive`组件停用时调用，该钩子函数在服务端渲染期间不被调用。

## Vue中父子组件生命周期执行顺序

在单一组件中，钩子的执行顺序是beforeCreate-> created -> mounted->... ->destroyed

父子组件生命周期执行顺序：

- 加载渲染过程

  ```txt
  父beforeCreate->父created->父beforeMount->子beforeCreate->子created->子beforeMount->子mounted->父mounted
  ```

- 更新过程

  ```txt
  父beforeUpdate->子beforeUpdate->子updated->父updated
  ```

- 销毁过程

  ```txt
  父beforeDestroy->子beforeDestroy->子destroyed->父destroyed
  ```

- 常用钩子简易版

  ```txt
  父create->子created->子mounted->父mounted
  ```

## nextTick 用法

官网解释：

> 将回调延迟到下次 DOM 更新循环之后执行。在修改数据之后立即使用它，然后等待 DOM 更新。

```html
<div class="app">
  <div ref="msgDiv">{{msg}}</div>
  <div v-if="msg1">Message got outside $nextTick: {{msg1}}</div>
  <div v-if="msg2">Message got inside $nextTick: {{msg2}}</div>
  <div v-if="msg3">Message got outside $nextTick: {{msg3}}</div>
  <button @click="changeMsg">
    Change the Message
  </button>
</div>
```

```vue
new Vue({
  el: '.app',
  data: {
    msg: 'Hello Vue.',
    msg1: '',
    msg2: '',
    msg3: ''
  },
  methods: {
    changeMsg() {
      this.msg = "Hello world."
      this.msg1 = this.$refs.msgDiv.innerHTML
      this.$nextTick(() => {
        this.msg2 = this.$refs.msgDiv.innerHTML
      })
      this.msg3 = this.$refs.msgDiv.innerHTML
    }
  }
})
```

## vue中key属性的作用

一句话 key 的作用主要是为了高效的更新虚拟 DOM

key 的特殊 attribute 主要用在 Vue 的虚拟 DOM 算法，在新旧 nodes 对比时辨识 VNodes。如果不使用 key，Vue 会使用一种最大限度减少动态元素并且尽可能的尝试就地修改/复用相同类型元素的算法。而使用 key 时，它会基于 key 的变化重新排列元素顺序，并且会移除 key 不存在的元素。

有相同父元素的子元素必须有独特的 key。重复的 key 会造成渲染错误。

## Vue中key属性用index为什么不行

这是由于diff算法的机制所决定的，话不多说，直接上反例：

当我们选中某一个（比如第3个），再添加或删除内容的时候就能发现bug了

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>

</head>
<body>
    <div id="app">
        <span>ID:</span><input type="text" v-model="id">
        <span>Name:</span><input type="text" v-model="name">
        <button @click="handleClick">添加</button>

        <div v-for="(item, index) in list" :key="index">
            <input type="checkbox" />
            <span @click="handleDelete(index)">{{item.id}} --- {{item.name}}</span>
        </div>
    </div>
    <script>
        let vm = new Vue({
            el: '#app',
            data: {
                id: '',
                name: '',
                list: [
                    {id: 1, name: '张三'},
                    {id: 2, name: '李四'},
                    {id: 3, name: '王五'},
                    {id: 4, name: '赵六'},
                ]
            },
            methods: {
                handleClick() {
                    this.list.unshift({
                        id: this.id,
                        name: this.name
                    })
                },
                handleDelete(index) {
                    this.list.splice(index, 1)
                }
            },
        })
    </script>
</body>
</html>

```

## Vue的路由模式

> hash模式 与 history模式

- hash（即地址栏 URL 中的 # 符号)。

```txt
比如这个 URL：www.123.com/#/test，hash 的值为 #/test。

特点： hash 虽然出现在 URL 中，但不会被包括在 HTTP，因为我们hash每次页面切换其实切换的是#之后的内容，而#后内容的改变并不会触发地址的改变，
所以不存在向后台发出请求，对后端完全没有影响，因此改变 hash 不会重新加载页面。

每次hash发生变化时都会调用 onhashchange事件

优点：可以随意刷新
```

- history（利用了浏览器的历史记录栈）

```txt
特点：利用了 HTML5 History Interface 中新增的 pushState() 和 replaceState() 方法。（需要特定浏览器支持）

在当前已有的 back、forward、go的基础之上，它们提供了对历史记录进行修改的功能。只是当它们执行修改时，虽然改变了当前的URL，但浏览器不会立即向后端发送请求。

history：可以通过前进 后退控制页面的跳转，刷新是真是的改变url。

缺点：不能刷新，需要后端进行配置。由于history模式下是可以自由修改请求url，当刷新时如果不对对应地址进行匹配就会返回404。
但是在hash模式下是可以刷新的，前端路由修改的是#中的信息，请求时地址是不会变的
```

## vue中\$router和\$route的区别

- this.\$route：当前激活的路由的信息对象。每个对象都是局部的，可以获取当前路由的 path, name, params, query 等属性。
- this.\$router：全局的 router 实例。通过 vue 根实例中注入 router 实例，然后再注入到每个子组件，从而让整个应用都有路由功能。其中包含了很多属性和对象（比如 history 对象），任何页面也都可以调用其 push(), replace(), go() 等方法。
- routes：指创建路由实例的配置项。用来配置多个route路由对象

## 路由的两种跳转方式

声明式：< router-link :to="…">< /router-link>

编程式：router.push(…) ，router.replace(location)，router.go(n)

参数传递方式：第一种方法：params传参，url中会显示参数， 页面刷新数据不会丢失。

第三种方法：query传参，url中会显示参数

## 组件通信方式

常见使用场景可以分为三类：

- 父子通信：
  父向子传递数据是通过 props，子向父是通过 events（`$emit`）；通过父链 / 子链也可以通信（`$parent` / `$children`）；ref 也可以访问组件实例；provide / inject API；`$attrs/$listeners`
- 兄弟通信：
  Bus；Vuex
- 跨级通信：
  Bus；Vuex；provide / inject API、`$attrs/$listeners`

## 组件混入对象

mixin的作用是多个组件可以共享数据和方法，在使用mixin的组件中引入后，mixin中的方法和属性也就并入到该组件中，可以直接使用，在已有的组件数据和方法进行了扩充。引入mixin分全局引用和局部引用。

#### 为什么需要虚拟DOM，它有什么好处?

​    Web界面由DOM树(树的意思是数据结构)来构建，当其中一部分发生变化时，其实就是对应某个DOM节点发生了变化，虚拟DOM就是为了**解决浏览器性能问题**而被设计出来的。**如前**，若一次操作中有10次更新DOM的动作，虚拟DOM不会立即操作DOM，而是将这10次更新的diff内容保存到本地一个JS对象中，最终将这个JS对象一次性attch到DOM树上，再进行后续操作，避免大量无谓的计算量。**所以，**用JS对象模拟DOM节点的好处是，页面的更新可以先全部反映在JS对象(虚拟DOM)上，操作内存中的JS对象的速度显然要更快，等更新完成后，再将最终的JS对象映射成真实的DOM，交由浏览器去绘制。

## 请求器和拦截器

请求拦截器
请求拦截器的作用是在请求发送前进行一些操作，例如在每个请求体里加上token，统一做了处理如果以后要改也非常容易。

关于拦截，这里只说原理，前端的请求，最终还是离不开 ajax，像vue 的 vue-resource 、axios，都只是对ajax进行了统一的封装，它暴露出来的拦截器，其实就是写了一个方法，把ajax写在这个方法里面，（我们先说请求拦截器哈）在执行这个方法的时候，先将请求时要添加给请求头的那些数据（token、后端要的加密码…具体要看实际情况）先执行一遍，都赋值给一个变量，然后再统一传给ajax，接下来就是执行ajax，这就是所谓的请求拦截，其实就是先执行要添加的数据，然后再执行ajax，如果把这个添加数据的过程抽出来，就成了所谓的请求拦截器；

响应拦截器：
响应拦截器的作用是在接收到响应后进行一些操作，例如在服务器返回登录状态失效，需要重新登录的时候，跳转到登录页。

响应拦截器也是一样如此，就是在请求结果返回后，先不直接导出，而是先对响应码等等进行处理，处理好后再导出给页面，如果将这个对响应码的处理过程抽出来，就成了所谓的响应拦截器；


## Vue diff算法详解

- updateChildren

> 这个函数是用来比较两个结点的子节点



## Vue 和 React 数据驱动的区别

在数据绑定上来说，vue的特色是双向数据绑定，而在react中是单向数据绑定。

vue中实现数据绑定靠的是数据劫持（Object.defineProperty()）+发布-订阅模式

vue中实现双向绑定

```html
<input v-model="msg" />
```

react中实现双向绑定

```html
<input value={this.state.msg} onChange={() => this.handleInputChange()} />
```
