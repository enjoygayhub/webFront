# 工程化engineered

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
- 减小js体积：Code Split + 基于路由的按需加载，Tree Shaking+依赖按需引用,使用体积小的第三方依赖，terser压缩混淆代码，gzip压缩
- JS 默认阻塞 HTML 解析与渲染，非关键Js 异步加载）
- 长任务拆分，计算任务放web worker
- 使用事件委托，及时移除监听
- 减少DOM元素数量和深度，减少访问dom，避免直接修改样式，避免直接操作dom，React/vue避免重新渲染，重新计算

5. 网络方面： cdn， http2.0， 减少HTTP请求，客户端预请求，客户端离线包

6. 其他：service worker, SSR

## ES6 模块与 CommonJS 模块区别

CommonJS 是运行时加载、导出值拷贝、同步、this 指向模块；
ES6 Module 是解析时加载、导出动态引用、支持异步、this 为 undefined。

# webpack

## webpack 的功能

```txt
webpack 的主要原理:
它将所有的资源都看成是一个模块，并且把页面逻辑当作一个整体，通过一个给定的入口文件，webpack 从这个文件开始，找到所有的依赖文件，将各个依赖文件模块通过 loader 和 plugins 处理后，然后打包在一起，最后输出一个浏览器可识别的 JS 文件。

Webpack 具有四个核心的概念，分别是 Entry（入口）、Output（输出）、loader（加载器） 和 Plugins（插件）。
Entry 是 webpack 的入口起点，它指示 webpack 应该从哪个模块开始着手，来作为其构建内部依赖图的开始。
Output 属性告诉 webpack 在哪里输出它所创建的打包文件，也可指定打包文件的名称，默认位置为 ./dist。
loader 可以理解为 webpack 的编译器，它使得 webpack 可以处理一些非 JavaScript 文件。
在对 loader 进行配置的时候：
  1. test 属性，标志有哪些后缀的文件应该被处理，是一个正则表达式。
  2. use 属性，指定 test 类型的文件应该使用哪个 loader 进行预处理。
常用的 loader 有 css-loader、style-loader 等。

Plugins（插件）可以用于执行范围更广的任务，包括打包、优化、压缩、搭建服务器等等，要使用一个插件，一般是先使用 npm 包管理器进行安装，然后在配置文件中引入，最后将其实例化后传递给 plugins 数组属性。
```

## webpack 的构建流程

初始化: 启动构建，读取webpack.config.js与合并配置参数，加载 Plugin，实例化 Compiler。
编译: 从 Entry 出发，递归地找到所有依赖的模块,针对每个 Module 串行调用对应的 Loader 去翻译文件的内容。
转换模块：Loader 将模块代码转换为抽象语法树（AST），以便进行后续的处理。
生成依赖图：根据模块之间的引用关系，建立一个依赖图，优化，如去除重复依赖、按需加载等
确定 Chunk: 每个 Module根据依赖关系，生成代码块(Chunk)。
生成 Bundle: 每个 Chunk 都转换成一个单独的文件加入到输出列表，这步是可以修改输出内容的最后机会。
输出完成: 确定好输出内容后，根据配置确定的路径与文件名，把文件写入到文件系统。

## webpack 常用插件

- html-webpack-plugin

> 用于生成一个html文件，并将最终生成的js，css以及一些静态资源文件以script和link的形式动态插入其中

- webpack-dev-server

  > 用于实时的打包编译，打包好的 main.js 是托管到了内存中，所以在项目根目录中看不到，但是我们可以认为在项目的根目录中，有一个看不见的 main.js

- CommonsChunkPlugin

  > 主要是用来提取第三方库（如jQuery）和公共模块(公共js，css都可以)，常用于多页面应用程序，生成公共 chunk，避免重复引用。

## webpack添加多个loader

1. use中使用数组，包含多个loader，或者对象 use: ['loader3', 'loader2', 'loader1']
2. rules中添加多个规则

`loader`的顺序: 从右到左, 从下到上：且都会执行

通过配置不同的参数改变`loader`的执行顺序，`pre` 前面的， `post`在后面的， `normal`正常

```js
{
    test: /\.js$/,
    use: ['loader1'],
    enforce: "pre"
},
{
    test: /\.js$/,
    use: ['loader2']
},
{
    test: /\.js$/,
    use: ['loader3'],
    enforce: "post"
},
```

```
loader带参数执行的顺序: pre -> normal -> inline -> post
```

引用时可以通过特殊符号禁用loader或选择loader

1. `!`禁用`normal-loader`

   ```js
   import LeftLogo from "!../../images/svgIcons/chevron-left.svg";
   ```

2. `-!`禁用`pre-loader`和 `normal-loader`

3. `!!` 禁用`pre-loader`、`normal-loader`、`post-loader`,只能行内处理

   ```js
   const req = require.context(
     "!!svg-sprite-loader!!../../images/svgIcons",
     false,
     /.svg$/,
   );
   ```

## webpack 是怎么处理 commonjs/esm

Webpack 通过解析 require 调用来处理 CommonJS 模块，将模块打包成 IIFE（立即执行函数表达式）
Webpack 通过解析 import 和 export 语句来处理 ES 模块，并支持 Tree Shaking 和动态导入按需加载，可以用于代码分割，提高性能。

## 打包器(webpack/rollup) 如何加载 css 样式资源

Webpack 主要通过 css-loader 和 style-loader 这两个 Loader 来处理 CSS 文件。

css-loader:将 CSS 文件中的 @import 和 url() 等语句解析成 JavaScript模块。
style-loader:将 CSS 代码注入到 HTML 的 <style> 标签中，从而让浏览器能够直接解析和应用样式。
或者借助于 mini-css-extract-plugin 将 CSS 单独抽离出来。

rollup通过rollup-plugin-postcss:
将 CSS 文件处理成 JavaScript 模块，并支持 PostCSS。
将 CSS 提取成独立的文件，或者内联到 JavaScript 中。

# vite

## vite构建流程、

启动开发服务器: Vite 启动一个开发服务器，监听文件变化。
浏览器请求模块: 浏览器请求模块时，Vite 会根据请求路径找到对应的模块。
模块转换:1原生 ESM 模块: 直接返回模块代码。2需要转换的模块: 使用 esbuild 进行快速转换，生成 ES 模块。
返回模块给浏览器: Vite 将转换后的模块代码返回给浏览器，浏览器加载并执行。
HMR: 当文件发生变化时，Vite 会重新构建模块，并通过 WebSocket 通知浏览器更新相应的模块。

## hmr热更新

模块映射:
Vite 服务器在开发模式下将模块直接映射到浏览器。
每个模块都由一个唯一的 URL 映射，例如 /@fs/path/to/module.js。

模块更新:
当文件发生变化时，Vite 服务器会检测到文件的变化，并重新编译受影响的模块。
Vite 服务器会通过websocket发送更新信号给浏览器，通知浏览器哪些模块已经发生变化。

模块替换:
浏览器接收到更新信号后，会请求新的模块文件。
新的模块文件会替换旧的模块文件，而不会刷新整个页面。
浏览器会重新执行已更改的模块，并更新相关状态。

## js的代码压缩

1. terser 或者 uglify，及流行的使用 Rust 编写的 swc 压缩混淆化 JS。
2. gzip 或者 brotli 压缩，在网关处(nginx)开启
3. 使用打包工具分析产物体积，替换占用较大体积的库，如 moment -> dayjs
4. 使用支持 Tree-Shaking 的库，对无引用的库或函数进行删除，如 lodash -> lodash/es
5. 对无法 Tree Shaking 的库，进行按需引入模块，如使用 import Button from 'antd/lib/Button'，
6. 使用 babel (css 为 postcss) 时采用 browserlist，越先进的浏览器所需要的 polyfill 越少，体积更小
7. code spliting，路由懒加载，只加载当前路由的包，按需加载其余的 chunk，首页 JS 体积变小 (不减小总体积，但减小首页体积)

8. 去除多余字符: 空格，换行及注释
9. 压缩变量名：替换变量名，函数名及属性名
10. 解析程序逻辑: 编译预计算

## 代码还原sourmap

快速还原 / 定位：用在线工具（Source Map Explorer）或浏览器开发者工具，开箱即用；
工程化 / 批量还原：用 source-map 库编程式解析，提取原始位置或生成原始文件；
核心前提：Source Map 文件完整（尤其是 sourcesContent 字段），且与压缩代码的映射关系未被破坏。

## 什么是 AST，及其应用

AST 是 Abstract Syntax Tree 的简称抽象语法树
将 Typescript 转化为 Javascript (typescript)
将 SASS/LESS 转化为 CSS (sass/less)
将 ES6+ 转化为 ES5 (babel)
将 Javascript 代码进行格式化 (eslint/prettier)
识别 React 项目中的 JSX (babel)
GraphQL、MDX、Vue SFC 等等

而在语言转换的过程中，实质上就是对其 AST 的操作，核心步骤就是 AST 三步走
Code -> AST (Parse)
AST -> AST (Transform)
AST -> Code (Generate)

AST 的生成这一步骤被称为解析(Parser)，而该步骤也有两个阶段: 词法分析(Lexical Analysis)和语法分析(Syntactic Analysis)

### dom操作性能消耗的原理

浏览器的核心执行逻辑是「JS 引擎」和「渲染引擎」共享同一个主线程，且二者互斥。操作系统切换线程执行会保存上下文，直接操作dom会带来性能损耗。
操作 DOM 会触发「跨引擎桥接 + 上下文切换 + 渲染流水线重复执行」；
上下文切换的本质：线程切换时保存 / 恢复执行状态的系统级开销，DOM 操作越频繁，切换损耗越明显；
优化核心逻辑：减少 DOM 操作次数（批量处理）、避免跨引擎频繁通信（虚拟 DOM）、减少渲染触发（合并样式）。

### 如何分包

webpackoptimization.splitChunks
vite中配置build.rollupOptions.output.manualChunks

### 为啥能实现按需引用

库采用 ESM 规范开发，每个组件独立拆分、独立导出，import/export 是编译时（静态）分析。Tree Shaking 是实现按需引入的核心，基于静态分析，剔除打包产物中「未被引用的死代码」。配合sideEffects标识副作用，。

## JWT（JSON Web Token）的原理

JWT 是一种用于在不同服务之间传递安全可靠信息的紧凑且自包含的方式。它本质上是一个 JSON 对象，经过 Base64 编码并使用数字签名。
一个标准的 JWT 包含三个部分，用点（.）分隔：

头部（Header）：
指定了 JWT 的类型（通常为 JWT）和使用的签名算法（例如 HMAC SHA256）。

载荷（Payload）：
标识该 JWT 的唯一 id，创建时间，过期时间，以及其他自定义数据。
这些声明一般不加密，但被 Base64 URL 编码。

签名（Signature）：
使用头部和载荷的内容，结合一个密钥，通过指定的签名算法生成。用于验证 JWT 的完整性。

## 什么是服务器渲染 (SSR)

服务器渲染 (SSR)：将同一个组件渲染为服务器端的 HTML 字符串，将它们直接发送到浏览器，最后将这些静态标记"激活"为客户端上完全可交互的应用程序。

服务器处理请求：服务器接收到请求后，执行相应的逻辑，获取数据，并使用模板引擎将数据渲染成完整的 HTML。返回给浏览器

优点
首屏加载速度快： 浏览器可以直接渲染完整的 HTML，无需等待 JavaScript 执行，提升用户体验。
SEO 友好： 搜索引擎可以抓取到完整的 HTML，有利于 SEO。
更好的初始交互： 用户在页面加载完成前就可以进行一些简单的交互。

服务器渲染的缺点
服务器负载较大： 服务器需要承担更多的计算任务，消耗更多的资源。
开发复杂度高： 实现 SSR 需要掌握更多的技术，开发成本较高。
不利于单页面应用： SSR 不适合高度动态化的单页面应用。

## peerDependency 是为了解决什么问题

peerDependencies 是为了解决依赖冲突的问题，重复安装。在 npm 中，如果一个包 A 依赖于包 B，那么包 A 的用户在安装包 A 时，npm 会自动安装包 B。但是，如果包 A 和包 B 的版本不兼容，就会导致依赖冲突。

## js 的几种设计模式

1. 装饰器模式用于扩展对象的功能，而无需修改现有的类或构造函数。此模式可用于将特征添加到对象中，而无需修改底层的代码。
2. 工厂模式使用工厂方法创建对象而不需要指定具体的类或构造函数的模式。根据特定条件生成不同的对象时，可以使用此模式。
3. 单例模式一个单例对象是只能实例化一次的对象。如果不存在，则单例模式将创建类的新实例。如果存在实例，则仅返回对该对象的引用。重复调用构造函数将始终获取同一对象。
4. 模块模式。模块独立的代码，通过为变量创建单独的作用域来避免命名空间污染。
5. ES6模块，ES6的模块是以文件形式存储的。每个文件只能有一个模块。默认情况下，模块内的所有内容都是私有的。通过使用 export 关键字来暴露函数、变量和类。模块内的代码始终在严格模式下运行。

## js 的几种模块规范

```txt

第一种是 CommonJS 方案，它通过 require 来引入模块，通过 module.exports 定义模块的输出接口。这种模块加载方案是服务端的解决方案， Node.js 就是基于 CommonJS 规范的。

第二种是 AMD 方案

第三种是 CMD 方案（AMD 和 CMD 过时，基本没见过）

第四种是 ES6 Modules，使用 import 和 export 的形式来导入导出模块

```
