# 面试必备


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

## defer, async 和 同步脚本执行顺序

- defer 异步加载，延迟执行。当html解析完毕后再执行，在 DOMContentLoaded 事件触发之前完成。

  多个脚本按顺序执行。

- async 异步加载，加载好，立即执行。执行仍然会阻塞文档的解析，只是它的加载过程不会阻塞。

  多个脚本的执行顺序无法保证。
- 内联脚本 同步顺序执行

同为 defer 的两个脚本，前一个没加载完，后一个会先执行吗？ 👉 两个都是外部 defer 脚本：不会，浏览器会等前一个加载完再按顺序执行；👉 一个外部 defer + 一个内联 defer：会，内联 defer 会先执行 
内联脚本加 defer，肯定优先于外部 defer 执行吗？ 👉 不是 “肯定”，是 “可能”：仅当外部 defer 脚本在 HTML 解析完后仍未加载完成时，内联 defer 才会先执行；若外部已加载完，还是按书写顺序执行
内联脚本加 async，会立刻执行吗？ 👉 会（且和不加 async 的内联脚本几乎无区别）：async 对内联脚本无实际意义，解析到就立即执行
  

## 从html到DOM

1 字节解码：按编码把字节流转为字符流
2 分词生成 Token：状态机识别标签、文本、属性等 Token
3 语法分析构建 DOM：栈结构处理嵌套，节点挂树
4 容错补全：自动修复错误、补全缺失标签，形成完整 DOM

## web性能

Web 性能 = 用户感知性能
第一优先级是
响应性（从用户操作到画面发生变化的耗时；加载速度）。
帧率（画面更新频率，60fps 为人眼舒适标准，保证动画 / 滚动流畅。）

其次是内存使用和功耗

## web体验指标

Core Web Vitals（三大核心网页指标）

1. LCP (Largest Contentful Paint) —— 最大内容绘制（加载速度）
2. INP (Interaction to Next Paint) —— 交互到下一次绘制（交互响应）
   注：24 年 3 月起取代 FID（首次输入延迟）
3. CLS (Cumulative Layout Shift) —— 累计布局偏移（视觉稳定性）

提升体验性能的方法：

1. 最小化初始加载
2. 防止内容跳转和其他重排
3. 避免字体文件延迟
4. 可交互元素始终是可交互的
5. 使任务启动器显得更具交互性（按钮缩放动画）

## 性能检测

- ligthing
- devtool中的network 和 性能监控record
- web API ： Performance.timing , PerformanceEntry ， PerformanceObserver

### 指标参数

页面完全加载时间：loadEventStart - navigationStart
dom ready = domContentLoadEventEnd - navigationStart
FMP：performance.mark("fmp")手动打点
LCP：用户交互前，可见区域内的最大块选择

- img>或者SVG 中的 image>或者video> 的 poster（封面图）
- 带 background-image 的块级元素，包含文本的块级元素（<p>, <div>, <h1> 等文本块）
  lcp动态更新：每渲染一帧，检查是否有新的更大元素出现。若有，更新 LCP 候选并记录新时间。直到页面 load 事件完成或者用户第一次滚动 / 点击 / 输入，停止更新：

// 监听 LCP 条目
const observer = new PerformanceObserver((entryList) => {
const entries = entryList.getEntries();
const lcpEntry = entries[entries.length - 1]; // 取最后一次（最终 LCP）

console.log('LCP 时间:', lcpEntry.startTime, 'ms');
console.log('LCP 元素:', lcpEntry.element); // 对应 DOM 节点
console.log('元素尺寸:', lcpEntry.size); // 面积（宽×高）
});

observer.observe({ type: 'largest-contentful-paint', buffered: true });

## 页面性能优化

1. 图片方面：

- 最优大小（图片压缩，2倍率）
- 最优格式（webp格式，svg格式
- 加载方式（图片懒加载，img添加背景
- 加载优先级（img的fetchPriority 属性

2. HTML角度
- 嵌入内容过大（响应式资源提供多种格式尺寸，懒加载，不使用iframe）
- 资源加载（rel="preload"：提前加载首屏必需资源，dns-prefetch / preconnect）
- 减小阻塞 HTML 解析与渲染：（非关键JavaScript 异步加载）

3. css角度
- 压缩、最小化 CSS 并开启 gzip
- 拆分 CSS，首屏样式内联
- 简化选择器，避免复杂嵌套与冗余匹配，删除无用 CSS，
- 用 CSS 精灵图减少图片请求
- 动画 transform、opacity、filter，不触发回流/重绘
- （video/canvas/iframe、3D 变换、will-change）GPU 合成层加速
- 字体加载显示优化
- contain属性，限定布局 / 绘制 / 样式重算范围，content-visibility: auto指定浏览器在需要之前不要布局和渲染

4. js角度
- 减小js体积：terser压缩混淆代码，gzip压缩，Tree Shaking，依赖按需引用,使用体积小的第三方依赖，设置编译目标target最小化polyfill，peerDependencies解决重复包，Code Split + 基于路由的按需加载(减小关键路径js)
- JS加载顺序： 默认阻塞 HTML 解析与渲染，非关键Js 异步加载）
- 长任务拆分，计算任务放web worker
- 使用事件委托，及时移除监听
- 减少DOM元素数量和深度，减少访问dom，避免直接修改样式，避免直接操作dom，React/vue避免重新渲染，重新计算

5. 网络方面： cdn， http2.0， 减少HTTP请求，客户端预请求，客户端离线包

6. 其他：service worker, SSR

### dom操作性能消耗的原理

浏览器的单线程包括「JS 引擎」和「渲染引擎」。操作系统切换执行会保存上下文。
操作 DOM 会触发「跨引擎桥接通信 + 上下文切换 + 渲染流水线重复执行」；
上下文切换的本质：线程切换时保存 / 恢复执行状态的系统级开销。
优化核心逻辑：减少 DOM 操作次数（批量处理）、避免跨引擎频繁通信（虚拟 DOM）、减少渲染触发（合并样式）。

## 浏览器的同源政策

协议、域名、端口号必须相同，否则则不属于同一个域。

### 网络请求跨域

> 当一个请求url的 协议、域名、端口三者之间任意一个与当前页面url不同即为跨域。

1. JSONP (JSON with Padding) 通过动态创建 script，再请求一个带参网址实现跨域通信，目前已过时。

2. CORS (跨域资源共享)
    CORS的基本思想就是使用自定义的HTTP头部让浏览器与服务器进行沟通，从而决定请求或响应是应该成功还是失败。
    简单请求：只需服务端设置 `Access-Control-Allow-Origin：*` 即可，若要带 cookie 前端设置`withCredentials`为true,后端设置`Access-Control-Allow-Credentials`为true,同时`Access-Control-Allow-Origin`不能设置为`*`
    复杂请求：使用 OPTIONS 方法发起一个预检请求到服务器，告知服务器实际请求使用方法和首部自定义字段，服务端响应返回实际请求接受的域和方法，头部。如果要携带cookie，Access-Control-Allow-Origin，Access-Control-Allow-Headers，Access-Control-Allow-Methods 均不能设置为*。
3. nginx反向代理,服务端请求不受浏览器限制，既访问同源服务端，服务端再去寻找正在的地址拿到数据再返回
4. 本地开发启动正向代理服务     
5. 跨域属于浏览器行为，可以通过浏览器设置关闭
6. 浏览器插件可以拦截请求，修改responce 的body

简单请求判定：方法是 GET/HEAD/POST + 仅含安全首部 + Content-Type 为 x-www-form-urlencoded/multipart/form-data/text/plain（三者同时满足）；
复杂请求：不满足上述任一条件（如 POST JSON、PUT/DELETE、带自定义头）；
核心差异：复杂请求多了 OPTIONS 预检步骤，服务端需额外配置允许的方法 / 首部。

### 不受跨域影响的核心场景：
静态资源加载（img/CSS/ 字体 / 视频，仅渲染，JS 无法读取内容）；
链接跳转 / 表单提交（浏览器直接处理，无 JS 读取响应）；
嵌入型资源（iframe/object，仅展示，JS 无法跨域通信）；
资源预加载（prefetch/preload，仅缓存，不读取内容）。

核心判断规则：
「JS 无法读取内容」的资源 / 操作 → 不受跨域影响；
「JS 可读取 / 修改内容」的资源 / 接口 → 受跨域限制（需 CORS/JSONP 解决）。
比如：canvas 操作跨域图片、跨域字体加载需特殊处理，但本质不是「跨域读取限制」，而是「内容访问授权」。


## 强缓存与协商缓存

### Expires 与 Cache-Control：max-age
HTTP/1.0 中的Expires 
Expires 的值是一个绝对时间的 GMT 格式的时间字符串。比如 Expires 值是：`expires:Fri, 14 Apr 2017 10:47:02 GMT`。这个时间代表这这个资源的失效时间，只要发送请求时间是在 Expires 之前，那么本地缓存始终有效，则在缓存中读取数据。

HTTP/1.1 中，Cache-Control指定 max-age——代表经过的时间后缓存过期。
同时启用的时候 Cache-Control 优先级高。

### 缓存重新验证响应 

`Etag/If-None-Match`返回的是一个校验码。`Etag`可以保证每一个资源是唯一的，资源变化都会导致`Etag`变化。服务器根据浏览器发送的`If-None-Match`值来判断是否命中缓存。

Last-Modify是一个时间标识该资源的最后修改时间，例如Last-Modify: Thu,31 Dec 2037 23:59:59 GMT。当浏览器再次请求该资源时，request的请求头中会包含If-Modify-Since，该值为缓存之前返回Last-Modify。服务器收到If-Modify-Since后，根据资源的最后修改时间判断是否命中缓存。如果命中缓存，则返回304，并且不会返回资源内容，并且不会返回Last-Modify。

`Last-Modified` 与 `ETag` 是可以一起使用的，服务器会优先验证`ETag`，一致的情况下，才会继续比对 `Last-Modified`，最后才决定是否返回 304。

### 不使用缓存 Cache-Control

no-cache 指令不会阻止响应的存储，而是阻止在没有重新验证的情况下重用响应。
no-store 响应不存储在任何缓存中
private 私有缓存

## ES6 模块与 CommonJS 模块区别

CommonJS 是运行时加载、导出值拷贝、同步、this 指向模块；
ES6 Module 是解析时加载、导出动态引用、支持异步、this 为 undefined。

## 闭包

闭包是由捆绑起来（封闭的）的函数和函数周围状态（词法环境）的引用组合而成。闭包让函数能访问它的外部作用域。在 JavaScript 中，闭包会随着函数的创建而同时创建。


闭包的优点：

- 希望一个变量长期保存内存中；
- 避免全局变量污染；
- 私有成员的存在。

闭包的缺点：

- 常驻内存，增加内存使用量；
- 使用不当造成内存泄漏。

## js 原型，原型链以及特点

```JavaScript

function Class(){
    this.name='name';
}
classA = new Class();//小写classA为Class类的实例；Class为构造函数

```

__proto__（隐式原型），非web标准却被大多数浏览器支持，指向它的构造函数的prototype。
即class.__proto__ === Class.prototype; 
prototype（显式原型）只有函数对象才有（普通对象没有，是构造函数的一个属性，存放所有实例共享的属性和方法

prototype中一般包含2个属性，一个是constructor，指向Class函数自身，用于标识原型属于谁；一个是__proto__,指向更高一级的原型对象，层层向上直到一个对象的原型对象为 null。

Object.prototype.__proto__ === null;

ES5 中Object.getPrototypeOf() 方法来获取对象的原型。

当访问一个实例对象的属性时，如果这个对象内部不存在这个属性，那么它就会去它的原型对象里找这个属性，这个原型对象又会有自己的原型，于是就这样一直找下去，也就是原型链。

特点：
JavaScript 对象是通过引用来传递的，创建的每个新对象实体中并没有一份属于自己的原型副本。当我们修改原型时，与之相关的对象也会继承这一改变。

特别的对象：Object.create(null);没有原型

### 执行上下文 调用栈，作用域

+ 执行上下文 是函数调用时创建的运行环境，每次调用都不同。比如变量对象的定义、作用域链的扩展、提供调用者的对象引用等信息。
包括this，变量环境var，词法环境（let， const），外部环境

+ 调用栈 call stack负责管理执行上下文的进出顺序。
+ 作用域： 就是变量、函数可以被访问的有效范围。
函数的词法作用域，也叫静态作用域，变量的作用域在函数创建时就已经确定了，而不是在函数调用时确定。
词法作用域保存到内部属性 [[Environment]] 中。无论函数未来在何处被调用，创建时确定，永不改变

执行上下文会引用函数的 [[Environment]] 来构建作用域链。
+ 函数的this [[thisMode]] ，当函数没有明确调用者时，规定this 指向谁。
 私有属性有三个取值。
lexical：表示从上下文中找this，这对应了箭头函数。
global：表示当this为undefined时，取全局对象，对应了普通函数。
strict：当严格模式时使用，this严格按照调用时传入的值，可能为nul或者undefined。


## Event Loop 事件循环

> 参考链接：[详解JavaScript中的Event Loop（事件循环）机制](https://zhuanlan.zhihu.com/p/33058983?utm_source=wechat_session&utm_medium=social&utm_oi=859347813597863936)

```js
微任务: Promise.then / catch / finally(不是promise，promise里是立即执行)
  async / await语法糖（本质就是 Promise 微任务） await 后面的代码全部塞进微任务队列
  MutationObserver的回调
  process.nextTick(Node.js 环境)
  queueMicrotask()手动添加微任务
宏任务: script(整体代码)  setTimeout  setInterval   I/O  setImmediate(Node.js 环境)   UI 交互事件
同一次事件循环中:  微任务永远在宏任务之前执行
```

事件循环的过程：
> 首先script脚本整体是一个大的异步任务，先执行script脚本。这个script脚本会包含同步任务和异步任务，同步任务会先在主线程上执行，异步任务（分为宏任务和微任务）会添加到任务队列中，任务队列分为宏任务队列和微任务队列。
>
> 当同步任务执行完毕后，此时的执行栈已经被清空，会去执行异步任务。此时会先从微任务队列中取一个微任务放到执行栈中执行，若有新的微任务或宏任务产生，添加到相应的任务队列中，循环往复，直至微任务队列清空。
>
> 紧接着会从宏任务队列取一个宏任务放到执行栈中执行，此时可能会产生新的微任务，将微任务放到微任务队列中，当这个宏任务执行完后会继续执行微任务队列，如果没有产生就继续执行下一个宏任务。循环往复，直至所有任务执行完毕。

```js
async function async1(){
    console.log('asy1 start');
    await async2();
    console.log("asy1 end");}
async function async2(){console.log("async2");}
async1();
setTimeout(()=>{console.log('timeout')},0);
new Promise(function(resolve){
    console.log("promise1");
    resolve();})
    .then(function(){console.log('promise2')});
console.log('script end');
//结果
//asy1 start
//async2
//promise1
//script end
//asy1 end
//promise2
//timeout
```


# 面经
## LBA

1. 如何处理前后端跨域问题,

CORS：
设置 Access-Control-Allow-Origin 响应头，可以是特定域名或通配符 \*。
如果需要支持带有自定义头部的请求，还需要设置 Access-Control-Allow-Headers。
如果需要支持 POST、PUT 等非简单请求，还需要设置 Access-Control-Allow-Methods。
代理：
通过服务转发，绕过浏览器的跨域限制

2. post预检请求每次都会发吗

不会
预检请求是 CORS 机制的一部分，用于确保跨域请求符合安全策略。
它们在非简单请求和包含自定义请求头的情况下自动发送。
预检请求帮助确保客户端和服务器之间的通信符合 CORS 规则。
成功的预检请求会被浏览器缓存，通常缓存时间为 24 小时或 Access-Control-Max-Age 指定的时间。
在缓存有效期内，相同的非简单请求将不会再次触发预检请求。

3. script标签，在html解析过程中的执行顺序

默认情况下，脚本按顺序同步执行。
async 属性 使得脚本异步加载和执行，执行顺序不确定。
defer 属性 使得脚本异步加载，但在文档解析完成，在 DOMContentLoaded 事件触发之前执行。后按顺序执行。
内联脚本 按照在文档中的顺序执行。


4. /\d{1,6}?/ ​匹配000000,结果是什么​

?非贪心匹配原则，结果是0

5. 怎么实现 excludeUndefined，使 excludeUndefined<obj['name']> 的类型是 string 而不是 string | undefined

```ts
type obj = {
  name?: string;
};
type excludeUndefined<T> = T extends undefined ? never : T;
```

6. render Check 组件， 点击 button， executed 会不会打印

```jsx
function Check() {
  const [toggle, setToggle] = useState(false);
  return (
    <div>
      <Button
        onClick={() => {
          setToggle((t) => !t);
        }}
      />
    </div>
  );
}
function Button({ onClick }) {
  console.log("executed");
  return <button onClick={onClick}>toggle</button>;
}
// 优化如下
function App() {
  const [count, setCount] = React.useState(false);
  const click = React.useCallback(() => {
    setCount((c) => !c);
  });
  const MyButton = React.memo(({ click }) => {
    console.log("executed");
    return <button onClick={click}>toggle</button>;
  });
  return (
    <div className="App">
      <MyButton onClick={click}></MyButton>
    </div>
  );
}
```

7. 怎么让外层元素，包裹两个元素，不溢出

<div>
  <div>
  xaxsaxasxsaxsaxsaxa
  </div> <div>xbasbxjksabxanbxjasnxjkaxbasbxjksabxanbxjasnxjkaxbasbxjksabxanbxjasnxjkaxbasbxjksabxanbxjasnxjkaxbasbxjksabxanbxjasnxjkaxbasbxjksabxanbxjasnxjkaxbasbxjksabxanbxjasnxjkaxbasbxjksabxanbxjasnxjka
  </div>
</div>

word-wrap: break-word;//允许长单词换行

## 贝壳

setState后发生了什么

### webpack原理，流程

初始化阶段：

初始化参数：从配置文件、 配置对象、Shell 参数中读取，与默认配置结合得出最终的参数
创建编译器对象：用上一步得到的参数创建 Compiler 对象
初始化编译环境：包括注入内置插件、注册各种模块工厂、初始化 RuleSet 集合、加载配置的插件等
开始编译：执行 compiler 对象的 run 方法
确定入口：根据配置中的 entry 找出所有的入口文件，调用 compilition.addEntry 将入口文件转换为 dependence 对象

构建阶段：

编译模块(make)：根据 entry 对应的 dependence 创建 module 对象，调用 loader 将模块转译为标准 JS 内容，调用 JS 解释器将内容转换为 AST 对象，从中找出该模块依赖的模块，再 递归 本步骤直到所有入口依赖的文件都经过了本步骤的处理

完成模块编译：上一步递归处理所有能触达到的模块后，得到了每个模块被翻译后的内容以及它们之间的 依赖关系图

生成阶段：

输出资源(seal)：根据入口和模块之间的依赖关系，组装成一个个包含多个模块的 Chunk，再把每个 Chunk 转换成一个单独的文件加入到输出列表，这步是可以修改输出内容的最后机会
写入文件系统(emitAssets)：在确定好输出内容后，根据配置确定输出的路径和文件名，把文件内容写入到文件系统

webpack优化

1. 构建用时分析
2. resolve配置
3. externals 配置
4. 缩小范围
5. noParse
6. ignorePlugin
7. 多进程配置
8. 配置缓存AST

### sass编译css过程

1. 解析 Less 文件
   词法分析: 将 Less 代码拆分成一个个有意义的词法单元（token），如变量名、属性名、函数名等。
   语法分析: 根据 Less 语法规则，将词法单元组合成语法树（抽象语法树 AST）。AST 是对 Less 代码结构的一种表示，方便后续的处理。
2. 变量替换
   遍历 AST，查找所有变量引用。
   根据变量定义，将变量引用替换为对应的值。
   处理变量嵌套和递归引用。
3. 混合（Mixin）展开
   找到所有混合调用。
   根据混合定义，将混合的内容插入到调用处。
   处理混合的参数传递和默认值。
4. 嵌套规则展开
   将嵌套的规则展开为扁平的规则集。
   处理嵌套选择器和继承。
5. 计算表达式
   遍历 AST，查找所有的表达式（如颜色计算、单位转换等）。
   使用 JavaScript 引擎计算表达式的值。
   将计算结果替换回表达式所在的位置。
6. 生成 CSS
   根据处理后的 AST，生成符合 CSS 语法的字符串。
   处理 CSS 的输出格式，如缩进、注释等。
7. 输出 CSS 文件
   将生成的 CSS 字符串写入到指定的文件中。

### fiber原理
   在 React 16 之前，React 的协调过程是不可中断的，如果组件树非常庞大，更新过程可能会导致页面卡顿。而 Fiber 的引入，使得 React 的协调过程变得可中断和可优先级。

Fiber 的主要作用
可中断的渲染: Fiber 将渲染工作拆分成更小的单元，每个单元称为 Fiber。这些 Fiber 可以被中断，让出主线程，从而避免长时间的渲染阻塞。
优先级调度: Fiber 引入了优先级概念，不同的更新可以有不同的优先级。高优先级的更新会被优先处理，保证用户交互的流畅性。
异步渲染: Fiber 使得渲染过程可以异步进行，从而避免主线程阻塞，提升用户体验。
更好的错误边界: Fiber 改善了错误处理机制，使得错误可以被更精准地捕获和隔离，防止整个应用崩溃。
支持渐进式渲染: Fiber 支持渐进式渲染，可以先渲染重要的部分，然后再渲染不太重要的部分，提升首屏加载速度。

Fiber 的工作原理
Fiber 节点: Fiber 节点是 React 的一个内部数据结构，它代表了虚拟 DOM 中的一个元素。
Fiber 树: React 会将虚拟 DOM 转换成 Fiber 树，Fiber 树中的每个节点都包含了该节点的各种信息，如类型、属性、子节点等。
双缓存: Fiber 采用双缓存机制，在内存中维护两棵 Fiber 树，一棵是当前正在使用的树，另一棵是正在构建的新树。
工作循环: React 通过一个工作循环来遍历 Fiber 树，进行 diff 和更新。这个工作循环是可以被中断的，当主线程空闲时，React 会继续执行工作循环。

### React diff算法
### hooks性能优化

1. 使用 useMemo 和 useCallback
2. 有条件地渲染组件
3. 使用 useRef ，避免在每次渲染时创建新的对象
4. 批量更新状态，可以减少不必要的渲染。
5. 使用 React.memo 和 shouldComponentUpdate
6. 使用 useEffect 控制副作用
7. 使用 useContext
8. 代码分割和懒加载 lazy(()=>import())
9. 减少渲染层级

## DD

### 返回值

typeof Array.prototype //object
typeof Function.prototype //function
typeof Object.prototype //object

### 任务顺序

process.nextTick() 最优先执行的，当前轮次的事件循环结束之前被调用。在当前同步代码执行完成后立即执行的。
promise.then() 在所有同步代码和 process.nextTick() 执行完后，但在宏任务之前执行
setImmediate() 红任务，在timeOut之前
setTimeout()

### 异步顺序

```js
function async1(){
  console.log(async1 start!)
  await new Promise(resolve=>{
    console.log("promise!")
  })
  console.log("promise success")
  return "async1 end"
}
console.log("script start!");
async1().then(res=>{
  console.log(res)
})
console.log("script end!");
```

### 状态码

206 Partial Content
200 from cache
304 not modify

### 写findIndex

```js
Array.prototype.findIndex = function (predicate) {
  "use strict";
  if (this == null) {
    throw new TypeError(
      "Array.prototype.findIndex called on null or undefined",
    );
  }
  if (typeof predicate !== "function") {
    throw new TypeError("predicate must be a function");
  }
  var list = Object(this);
  var length = list.length >>> 0;
  var thisArg = arguments[1];
  var value;

  for (var i = 0; i < length; i++) {
    value = list[i];
    if (predicate.call(thisArg, value, i, list)) {
      return i;
    }
  }
  return -1;
};
```

## 其他

React lazy 是怎么实现得

面对大流量项目，前端的容灾措施

监控线上性能指标

倒计数组件如何保持准确

