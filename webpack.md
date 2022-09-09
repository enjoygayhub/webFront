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

Plugins（插件）可以用于执行范围更广的任务，包括打包、优化、压缩、搭建服务器等等，要使用一个插件，
一般是先使用 npm 包管理器进行安装，然后在配置文件中引入，最后将其实例化后传递给 plugins 数组属性。
```

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

   

