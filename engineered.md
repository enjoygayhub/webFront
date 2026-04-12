# 工程化engineered

# webpack

## webpack 的构建流程

初始化: 启动构建，读取webpack.config.js与合并配置参数，加载 Plugin，实例化 Compiler。
compile: 从 Entry 出发，编译。
转换模块：
1. resolve 解析路径
根据 resolve 配置找文件真实路径
2. 加载模块内容（readFile）
读取文件源代码字符串
3. 调用 Loader 链进行编译，从右往左、从下往上执行
4. 解析依赖，代码生成 AST，遍历并收集所有依赖路径，存入依赖列表
5. 递归所有抵赖，重复转换
编译完成seal：代码优化，生成chunk和资源
输出emit: 根据output配置确定的路径与文件名，把文件写入到文件系统。

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


### 如何分包

webpackoptimization.splitChunks
vite中配置build.rollupOptions.output.manualChunks

### 为啥能实现按需引用

库采用 ESM 规范开发，每个组件独立拆分、独立导出，import/export 是编译时（静态）分析。Tree Shaking 是实现按需引入的核心，基于静态分析，剔除打包产物中「未被引用的死代码」。配合sideEffects标识副作用，。


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


## js 的几种模块规范

```txt

第一种是 CommonJS 方案，它通过 require 来引入模块，通过 module.exports 定义模块的输出接口。这种模块加载方案是服务端的解决方案， Node.js 就是基于 CommonJS 规范的。

第二种是 AMD 方案

第三种是 CMD 方案（AMD 和 CMD 过时，基本没见过）

第四种是 ES6 Modules，使用 import 和 export 的形式来导入导出模块

```
