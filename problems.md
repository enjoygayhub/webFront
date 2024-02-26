# 记录项目中遇到的问题与解决

### 重写video标签自带的快进快退功能 
    [掘金](https://juejin.cn/post/7277029942297853989)

### El-menu 随着路由切换，菜单每次都是重新展开，或者展开的项错误。
    default-active需为计算属性;
    设置 :default-active="activeMenu" router=true
    
    虽然不知道原理是啥，把vue-router中配置路由createRouter参数里的routes里的name属性去掉就正常了。

### map中循环发布网络请求会bug，一次性堆积太多请求，使用for循环加await才能达到阻塞效果

### puppeteer 启动的浏览器，仍然被发现不是真实环境，原因：navigator.webdriver 值 为true，在原型上将其删除navigator.__proto__ .webdriver

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