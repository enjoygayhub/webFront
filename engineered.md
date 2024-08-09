# 工程化engineered
## 页面性能提升
1. 减小下载文件大小：Code Split + 基于路由的按需加载，Tree Shaking,使用体积小的第三方依赖，依赖按需引用，terse压缩混淆代码，gzip压缩
2. 图片方面：图片压缩，图片懒加载，base64格式，svg格式，css Sprites，iconFont，
3. 网络方面：prefetch cdn
4. 缓存方面：缓存http请求
5. 代码方面：减少HTTP资源请求次数，减少DOM元素数量和深度，避免各种形式重排重绘，避免直接修改样式，避免直接操作dom，React/vue避免重新渲染，重新计算
6. 其他：service worker, SSR，web worker

## js 的几种设计模式
  1. 装饰器模式用于扩展对象的功能，而无需修改现有的类或构造函数。此模式可用于将特征添加到对象中，而无需修改底层的代码。
  2. 工厂模式使用工厂方法创建对象而不需要指定具体的类或构造函数的模式。根据特定条件生成不同的对象时，可以使用此模式。
  3. 单例模式一个单例对象是只能实例化一次的对象。如果不存在，则单例模式将创建类的新实例。如果存在实例，则仅返回对该对象的引用。重复调用构造函数将始终获取同一对象。 
  4. 模块模式。模块独立的代码，通过为变量创建单独的作用域来避免命名空间污染。
  5. ES6模块，ES6的模块是以文件形式存储的。每个文件只能有一个模块。默认情况下，模块内的所有内容都是私有的。通过使用 export 关键字来暴露函数、变量和类。模块内的代码始终在严格模式下运行。

## js 的几种模块规范

```txt
js 中现在比较成熟的有四种模块加载方案。
第一种是 CommonJS 方案，它通过 require 来引入模块，通过 module.exports 定义模块的输出接口。这种模块加载方案是服务器端的解决方案，它是以同步的方式来引入模块的，因为在服务端文件都存储在本地磁盘，所以读取非常快，所以以同步的方式加载没有问题。但如果是在浏览器端，由于模块的加载是使用网络请求，因此使用异步加载的方式更加合适。

第二种是 AMD 方案，这种方案采用异步加载的方式来加载模块，模块的加载不影响后面语句的执行，所有依赖这个模块的语句都定义在一个回调函数里，等到加载完成后再执行回调函数。require.js 实现了 AMD 规范。

第三种是 CMD 方案，这种方案和 AMD 方案都是为了解决异步模块加载的问题，sea.js 实现了 CMD 规范。它和 require.js的区别在于模块定义时对依赖的处理不同和对依赖模块的执行时机的处理不同。

第四种方案是 ES6 提出的方案，使用 import 和 export 的形式来导入导出模块
```

## ES6 模块与 CommonJS 模块区别

> CommonJS 模块输出的是一个值的拷贝，ES6 模块输出的是值的引用。
CommonJS 模块输出的是值的拷贝，也就是说，一旦输出一个值，模块内部的变化就影响不到这个值。
ES6 模块是对脚本静态分析的时候，遇到模块加载命令 import，就会生成一个只读引用。等到脚本真正执行时，再根据这个只读引用，到被加载的那个模块里面去取值。

> CommonJS 模块是运行时加载，ES6 模块是编译时输出接口。CommonJS 模块就是对象，即在输入时是先加载整个模块，生成一个对象，然后再从这个对象上面读取方法，这种加载称为“运行时加载”。而 ES6 模块不是对象，它的对外接口只是一种静态定义，在代码静态解析阶段就会生成。


## 进程与线程

```txt
进程是CPU进行资源分配的基本单位
线程是操作系统进行任务调度和执行的基本单位

简单理解：

1.计算机的核心是CPU，它承担了所有的计算任务。它就像一座工厂，时刻在运行。
2.假定工厂的电力有限，一次只能供给一个车间使用。也就是说，一个车间开工的时候，其他车间都必须停工。就是，单个CPU一次只能运行一个任务。
3.进程就好比工厂的车间，它代表CPU所能处理的单个任务。任一时刻，CPU总是运行一个进程，其他进程处于非运行状态。
4.一个车间里，可以有很多工人。他们协同完成一个任务。
5.线程就好比车间里的工人。一个进程可以包括多个线程。
6.车间的空间是工人们共享的，比如许多房间是每个工人都可以进出的。这象征一个进程的内存空间是共享的，每个线程都可以使用这些共享内存。
7.可是，每间房间的大小不同，有些房间最多只能容纳一个人，比如厕所。里面有人的时候，其他人就不能进去了。
  这代表一个线程使用某些共享内存时，其他线程必须等它结束，才能使用这一块内存。
8.一个防止他人进入的简单方法，就是门口加一把锁。先到的人锁上门，后到的人看到上锁，就在门口排队，等锁打开再进去。
  这就叫"互斥锁"（Mutual exclusion，缩写 Mutex），防止多个线程同时读写某一块内存区域。
9.还有些房间，可以同时容纳n个人，比如厨房。也就是说，如果人数大于n，多出来的人只能在外面等着。这好比某些内存区域，只能供给固定数目的线程使用。
10.这时的解决方法，就是在门口挂n把钥匙。进去的人就取一把钥匙，出来时再把钥匙挂回原处。后到的人发现钥匙架空了，就知道必须在门口排队等着了。
  这种做法叫做"信号量"（Semaphore），用来保证多个线程不会互相冲突。

```

堆空间:
堆是进程中的动态内存区域，用于存储动态分配的对象和数组。
堆空间由所有线程共享，即所有线程都可以访问同一个堆空间中的对象。
堆空间的分配和回收通常是由运行时环境或垃圾回收器管理的。
栈空间:
栈是线程中的私有内存区域，用于存储局部变量和函数调用的信息（如函数参数、局部变量和返回地址）。
每个线程有自己的栈空间，因此多个线程之间不会相互影响栈空间。
栈空间的分配和回收通常是在函数调用和返回时自动完成的。


一个进程通常有一个堆空间，这个堆空间被该进程中的所有线程共享。
每个线程有一个私有的栈空间，用于存储该线程的局部变量和函数调用栈。

### JS 是一个单线程的语言，它这个特点有什么好处吗，为什么要设计成单线程的？

单线程模型使得语言的设计和实现更加简单直观。
保证数据一致性，开发者不需要考虑复杂的并发控制机制，如锁、信号量等。
通过异步编程模型和事件循环机制，能够高效地处理 I/O 密集型任务，如网络请求和用户输入事件。
原因：确保浏览器主线程始终可以处理用户界面的更新。

### 浏览器环境下， JS 执行在哪个进程哪个线程

浏览器环境中，JavaScript 代码主要在渲染进程的主线程上执行。

浏览器进程 (Browser Process):
负责管理浏览器的用户界面、书签、历史记录等非特定于网页的功能。
通常只运行一个实例。
渲染进程 (Renderer Processes):
每个打开的标签页或窗口通常都有一个独立的渲染进程。
渲染进程负责渲染网页、执行 JavaScript 代码等。
GPU 进程 (GPU Process):负责图形渲染任务。
插件进程 (Plugin Processes):用于处理 Flash 等插件。

## 编译步骤

即时编译是 V8 引擎的一项关键技术，它允许引擎在运行时将 JavaScript 代码编译成本地机器码，从而显著提高执行效率。以下是即时编译过程的大致步骤：

解析:
V8 引擎首先解析 JavaScript 代码，生成抽象语法树 (Abstract Syntax Tree, AST)。
词法分析:
引擎进行词法分析，生成中间表示 (Intermediate Representation, IR)。
优化:
V8 引擎会对 IR 进行优化，包括常量折叠、死代码消除等优化技术。
编译:
优化后的 IR 被编译成本地机器码。这个过程由 V8 的即时编译器完成。
执行:
编译后的本地机器码可以直接在目标平台上执行。


### Source Map

编译过程中，生成的产物代码中被转换的部分与源代码中相应部分的映射关系表

## 服务器类型

网络服务器（Web server）包括硬件(电脑)，软件（HTTP服务）；
静态网络服务器：返回的文件是托管文件的原样；
动态网络服务器：普通是包括静态的应用服务器和数据库；例如将数据库内容填充到HTML模板生成浏览器所收到的文档内容,早期前后端不分离的项目就是这也。

## peerDependency 是为了解决什么问题

peerDependencies 是为了解决依赖冲突的问题，重复安装。在 npm 中，如果一个包 A 依赖于包 B，那么包 A 的用户在安装包 A 时，npm 会自动安装包 B。但是，如果包 A 和包 B 的版本不兼容，就会导致依赖冲突。

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

## webpack 的构建优化

1. 持久化缓存: cache，缓存AST
2. 多进程: thread-loader
3. 更快的 loader: swc

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
   import LeftLogo from '!../../images/svgIcons/chevron-left.svg';
   ```

2. `-!`禁用`pre-loader`和 `normal-loader`

3. `!!` 禁用`pre-loader`、`normal-loader`、`post-loader`,只能行内处理

   ```js
   const req = require.context('!!svg-sprite-loader!!../../images/svgIcons', false, /.svg$/)
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
3. 使用 webpack-bundle-analyzer 分析打包体积，替换占用较大体积的库，如 moment -> dayjs
4. 使用支持 Tree-Shaking 的库，对无引用的库或函数进行删除，如 lodash -> lodash/es
5. 对无法 Tree Shaking 的库，进行按需引入模块，如使用 import Button from 'antd/lib/Button'，此处可手写 babel-plugin 自动完成，但不推荐
6. 使用 babel (css 为 postcss) 时采用 browserlist，越先进的浏览器所需要的 polyfill 越少，体积更小
7. code spliting，路由懒加载，只加载当前路由的包，按需加载其余的 chunk，首页 JS 体积变小 (不减小总体积，但减小首页体积)
8. 使用 webpack 的 splitChunksPlugin，把运行时、被引用多次的库进行分包，在分包时要注意避免某一个库被多次引用多次打包。此时分为多个 chunk，虽不能把总体积变小，但可提高加载性能 

目前前端工程化中使用 terser 和 swc 进行 JS 代码压缩。
通过 AST 分析，根据选项配置一些策略，来生成一颗更小体积的 AST 并生成代码。

1. 去除多余字符: 空格，换行及注释
2. 压缩变量名：替换变量名，函数名及属性名
3. 解析程序逻辑: 编译预计算

## window.performance

window.performance 是一个 Web API，用于获取和分析网页的性能数据。
它提供了一系列的方法和属性，可以帮助开发者了解网页的加载和运行情况。
比如window.performance.timing里有很多时间点

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

## semver
语义化版本号。版本格式：主版本号.次版本号.修订号

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

## Core Web Vitals（核心网页指标）是什么？

1. 最大内容绘制 (Largest Contentful Paint, LCP):
2. 首次输入延迟 (First Input Delay, FID):
3. 累积布局偏移 (Cumulative Layout Shift, CLS):

### 如何确认你们项目是否依赖某一个依赖项
yarn list
npm list --depth=0

### 如何检测出你们安装的依赖是否安全
npm audit/yarn audit

### dom操作性能消耗的原理

渲染引擎与js引擎是互斥的单线程，操作系统切换线程执行会保存上下文，直接操作dom是便会带来性能损耗

### core-js 是做什么用的
polyfill垫片

### git hooks 原理是什么
git 允许在各种操作之前添加一些 hook 脚本，如未正常运行则 git 操作不通过。
例如：precommit，prepush

###  eslint 的作用
eslint，对代码不仅有风格的校验，更有可读性、安全性、健壮性的校验。

### pnpm 有什么作用
它解决了 npm/yarn 平铺 node_modules 带来的依赖项重复的问题 (doppelgangers)

### browserslist作用

打包到所支持的浏览器的所需最小内容

### 如何分包
webpack中提供了方案4.0之前是：CommonsChunkPlugin 4.0后是optimization.splitChunks
vite中配置build.rollupOptions.output.manualChunks