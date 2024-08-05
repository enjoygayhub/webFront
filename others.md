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
  	polygon( [<fill-rule>,]? [<shape-arg> <shape-arg>] )
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
浏览器解析 HTML 资源的过程是一个复杂而有序的过程，涉及到了多个阶段，包括解析 HTML 文档、下载外部资源、构建渲染树、布局和绘制等。下面我们将详细介绍这一过程，并解释浏览器如何处理 CSS 资源、JavaScript 资源以及图片等外部资源。

### 1. 解析 HTML 文档
当浏览器接收到 HTML 文档时，它会开始解析文档。解析过程包括：
- **标记化**:
  - 将 HTML 文本转换为标记流。
  - 这一步骤将 HTML 文本解析成一系列的标记，如开始标签、结束标签、文本节点等。

- **构建 DOM 树**:
  - 根据标记流构建文档对象模型 (DOM) 树。
  - DOM 树是一个树状结构，表示文档的结构和内容。

- **解析样式表**:
  - 当浏览器遇到 `<link>` 或 `<style>` 标签时，它会下载并解析样式表，然后将样式规则应用到 DOM 树中的元素上。

### 2. 下载外部资源
- **CSS 资源**:
  - 当浏览器遇到 `<link rel="stylesheet">` 或 `<style>` 标签时，它会下载 CSS 文件。
  - CSS 文件被解析，并应用到相应的 DOM 元素上。
  - 浏览器通常会暂停 HTML 的解析，直到 CSS 文件完全下载并解析完成。

- **JavaScript 资源**:
  - 当浏览器遇到 `<script>` 标签时，它会下载 JavaScript 文件。
  - 默认情况下，浏览器会暂停 HTML 的解析，直到 JavaScript 文件完全下载并执行完成。
  - 通过设置 `async` 或 `defer` 属性，可以使 JavaScript 文件异步加载，避免阻塞 HTML 的解析。

- **图片资源**:
  - 当浏览器遇到 `<img>` 或 `<source>` 标签时，它会下载图片资源。
  - 图片资源的下载通常是异步的，不会阻塞 HTML 的解析。

### 3. 构建渲染树
- **渲染树**:
  - 渲染树是由 DOM 节点和样式信息组成的树形结构。
  - 浏览器将 DOM 树中的元素与样式表中的规则相结合，构建出渲染树。
  - 渲染树表示了最终要渲染到屏幕上的元素及其样式信息。

### 4. 布局
- **布局**:
  - 浏览器根据渲染树计算每个元素的位置和尺寸。
  - 这一步骤决定了元素在页面上的确切位置。

### 5. 绘制
- **绘制**:
  - 最后，浏览器将渲染树中的元素绘制到屏幕上。
  - 绘制是将每个元素的实际像素绘制到屏幕上。

### 处理外部资源的注意事项
- **CSS 和 JavaScript 的加载顺序**:
  - CSS 和 JavaScript 的加载顺序会影响页面的渲染和交互。
  - 将 CSS 放在 `<head>` 中，JavaScript 放在文档末尾可以优化页面加载性能。

- **异步加载**:
  - 使用 `async` 和 `defer` 属性可以让 JavaScript 异步加载，避免阻塞页面渲染。
  - 图片也可以通过懒加载（lazy loading）技术异步加载。

- **资源优先级**:
  - 浏览器通常会优先加载对页面渲染至关重要的资源，如关键路径 CSS。
  - 使用 `preload` 和 `prefetch` 可以提前加载重要的资源。

### 总结
- **HTML 解析**:
  - 浏览器解析 HTML 文档，构建 DOM 树，并解析样式表。

- **资源下载**:
  - 浏览器下载 CSS、JavaScript 和图片资源。
  - CSS 和 JavaScript 的加载顺序会影响页面渲染。

- **渲染和绘制**:
  - 浏览器构建渲染树，计算布局，并最终将页面绘制到屏幕上。

如果您有其他具体的需求或应用场景，请告诉我！

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