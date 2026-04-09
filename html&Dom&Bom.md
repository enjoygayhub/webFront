# HTML

## 从html到DOM

1 字节解码：按编码把字节流转为字符流
2 分词生成 Token：状态机识别标签、文本、属性等 Token
3 语法分析构建 DOM：栈结构处理嵌套，节点挂树
4 容错补全：自动修复错误、补全缺失标签，形成完整 DOM


## HTML 页面加载过程

### 1导航

- DNS 查询
- TCP3次握手
- TSL协商

### 2html响应

- 一个get请求返回html文件，首字节时间（TTFB）是用户通过点击链接进行请求与收到第一个 HTML 数据包之间的时间。第一个内容分块通常是 14KB 的数据。
- 拥塞控制 / TCP 慢启动

### 3解析
图片和css下载不会阻塞解析，但同步脚本执行会。
等待获取 CSS 不会阻塞 HTML 的解析或者下载，但是会阻塞 JavaScript
- 构建Dom树
- 预加载扫描器会解析可用的内容并请求高优先级的资源，如 CSS、JavaScript 和 web 字体。不用等主线程解析器找到对外部资源的引用时才去请求。
- 构建 CSSOM 树 (与html构建dom的流程类似)
- JavaScript 编译（脚本被解析为抽象语法树。浏览器引擎会将抽象语法树输入编译器，输出字节码。

### 4渲染

- 将 DOM 和 CSSOM 组合成渲染树，渲染树包含所有可见节点的内容和计算样式
- 布局（确定每个节点的大小和位置）
- 绘制（生成各层的位图（像素数据，不考虑层叠、位置、顺序）
- 合成（GPU 把多层位图合并成一张完整画面 排序所有图层


### 5交互
- 可交互时间（TTI）是测量从第一个请求导致 DNS 查询和 SSL 连接到页面可交互时所用的时间——可交互是在首次内容绘制之后页面在 50ms 内响应用户的交互。

## defer 和 async 的区别

- defer 异步加载，延迟执行。当html解析完毕后再执行，在 DOMContentLoaded 事件触发之前完成。

  多个脚本按顺序执行。

- async 异步加载，加载好，立即执行。执行仍然会阻塞文档的解析，只是它的加载过程不会阻塞。

  多个脚本的执行顺序无法保证。
  

# Dom & Bom

## window的screenX, screenY,outerHeight/innerHeight outerWidth/innerWidth

screenX, screenY 浏览器的边界到系统屏幕左和顶边界的距离
innerHeight，innerWidth 浏览器显示页面内容的窗口大小（不包含工具栏、导航栏等等），亦为下面的client
outerHeight，outerWidth 表示整个浏览器窗口的宽度，包括边框、菜单栏, 底部状态栏从,侧边栏

## Element的clientWidth offsetWidth scrollWidth

- 元素宽度clientWidth= width + 左右padding (不包含border,margin,与boxsizing有关)
- 元素布局宽度offsetWidth=width + 左右padding + 左右border + 滚动条（不包含margin）
- 元素内容宽度scrollWidth 测量方式和clientWidth计算方式相同。区别在于包括由于overflow溢出而在屏幕上不可见的内容。

## 事件对象中的clientX offsetX screenX pageX，

- [clientX, clientY]
  client客户端，客户端窗口就是指游览器的显示页面内容的窗口大小（不包含工具栏、导航栏等等）
  [clientX, clientY]就是鼠标点击位置距客户端窗口左边界和上边界的距离

- [screenX, screenY]
  获取鼠标点击位置在屏幕显示器的距离，根据屏幕分辨率来计算，多显示器分屏也能叠加。

- [offsetX, offsetY]

offset意为偏移量,鼠标点击的点距离与点击的**元素**的左边界和上边界的距离。边界计算不包含margin border

- [pageX, pageY]

page为页面的意思。在没有由于滚动而隐藏的区域时pageX, pageY与clientX, clientY大小一样
[pageX, pageY]是鼠标点击的点距离client边界的距离，包括由于滚动隐藏的不可见的空间距离。

其中page与offset常用属性。

## 事件模型以及三种事件绑定方法

现代浏览器事件模型二个过程：捕获；冒泡
事件捕获阶段。捕获指的是事件从html根元素向下传播到实际点击的目标元素（指在dom文档树中向下层），依次检查经过的节点是否绑定了该事件监听函数，如果有且该事件是设定在捕获阶段执行，便执行。

冒泡阶段：捕获阶段结束后，反向再检查一次，冒泡回根元素

事件绑定方法：

- 行内事件，过时了，不建议使用,没有捕获与冒泡，节点复制后事件丢失

```html
<button onclick="alert('ok')">Press me</button>
```

- js中在元素节点上添加事件属性，缺点：重复定义后者会覆盖前者

  ```js
  document.querySelector("button").onclick = alert("ok");
  ```

- addEventListener（eventName，function，useCapture），常用，可取消

  ```js
  const btn = document.querySelector("button");

  function bgChange() {
    const rndCol =
      "rgb(" + random(255) + "," + random(255) + "," + random(255) + ")";
    document.body.style.backgroundColor = rndCol;
  }
  // 此处为click没有on
  btn.addEventListener("click", bgChange);
  // 取消
  btn.removeEventListener("click", bgChange);
  ```

addEventListener，其中第三个参数useCapture可以指定事件是否在捕获阶段执行。默认是false，在冒泡阶段执行。
现代浏览器中，第三个参数可传对象option：{capture:boolean,once:boolean,passive:boolean,signal:AbortSignal}。
capture效果同useCapture，once为true时事件仅执行一次，这两个常用，兼容性良好，下面2个兼容性不好，也不常用
passive为true时，事件的preventDefault()无效，用于优化滚屏性能，signal可以用于设置移除监听。

注：capture选项不相同，事件回调函数可以被重复添加

## 事件代理/事件委托 以及 优缺点

> 事件委托本质上是利用了浏览器事件冒泡的机制。因为事件在冒泡过程中会上传到父节点，并且父节点可以通过事件对象获取到目标节点，因此可以把子节点的监听函数定义在父节点上，由父节点的监听函数统一处理多个子元素的事件，这种方式称为事件代理。
>
> 使用事件代理我们可以不必要为每一个子元素都绑定一个监听事件，这样减少了内存上的消耗。并且使用事件代理我们还可以实现事件的动态绑定，比如说新增了一个子节点，我们并不需要单独地为它添加一个监听事件，它所发生的事件会交给父元素中的监听函数来处理。

事件委托的优点：

1. 减少内存消耗，不必为大量元素绑定事件
2. 为动态添加的元素绑定事件

事件委托的缺点:

1. 部分事件如 focus、blur 等无冒泡机制，所以无法委托。
2. 事件委托有对子元素的查找过程，委托层级过深，可能会有性能问题
3. 频繁触发的事件如 mousemove、mouseout、mouseover等，不适合事件委托

## 如何阻止事件默认行为与阻止事件冒泡；target 和 currentTarget

```js
//阻止默认的行为，比如有href属性的a标签不会跳转，checkbox的选中不会生效等。
document.querySelector("a").addEventListener("click", function (e) {
  e.preventDefault();
});
// 停止事件传播
document.querySelector("a").addEventListener("click", function (e) {
  e.stopPropagation();
});
document.addEventListener("click", function (ev) {
  //target:  触发事件的元素。 事件代理中常用来判断时哪个子节点触发的
  console.log("ev.target:", ev.target);
  // 比如判断子节点是否有属性data-id等于1
  if (e.target.getAttribute("data-id") == "1") {
    //todo
  }
  // currentTarget: 事件本身绑定在的元素。事件代理中通常将事件绑定在父节点上
  console.log("ev.currentTarget:", ev.currentTarget);
});
```


## DOM与Node节点

属性nodeType：识别节点类型，最常见的4种如下

- 元素阶段如div span的 nodeType=1；
- 文本类容Text的 nodeType=3；
- 注释内容Comment nodeType=8；
- 文档模型Document nodeType = 9；
  查询方式：
- querySelector 返回匹配的第一个元素Element
- querySelectorAll 返回NodeList，返回的是镜像节点，即使后续节点发生变化也不会实时更新
- getElementById
- getElementsByClassName
- getElementsByTagName 返回的是匹配的元素集合HTMLCollection。
- getElementsByName 返回的NodeList

Node.contains() 返回的是一个布尔值，来表示传入的节点是否为该节点的后代节点

Element.getBoundingClientRect() 返回元素的大小及其相对于可视化窗口（视口）的位置。

Node.cloneNode() 克隆一个元素节点会拷贝它所有的属性以及属性值,当然也就包括了属性上绑定的事件(比如onclick="alert(1)"),但不会拷贝那些使用addEventListener()方法或者node.onclick = fn这种用JavaScript动态绑定的事件

Node.appendChild() 将一个节点附加到指定父节点的子节点列表的末尾处

Element.append() 方法在父节点最后一个子节点之后插入一组 Node 对象或 DOMString 对象。被插入的 DOMString 对象等价为 Text 节点.

Node.childNodes() 返回节点的子节点集合，包括元素节点、文本节点还有属性节点

Element.children() 只返回节点的元素子节点集合, 即 nodeType为1的节点。

## 其他，一些window的内容

- window对象的focus和blur事件可以监听窗口在浏览器tab页中是否聚焦，visibilitychange事件可以判断时候触发显示或隐藏
- window.devicePixelRatio 设备像素比，返回当前显示设备的物理像素分辨率与CSS 像素分辨率之比，PC上一般是1，既1px等于1物理像素，手机上可能大于2，1px会有更多的物理像素去显示
- window.matchMedia() 表示指定的媒体查询字符串是否匹配的结果。
- window.getSelection() 表示用户选择的文本范围或光标的当前位置。
- var name = prompt() 输入函数

- 无论你在HTML元素的内容中使用多少空格(包括空白字符，包括换行)，当渲染这些代码的时候，HTML解释器会将连续出现的空白字符减少为一个单独的空格符。

```html
<link rel="shortcut icon" href="favicon.ico" type="image/x-icon" />
<link rel="stylesheet" href="my.css" />
<script src="my-js-file.js"></script>
<html lang="zh-CN"></html>
<a href="mailto:nowhere@mozilla.org">向 nowhere 发邮件</a>
<blockquote cite="">块级引用</blockquote>
<q cite="">行内引用</q><cite></cite>
```

- 使用iframe时，注意安全：尽量使用https,使用sandbox属性，限制其功能

## 标签的使用

```html
1分钟刷新
<meta http-equiv="Refresh" content="60" />
5秒后跳转:比如无权限时跳转
<meta http-equiv="Refresh" content="5;URL=page2.html" />
```

```js
//定时修改document.title 可以达到网页消息提醒的目的，title标签妙用

<script type="module" >  ES6标准当成模块解析，默认阻塞同defer 即异步加载，延迟执行
<script type="module" async>  同async 异步加载，阻塞执行

<link rel="dns-prefeteh" href=""> dns 预解析DNS
//还有preconnect 预TCP链接，proload：预先http请求，prerender预渲染
```

## Location

Location下的属性：

```js
window.location.protocol = "https";
window.location.host = "127.0.0.1:5500";
window.location.hostname = "127.0.0.1";
window.location.port = "5500";
window.location.pathname = "test/path";
window.location.search = "wd=ff";
window.location.hash = "/home";
window.location.href = "http://127.0.0.1:5500/liveSeverTest.html";

window.location.reload(); //重新加载文档,参数为false或者不传，浏览器可能从缓存中读取页面。参数为true,强制从服务器重新下载文档。
window.location.replace(url); //跳转但没有历史
//hash属性可触发hashchange事件
```

js中的URL对象与之类似；也有相关属性；
encodeURI(url)和encodeURIComponent(url) 用于编码URL，变成带%的肉眼识别不了的一长串。

## Navigator

navigator.userAgent 浏览器信息（userAgent 是可以模拟，不一定可靠）

navigator.onLine 当前网络的状态

navigator.clipboard 访问剪切板（读取均为异步）

navigator.mediaDevices 媒体信息设备，可用于控制屏幕

navigator.cookieEnabled（表示当前页面是否启用了cookie）

## history

直接动作：
history.back() 向后移动一页
history.forward() 向前移动一页
history.go() 向前或者向后移动指定页数,默认参数0，会刷新当前页
history.length() 当前会话中的历史页面数
直接控制栈：
history.pushState() 向当前浏览器会话的历史堆栈中添加一个状态
history.replaceState() 修改当前历史记录状态
history.state 返回在会话栈顶的状态值的拷贝
window.onpopstate 当活动历史记录条目更改时，将触发popstate事件

## cookie 有哪些字段

Domain
Path
Expire/MaxAge
HttpOnly
Secure
SameSite

## window.requestIdleCallback

requestIdleCallback 维护一个队列，将在浏览器空闲时间内执行。
执行重计算而非紧急任务
空闲回调执行时间应该小于 50ms，最好更少
空闲回调中不要操作 DOM，因为它本来就是利用的重排重绘后的间隙空闲时间，重新操作 DOM 又会造成重排重绘

##  IntersectionObserver

创建一个新的`IntersectionObserver`对象，当其监听到目标元素的可见部分穿过了一个或多个**阈(thresholds)**时，会执行指定的回调函数。

```js
let observer = new IntersectionObserver((entries, observer) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      entry.target.classList.add('animate')
      observer.unobserve(entry.target)
    }
  })
})

document.querySelectorAll('mark').forEach(mark => {
  observer.observe(mark)
})
```

## navigator.getBattery()

获取设备电量信息

## AbortController 
取消异步信号
async function fetchWithTimeout(url, options = {}) {
  const { timeoutMS = 3000 } = options;

  return await fetch(url, {
    ...options,
    signal: AbortSignal.timeout(timeoutMS),
  });
}

## window.performance

window.performance 是一个 Web API，用于获取和分析网页的性能数据。
它提供了一系列的方法和属性，可以帮助开发者了解网页的加载和运行情况。
比如window.performance.timing里有很多时间点

## localStorage，sessionStorage，cookie 的区别

```txt
共同点：都是保存在浏览器端，同源限制。
区别：
(1) cookie数据始终在同源的http请求中携带（即使不需要），即cookie在浏览器和服务器间来回传递
    sessionStorage和localStorage不会自动把数据发给服务器，仅在本地保存
(2) 存储大小限制也不同
    cookie数据不能超过4k
    sessionStorage和localStorage 虽然也有存储大小的限制，但比cookie大得多，可以达到5M或更大
(3) 数据有效期不同
    sessionStorage：仅在当前浏览器窗口关闭前有效，自然也就不可能持久保持；
    localStorage：始终有效，窗口或浏览器关闭也一直保存，因此用作持久数据；
    cookie只在设置的cookie过期时间之前一直有效，即使窗口或浏览器关闭;
(4) 作用域不同
    sessionStorage不在不同的浏览器窗口中共享，即使是同一个页面；
    localStorage 在所有同源窗口中都是共享的；
    cookie也是在所有同源窗口中都是共享的
```
