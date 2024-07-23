# 一些学习笔记

## 一些简写

+ font ：style| weight |size/line-height |family ;字号字体必有，字体必须最后

+ text-shadow: offset-x | offset-y |? blur-radius | color ;

+ background:
  - `<attachment>` : scroll [|](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Value_definition_syntax#single_bar) fixed [|](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Value_definition_syntax#single_bar) local
  
  - `<box>`:  border-box [|](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Value_definition_syntax#single_bar) padding-box [|](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Value_definition_syntax#single_bar) content-box | text
  
  - `<background-color>`
  
  - `<bg-image>`: none|url()| linear-gradient(to-bottom,rgba(),rgba())
  
  - `<position>`:left [|](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Value_definition_syntax#single_bar) center [|](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Value_definition_syntax#single_bar) right [|](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Value_definition_syntax#single_bar) top [|](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Value_definition_syntax#single_bar) bottom | length-percentage
  
  - `<bg-size>` : cover | contain | length| percentage
  
  - `<repeat-style>`: repeat [|](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Value_definition_syntax#single_bar) space [|](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Value_definition_syntax#single_bar) round [|](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Value_definition_syntax#single_bar) no-repeat
  
+ clip-path: 裁剪盒子显示; shape-outside: 指定使用下面列表的值来定义浮动元素的浮动区域。这个浮动区域决定了行内内容所包裹的形状。

  - 插入长方形，前面1-4个参数控制上右下左的距离，round后参数控制圆角
    
    ```css
    inset( <shape-arg>{1,4} [round <border-radius>]? )
    ```
    
  - 圆形，参数为半径，圆心
  
      ```css
      circle( [<shape-radius>]? [at <position>]? )
      ```
  
  - 椭圆，参数为两轴半径，圆心位置
  
      ```css
      ellipse( [<shape-radius>{2}]? [at <position>]? )
      ```
  
  - 多边形，参数为顶点位置
  	
  	```css
  	polygon( [<fill-rule>,]? [<shape-arg> <shape-arg>]
  	```
  	
  - path()
  
    ```css
    path( [<fill-rule>,]? <string>)
    ```

## 修改滚动条

```css
scroll::-webkit-scrollbar {
width: 10px;
height: 10px;
}
/*正常情况下滑块的样式*/
.scroll::-webkit-scrollbar-thumb {
background-color: rgba(0,0,0,.05);
border-radius: 10px;
-webkit-box-shadow: inset1px1px0rgba(0,0,0,.1);
}
/*鼠标悬浮在该类指向的控件上时滑块的样式*/
.scroll:hover::-webkit-scrollbar-thumb {
}
/*鼠标悬浮在滑块上时滑块的样式*/
.scroll::-webkit-scrollbar-thumb:hover {
}
/*正常时候的主干部分*/
.scroll::-webkit-scrollbar-track {
border-radius: 10px;
-webkit-box-shadow: inset006pxrgba(0,0,0,0);
background-color: white;
}
/*鼠标悬浮在滚动条上的主干部分*/
.scroll::-webkit-scrollbar-track:hover {
}
```

###  IntersectionObserver

创建一个新的`IntersectionObserver`对象，当其监听到目标元素的可见部分穿过了一个或多个**阈(thresholds)**时，会执行指定的回调函数。

```js
let observer = new IntersectionObserver((entries, observer) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      entry.target.classList.add('animate')
      observer.unobserve(entry.target)
    }
  })
})

document.querySelectorAll('mark').forEach(mark => {
  observer.observe(mark)
})
```

## 标签

```html
1分钟刷新
<meta http-equiv="Refresh" content="60">
5秒后跳转:比如无权限时跳转
<meta http-equiv="Refresh" content="5;URL=page2.html">

```

```js
//定时修改document.title 可以达到网页消息提醒的目的，title标签妙用

<script type="module" >  ES6标准当成模块解析，默认阻塞同defer 即异步加载，延迟执行
<script type="module" async>  同async 异步加载，阻塞执行

<link rel="dns-prefeteh" href=""> dns 预解析DNS
//还有preconnect 预TCP链接，proload：预先http请求，prerender预渲染
```


## 从html到DOM

1. 字节流解码到字符流数据
2. 字符预处理
3. 令牌化，将字符数据转化成令牌 (状态机
4. 构建DOM树，由令牌到DOM节点
5. 将CSSOM与DOM合并得到渲染树，忽略掉不渲染的标签
6. 布局，计算盒子模型大小位置
7. 绘制，遍历布局树，生成图层，合成页面


## 构建普通ts脚本项目

1. 新建项目文件夹，vscode打开文件夹,
2. 唤起cmd终端，输入git init 初始化git，
3. 输入npm init -y 初始化包管理工具，按提示输入项目信息,
4. 输入npm install typescript --save -d ：安装ts在开发环境，
5. 输入tsc init 初始化ts配置，修改tsconfig.json 相关配置，如配置rootDir,outDir等
6. 输入tsc 可编译ts为js,
7. 添加gitignore，readme等文件,

## 编码知识
UTF-16对于码点小于0xFFFF的用2个字节表示，大于0xFFFF的编码用四个字节表示。

UTF-16编码大于0xFFFF（四个字节）过程：
1. 码位减去 0x10000，得到的值的范围为20比特长的 0...0xFFFFF，不足的话，前面补0。
2. 高位的10比特的值加上 0xD800 得到第一个码元，
3. 低位的10比特的值加上 0xDC00 得到第二个码元，
4. 将两个组合，得到UTF-16编码

UTF-8 是它是一种变长的编码方式, 使用的字节个数从 1 到 4 个不等，UTF-8的编码规则：

1. 对于只有一个字节的符号，字节的第一位设为0，后面 7 位为这个符号的 Unicode 码。此时，对于英语字母UTF-8 编码和 ASCII 码是相同的。
2. 对于 n 字节的符号（n > 1），左侧第一个字节的前 n 位都设为 1，第 n + 1 位设为0，后面所有字节的前两位一律设为 10
utf-8转Base64编码规则

1. 获取每个字符的Unicode码，转为utf-8编码
2. 三个字节作为一组，一共是24个二进制位
字节数不能被 3 整除，用0字节值在末尾补足
3. 按照6个比特位一组分组，前两位补0，凑齐8位
4. 计算每个分组的数值
5. 以值作为索引，去ASCII码表找对应的值，00000000为 ’=‘

utf-8转%编码
● 获取utf-8编码后的二进制
● 8位一组，转为16进制的值
● %拼接