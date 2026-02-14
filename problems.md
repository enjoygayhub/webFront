# 记录项目中遇到的问题与解决

### 重写video标签自带的快进快退功能

    [掘金](https://juejin.cn/post/7277029942297853989)

### El-menu 随着路由切换，菜单每次都是重新展开，或者展开的项错误。

    default-active需为计算属性;
    设置 :default-active="activeMenu" router=true

    虽然不知道原理是啥，把vue-router中配置路由createRouter参数里的routes里的name属性去掉就正常了。

### map中循环发布网络请求会bug

一次性堆积太多请求，使用for循环加await才能达到阻塞效果

### puppeteer 启动的浏览器，仍然被发现不是真实环境，原因：navigator.webdriver 值 为true，

    在原型上将其删除navigator.__proto__ .webdriver

### structuredClone() 结构化克隆深拷贝，兼容问题报错

### 页面公告要展示文本换行，可以将div改成textarea，disbale掉编辑

### navigator.wakeLock 可以设定屏幕保持唤醒状态

    ···
    function tryKeepScreenAlive(minutes) {

navigator.wakeLock.request("screen").then(lock => {
setTimeout(() => lock.release(), minutes _ 60 _ 1000);
});
}
···

### URL.parse() 替代 new URL()

new URL() When parameter can’t parse it throws an error.

### css columns

[css columns](https://juejin.cn/post/7295057643608899621) 设置columns：3;或者columns: 100px;实现分栏布局。配和scroll-snap可以实现swiper吸附效果

## textarea 随内容自动增高，而不是固定高度，出现滚动条

```css
textarea {
  field-sizing: normal;
}
```

## blog

[数据结构数组与类数组](http://www.360doc.com/content/18/0925/05/3175779_789416619.shtml)
[浮动元素的特点](https://juejin.cn/post/6844903891155288072)
[虚拟列表](https://blog.csdn.net/weixin_39932611/article/details/110746868?utm_medium=distribute.pc_relevant.none-task-blog-baidujs_baidulandingword-1&spm=1001.2101.3001.4242)
[跨域错误捕获](https://www.jianshu.com/p/315ffe6797b8)

## [重排与重绘，提升性能。](http://www.ruanyifeng.com/blog/2015/09/web-page-performance-in-depth.html)

### build后分包的js文件内报错，react 模块是undefined，

分包规则错误，触发循环引用了，导致报错。

### useEffect方法内访问到state的旧值

如果依赖项数组为空（[]），useEffect 只会在组件挂载时运行一次，永远只会访问最初的state值；
如果依赖性不为空，但在useEffect方法内访问到的是旧值，可能是因为useEffect方法是异步执行的，
解决：用useRef方法，将state的值存储到ref中，然后在useEffect方法内访问ref.current即可。

### JSON.stringify 会忽略函数，因为 JSON 不支持函数类型。

如果需要保留函数，需通过 replacer 手动处理（如转为字符串）。

直接序列化函数（如 JSON.stringify(fn)）会返回 undefined

### node_modules/bin中放着可执行的脚本

终端命令ls -l 可查看脚本链接的实际地址。
开启javascript调试终端，可以调试命令汗后执行的代码

### flex 布局，容器设置flex：flex-start，子元素设置margin-left，子元素会被推到容器右侧

### 组件库二次封装导致按需引入失效

例如

```js
import { Alert } from "antd";
Alert.a = "a"; // 核心问题：修改了导入的对象
export default Alert;
```

核心原因：「副作用（Side Effect）」导致 Tree Shaking 失效
Alert.a = 'a' 属于「修改对象属性」的操作，是明显的副作用，打包工具无法确定这个修改是否会影响全局（比如其他地方是否依赖 Alert.a），因此即使没人导入，也会被保留；

使用/_#**PURE**_/ 只能保证函数无副作用，但不能改变赋值操作
Alert.a = 'a';// 这行依然是副作用

解决方法一：
使用函数包裹，只有在调用时才会执行赋值操作。然后在函数前添加/_#**PURE**_/ 注释

```js
import { Alert } from "antd";

function setAlertA() {
  Alert.a = "a"; // 核心问题：修改了导入的对象
}
const MyAlert = /*#__PURE__*/ setAlertA();
export default MyAlert;
```

方案二：在package.json中添加sideEffects字段，告诉打包工具哪些文件有副作用，哪些文件无副作用。
方案三：在使用项目的时候 rollup配置treeShake.moduleSideEffects: false
