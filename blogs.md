## [图片加载优化方案](https://blog.csdn.net/qq_33539213/article/details/106189209)

当图片太多时：可以将图片服务与应用服务分离；图片服务器采用独立域名 ,css、js和图片就可以并发请求了

借助第三方软件来进行压缩。

图片懒加载：当图片滚动到视野中（滚动高度+窗口高度>图片位置）时，用data-src标签里的地址替换src地址**

css Sprites

将图片压缩成base64格式来节约请求

##  [重排与重绘，提升性能。](http://www.ruanyifeng.com/blog/2015/09/web-page-performance-in-depth.html)

 [defer与async](https://segmentfault.com/a/1190000006778717?utm_source=sf-related)

+ async 无序异步解析，会阻塞

+ defer 异步延迟

[事件代理](https://davidwalsh.name/event-delegate)

[浮点数的存贮表示](https://zhuanlan.zhihu.com/p/75581822)

[cookies,session](https://cnblogs.com/moyand/p/9047978.html)

[跨域错误捕获](https://www.jianshu.com/p/315ffe6797b8)


[三次握手和4次挥手详解](https://www.zhihu.com/search?type=content&q=%E8%AF%B4%E4%B8%80%E8%AF%B4TCP%E7%9A%84%E4%B8%89%E6%AC%A1%E6%8F%A1%E6%89%8B)

[cdn加速](https://zhuanlan.zhihu.com/p/82793949)  ： 最近结点内容分发

[输入url后发生了什么](https://segmentfault.com/a/1190000014872028) 

1. 逐级寻找缓存对应ip地址,dns服务器找

2. 建立tcp连接，请求资源，服务端判断缓存时间，可选择返回304

3. 下载网页，渲染：生成DOM树，遇到js文件会解释执行阻塞渲染过程，图片和css异步加载，css规则树与dom合并成render渲染树，布局绘制。

## [页面性能提升](https://www.jianshu.com/p/d9c20eafa67e)

   1. 减少HTTP资源请求次数,使用可缓存的AJAX：使用静态资源CDN来存储文件：推荐使用异步JavaScript资源;消除阻塞渲染的CSS及JavaScript：
   2. 减少DOM元素数量和深度：减少使用关系型样式表的写法：
   3. 合理利用浏览器缓存：图片懒加载：使用iconFont代替图片图标：
   4. 尽量使用id;合理缓存DOM对象：
   5. 避免各种形式重排重绘：


## [数据结构数组与类数组](http://www.360doc.com/content/18/0925/05/3175779_789416619.shtml)

[浮动元素的特点](https://juejin.cn/post/6844903891155288072)

[外边距溢出问题](https://juejin.cn/post/6844904033509965831) 

[边距折叠问题](https://juejin.cn/post/6844903497045917710)

## js渲染10w数据优化

[虚拟列表](https://blog.csdn.net/weixin_39932611/article/details/110746868?utm_medium=distribute.pc_relevant.none-task-blog-baidujs_baidulandingword-1&spm=1001.2101.3001.4242)

## dom操作性能消耗的原理

渲染引擎与js引擎互斥的单线程，操作系统切换线程执行会保存上下文，直接操作dom是便会带来性能损耗

