# 工程化engineered

## 准备

+ package.json  包含基础配置文件

+ npm 或者yarn 包管理工具

+ 确定技术栈 src目录入口源码文件

+ 构建工具 webpack等，构建webpack配置，针对开发/生产环境不同配置

+ 安装配置Loader， plugin，gitignore，readme等

  

## 热更新

+ 自动刷新整个页面
+ 页面整体无刷新而只更新部分

### 热更新方案：

1. watch 模式
2. live Reload
3. hot module replacement，热替换：loader中实现了相关api，开发中通过websocket通知更改

大概流程是我们用webpack-dev-server启动一个服务之后，浏览器和服务端是通过websocket进行长连接，webpack内部实现的watch就会监听文件修改，只要有修改就webpack会重新打包编译到内存中，然后webpack-dev-server依赖中间件webpack-dev-middleware和webpack之间进行交互，每次热更新都会请求一个携带hash值的json文件和一个js，websocker传递的也是hash值，内部机制通过hash值检查进行热更新

## Source Map

编译过程中，生成的产物代码中被转换的部分与源代码中相应部分的映射关系表

