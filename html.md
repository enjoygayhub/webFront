# HTML

- [HTML](#html)
  - [HTML5 的新特性](#html5-的新特性)
  - [localStorage，sessionStorage，cookie 的区别](#localstoragesessionstoragecookie-的区别)
  - [DOCTYPE的作用是什么](#doctype的作用是什么)
  - [语义化标签的作用](#语义化标签的作用)
  - [行内元素 块级元素](#行内元素-块级元素)
  - [canvas与SVG](#canvas与svg)

## HTML5 的新特性

1. 语义化标签:header、footer、section、nav、aside、article

2. 增强型表单：input的多个type(color, date, datetime, email, month, number, range, search, tel, time, url, week)

3. 新增表单元素：

   - datalist	定义选项列表。请与 input 元素配合使用该元素，来定义 input 可能的值。
   - keygen 规定用于表单的密钥对生成器字段。
   - output 定义不同类型的输出，比如脚本的输出。

4. 新增表单属性：placehoder, required, min和max, step, height 和 width, autofocus, multiple

5. 音频视频：

   - audio 定义音频内容
   - video	定义视频（video 或者 movie）
   - source	定义多媒体资源 video 和 audio
   - embed	定义嵌入的内容，比如插件。
   - track	为诸如 video 和 audio 元素之类的媒介规定外部文本轨道。

6. canvas：位图

7. 地理定位：

8. 拖拽：drag

9. 本地存储：localStorage、sessionStorage

10. 新事件：onresize、ondrag、onscroll、onmousewheel、onerror、onplay、onpause

11. WebSocket

12. 移除元素
acronym，applet，basefont ，big ，center，dir，font，frame，frameset，noframes，strike，tt

## localStorage，sessionStorage，cookie 的区别

```js
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

## DOCTYPE的作用是什么

```js
(1) <!DOCTYPE>声明一般位于第一行，处于 <html> 标签前。作用：告诉浏览器以什么样的模式来解析文档。DOCTYPE 不存在或格式不正确会导致文档以兼容模式呈现。
(2) 一般指定了之后会以标准模式来进行文档解析，否则就以兼容模式进行解析。在标准模式下，浏览器的解析规则都是按照最新的标准进行解析的;在兼容模式下，浏览器会以向后兼容的方式来模拟老式浏览器的行为，以保证一些老的网站的正确访问。
```

## 语义化标签的作用

- 去掉或样式丢失的时候能让页面呈现清晰的结构
- 方便其他设备解析（如屏幕阅读器、盲人阅读器、移动设备）以意义的方式来渲染网页
- 有利于SEO
- 便于团队开发和维护，遵循W3C标准，可以减少差异化

## 行内元素 块级元素

```js
常见的行内元素有 a b span img strong sub sup button input label select textarea
常见的块级元素有  div p ul ol li dl dt dd h1 h2 h3 h4 h5 h6
```

## canvas与SVG

- canvas与svg都是可以在浏览器上创建图形
- HTML5 的 canvas 元素使用 JavaScript 在网页上绘制图像。画布是一个矩形区域，您可以控制其每一像素。canvas 拥有多种绘制路径、矩形、圆形、字符以及添加图像、动画的方法
- canvas绘制位图,绘制出来的每一个图形的元素都是独立的DOM节点，能够方便的绑定事件或用来修改。canvas复杂度高会减慢渲染速度。canvas输出的是一整幅画布，就像一张图片一样，放大会失真。canvas不适合游戏应用。
- svg输出的图形是矢量图形，后期可以修改参数来自由放大缩小，SVG 图像在放大或改变尺寸的情况下其图形质量不会有所损失。svg最适合图像密集型的游戏，其中的许多对象会被频繁重绘

## 事件对象中的clientX offsetX screenX pageX的区别

- [clientX, clientY]

```txt
client直译就是客户端，客户端的窗口就是指游览器的显示页面内容的窗口大小（不包含工具栏、导航栏等等）
[clientX, clientY]就是鼠标距游览器显示窗口的长度

兼容性：IE和主流游览器都支持。
```

- [offsetX, offsetY]

```txt
offset意为偏移量
[offsetX, offsetY]是被点击的元素距左上角为参考原点的长度，而IE、FF和Chrome的参考点有所差异。

Chrome下，offsetX offsetY是包含边框的
IE、FF是不包含边框的，如果鼠标进入到border区域，为返回负值

兼容性：IE9+,chrome,FF都支持此属性。
```

- [screenX, screenY]

```txt
screen顾名思义是屏幕
[screenX, screenY]是用来获取鼠标点击位置到屏幕显示器的距离，距离的最大值需根据屏幕分辨率的尺寸来计算。

兼容性：所有游览器都支持此属性。
```

- [pageX, pageY]

```txt
page为页面的意思，页面的高度一般情况client浏览器显示区域装不下，所以会出现垂直滚动条。

[pageX, pageY]是鼠标距离页面初始page原点的长度。

在IE中没有pageX、pageY取而代之的是event.x、event.y。x和y在webkit内核下也实现了，所以火狐不支持x，y。

兼容性：IE不支持，其他高级游览器支持。
```

## 事件模型以及三种事件绑定方法

现代浏览器事件模型二个过程：捕获；冒泡
事件捕获阶段。捕获指的是事件从html根元素向下传播到实际点击的目标元素（指在dom文档树中向下层），依次检查经过的节点是否绑定了该事件监听函数，如果有且该事件是设定在捕获阶段执行，便执行。

冒泡阶段：捕获阶段结束后，反向再检查一次，冒泡回根元素

现代浏览器中，默认情况下，所有事件处理程序都在冒泡阶段执行。

事件绑定方法：

+ 行内事件，过时了，不建议使用,缺点行为与结构不分离

```html
<button onclick="alert('ok')">Press me</button> 
```

+ js中在元素节点上添加事件属性，缺点：重复定义后者会覆盖前者

  ```js
   document.querySelector('button').onclick=alert('ok')
  ```

+ addEventListener（eventName，function，useCaptrue），常用，可取消

  ```js
  const btn = document.querySelector('button');
  
  function bgChange() {
    const rndCol = 'rgb(' + random(255) + ',' + random(255) + ',' + random(255) + ')';
    document.body.style.backgroundColor = rndCol;
  }
  
  btn.addEventListener('click', bgChange);
  btn.removeEventListener('click', bgChange);
  ```

 addEventListener，其中第三个参数可以指定事件是否在捕获阶段执行。默认是false，在冒泡阶段执行。

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

## 如何阻止事件冒泡

```js
// w3c
e.stopPropagation()
// IE
e.cancelBubble = true
```

## 如何阻止事件默认行为

```js
//谷歌及IE8以上
e.preventDefault();
//IE8及以下
window.event.returnValue = false;
//无兼容问题（但不能用于节点直接onclick绑定函数）
return false;
```

## load 和 DOMContentLoaded 的区别（从没遇到过考这个）

- 当整个页面及所有依赖资源如样式表和图片都已完成加载时，将触发load事件。它与DOMContentLoaded不同，后者只要页面DOM加载完成就触发，无需等待依赖资源的加载。
- 当纯HTML被完全加载以及解析时，DOMContentLoaded 事件会被触发，而不必等待样式表，图片或者子框架完成加载。

## js判断图片是否加载完毕的方式（从没遇到过考这个）

- load事件

> 所有浏览器都支持img的load事件。

- readystatechange事件

> readyState属性为complete和loaded则表明图片已经加载完毕。测试IE6-IE10支持该事件，其它浏览器不支持。

- img 的 complete属性

> 轮询不断监测img的complete属性，如果为true则表明图片已经加载完毕，停止轮询。该属性所有浏览器都支持。



## js 延迟加载的方式有哪些

> js 延迟加载，也就是等页面加载完成之后再加载 JavaScript 文件。 js 延迟加载有助于提高页面加载速度。

一般有以下几种方式：

- defer 属性
- async 属性
- 动态创建 DOM 方式
- 使用 setTimeout 延迟方法
- 让 JS 最后加载

##  defer 和 async 的区别

- defer 属性表示延迟执行引入的 JavaScript，即这段 JavaScript 加载时 HTML 并未停止解析，这两个过程是并行的。当整个 document 解析完毕后再执行脚本文件，在 DOMContentLoaded 事件触发之前完成。

  多个脚本按顺序执行。

- async 属性表示异步执行引入的 JavaScript，与 defer 的区别在于，如果已经加载好，就会开始执行，也就是说它的执行仍然会阻塞文档的解析，只是它的加载过程不会阻塞。

  多个脚本的执行顺序无法保证。

## 其他

+ 所有的文件夹名和文件都使用小写字母，且没有空格,建议使用中划线作为分割

+ google font 字体，可以通过link载入；line-height = 1；可设置为数字，效果为数字乘以字体大小

+ var vaible = document.querySelector();vaible.getAttribute();.setAttribute()  设置属性

+ var name = prompt() 输入函数，localStorage.setItem();localStorage.getItem() 本地存储

+ 无论你在HTML元素的内容中使用多少空格(包括空白字符，包括换行)，当渲染这些代码的时候，HTML解释器会将连续出现的空白字符减少为一个单独的空格符。

+ ```html
  <link rel="shortcut icon" href="favicon.ico" type="image/x-icon">
  <link rel="stylesheet" href="my.css">
  <script src="my-js-file.js"></script>
  <html lang="zh-CN"></html>
  <a href="mailto:nowhere@mozilla.org">向 nowhere 发邮件</a>
  <blockquote cite = ''>块级引用</blockquote> <q cite = ''>行内引用</q><cite></cite>
  ```

使用iframe时，注意安全：

- 尽量使用https
- 使用sandbox属性，限制其功能