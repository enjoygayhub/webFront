#  js

###  [重排与重绘，提升性能。](http://www.ruanyifeng.com/blog/2015/09/web-page-performance-in-depth.html)

 [defer与async](https://segmentfault.com/a/1190000006778717?utm_source=sf-related)

+ async 无序异步解析，会阻塞

+ defer 异步延迟

[事件代理](https://davidwalsh.name/event-delegate)

[浮点数的存贮表示](https://zhuanlan.zhihu.com/p/75581822)

[cookies,session](https://cnblogs.com/moyand/p/9047978.html)

[跨域错误捕获](https://www.jianshu.com/p/315ffe6797b8)

## 变量提升：

+ 函数声明优先与变量声明，函数声明可被覆盖
+ 匿名函数不提升，不继承Function原型

[三次握手和4次挥手详解](https://www.zhihu.com/search?type=content&q=%E8%AF%B4%E4%B8%80%E8%AF%B4TCP%E7%9A%84%E4%B8%89%E6%AC%A1%E6%8F%A1%E6%89%8B)

[cdn加速](https://zhuanlan.zhihu.com/p/82793949)  ： 最近结点内容分发

[输入url后发生了什么](https://segmentfault.com/a/1190000014872028) 

1. 逐级寻找缓存对应ip地址,dns服务器找

2. 建立tcp连接，请求资源，服务端判断缓存时间，可选择返回304

3. 下载网页，渲染：生成DOM树，遇到js文件会解释执行阻塞渲染过程，图片和css异步加载，css规则树与dom合并成render渲染树，布局绘制。

## [页面性能提升](https://www.jianshu.com/p/d9c20eafa67e)

   1. 减少HTTP资源请求次数,使用可缓存的AJAX：使用静态资源CDN来存储文件：推荐使用异步JavaScript资源;消除阻塞渲染的CSS及JavaScript：
   2. 减少DOM元素数量和深度：减少使用关系型样式表的写法：
   3. 合理利用浏览器缓存：图片懒加载：使用iconfont代替图片图标：
   4. 尽量使用id;合理缓存DOM对象：
   5. 避免各种形式重排重绘：
# [响应式布局](https://juejin.im/post/5caaa230e51d452b672f9703)

- 设置viewport
- 媒体查询
- 字体的适配（字体单位）
- 百分比布局
- 图片的适配（图片的响应式）
- 结合flex，grid，BFC，栅格系统等已经成型的方案
# es6中类的继承
class B extends A {};
B.__proto__ ===A;
B.prototype.__proto__ === A.prototype;

## [数据结构数组与类数组](http://www.360doc.com/content/18/0925/05/3175779_789416619.shtml)

# Css

[浮动元素的特点](https://juejin.cn/post/6844903891155288072)

[外边距溢出问题](https://juejin.cn/post/6844904033509965831) [边距折叠问题](https://juejin.cn/post/6844903497045917710)

## js渲染10w数据优化

[虚拟列表](https://blog.csdn.net/weixin_39932611/article/details/110746868?utm_medium=distribute.pc_relevant.none-task-blog-baidujs_baidulandingword-1&spm=1001.2101.3001.4242)

