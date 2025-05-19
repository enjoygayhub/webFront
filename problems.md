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
    setTimeout(() => lock.release(), minutes * 60 * 1000);
  });
}
···

### URL.parse() 替代 new URL()
  new URL() When parameter can’t parse it throws an error.


###  css columns

[css columns](https://juejin.cn/post/7295057643608899621) 设置columns：3;或者columns: 100px;实现分栏布局。配和scroll-snap可以实现swiper吸附效果


## textarea 随内容自动增高，而不是固定高度，出现滚动条
```css
    textarea {
        form-sizing: normal;
    }
``` 

## blog
[数据结构数组与类数组](http://www.360doc.com/content/18/0925/05/3175779_789416619.shtml)
[浮动元素的特点](https://juejin.cn/post/6844903891155288072)
[虚拟列表](https://blog.csdn.net/weixin_39932611/article/details/110746868?utm_medium=distribute.pc_relevant.none-task-blog-baidujs_baidulandingword-1&spm=1001.2101.3001.4242)
[跨域错误捕获](https://www.jianshu.com/p/315ffe6797b8)
##  [重排与重绘，提升性能。](http://www.ruanyifeng.com/blog/2015/09/web-page-performance-in-depth.html)

### build后分包的js文件内报错，react 模块是undefined，
  分包规则错误，触发循环引用了，导致报错。

### useEffect方法内访问到state的旧值
  如果依赖项数组为空（[]），useEffect 只会在组件挂载时运行一次，永远只会访问最初的state值；
  如果依赖性不为空，但在useEffect方法内访问到的是旧值，可能是因为useEffect方法是异步执行的，
  解决：用useRef方法，将state的值存储到ref中，然后在useEffect方法内访问ref.current即可。