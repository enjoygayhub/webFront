# CSS

## CSS3 新特性

1. 伪类和伪元素选择器：
   > :first-child, :last-child, :nth-child(1), :link, :visited, :hover, :active
   > ::before, ::after, ::first-letter, ::first-line, ::selection
2. 背景、边框和颜色透明度：
    > background-size, background-origin, border-radius
    > box-shadow, border-image
    > rgba
3. 文字效果：
    > text-shadow
    word-wrap: break-word;//允许长单词换行
    
4. 2D 转换和 3D 转换：
   > transform, translate(), rotate(), scale(), skew(), matrix()
   > rotateX(), rotateY(), perspective
5. 动画、过渡：animation, transition
6. 多列：column-count, column-gap, column-rule
7. 用户界面：resize, box-sizing, outline-offset

## CSS 盒模型

盒模型总共包括4个部分：

- content：内容，容纳着元素的”真实“内容，例如文本
- padding：内边距
- border：边框
- margin：外边距

两种盒模型的区别：

- `box-sizing: content-box`  (默认)
  
  > 通过CSS样式设置的width的大小只是content的大小
- `box-sizing: border-box`
  
  > 通过CSS样式设置的width的大小是content + padding + border的和

## 设置一个元素的背景颜色，背景颜色会填充哪些区域

与background-clip属性有关:
padding-box: 背景颜色填充内容区和内边距区域（默认）。
border-box: 背景颜色填充内容区、内边距区域和边框区域。
content-box: 背景颜色仅填充内容区。

## margin/padding 设置百分比是相对谁的

总结：margin/padding设置百分比**都是**相对于**父盒子的宽度**(width属性)来计算量的
如果没有明确的父元素，或者父元素的宽度为 auto，则相对于视口的宽度。

## CSS 选择器优先级

选择器按优先级先后排列：!important>内联>id>class = 属性 = 伪类 >标签 = 伪元素 > 通配符 \*

- important 声明 1,0,0,0
- ID 选择器 0,1,0,0
- 类选择器 0,0,1,0
- 伪类选择器 0,0,1,0
- 属性选择器 0,0,1,0
- 标签选择器 0,0,0,1
- 伪元素选择器 0,0,0,1
- 通配符选择器 0,0,0,0

## 常用css选择器

| 选择器                                                       | 示例                | 学习CSS的教程                                                |
| :----------------------------------------------------------- | :------------------ | :----------------------------------------------------------- |
| [类型选择器](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Type_selectors) | `h1 { }`            | [类型选择器](https://developer.mozilla.org/zh-CN/docs/user:chrisdavidmills/CSS_Learn/CSS_Selectors/Type_Class_and_ID_Selectors#Type_selectors) |
| [通配选择器](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Universal_selectors) | `* { }`             | [通配选择器](https://developer.mozilla.org/zh-CN/docs/user:chrisdavidmills/CSS_Learn/CSS_Selectors/Type_Class_and_ID_Selectors#The_universal_selector) |
| [类选择器](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Class_selectors) | `.box { }`          | [类选择器](https://developer.mozilla.org/zh-CN/docs/user:chrisdavidmills/CSS_Learn/CSS_Selectors/Type_Class_and_ID_Selectors#Class_selectors) |
| [ID选择器](https://developer.mozilla.org/zh-CN/docs/Web/CSS/ID_selectors) | `#unique { }`       | [ID选择器](https://developer.mozilla.org/zh-CN/docs/user:chrisdavidmills/CSS_Learn/CSS_Selectors/Type_Class_and_ID_Selectors#ID_Selectors) |
| [标签属性选择器](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Attribute_selectors) | `a[title] { }`      | [标签属性选择器](https://developer.mozilla.org/zh-CN/docs/User:chrisdavidmills/CSS_Learn/CSS_Selectors/Attribute_selectors) |
| [伪类选择器](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Pseudo-classes) | `p:first-child { }` | [伪类](https://developer.mozilla.org/zh-CN/docs/User:chrisdavidmills/CSS_Learn/CSS_Selectors/Pseuso-classes_and_Pseudo-elements#What_is_a_pseudo-class) |
| [伪元素选择器](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Pseudo-elements) | `p::first-line { }` | [伪元素](https://developer.mozilla.org/zh-CN/docs/User:chrisdavidmills/CSS_Learn/CSS_Selectors/Pseuso-classes_and_Pseudo-elements#What_is_a_pseudo-element) |
| [后代选择器](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Descendant_combinator) | `article p`         | [后代运算符](https://developer.mozilla.org/zh-CN/docs/User:chrisdavidmills/CSS_Learn/CSS_Selectors/Combinators#Descendant_Selector) |
| [子代选择器](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Child_combinator) | `article > p`       | [子代选择器](https://developer.mozilla.org/zh-CN/docs/User:chrisdavidmills/CSS_Learn/CSS_Selectors/Combinators#Child_combinator) |
| [相邻兄弟选择器](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Adjacent_sibling_combinator) | `h1 + p`            | [相邻兄弟](https://developer.mozilla.org/zh-CN/docs/User:chrisdavidmills/CSS_Learn/CSS_Selectors/Combinators#Adjacent_sibling) |
| [通用兄弟选择器](https://developer.mozilla.org/zh-CN/docs/Web/CSS/General_sibling_combinator) | `h1 ~ p`            | [通用兄弟](https://developer.mozilla.org/zh-CN/docs/User:chrisdavidmills/CSS_Learn/CSS_Selectors/Combinators#General_sibling) |

## 伪类与伪元素的区别

```js
一般在css3中使用单冒号来表示伪类，用双冒号来表示伪元素。

伪类一般匹配的是元素的一些特殊状态，如hover、link ，active等，
而伪元素一般匹配的特殊的位置，比如after、before等。 伪元素默认行内元素，css中其必须有content属性，就算是content：""也必须写，否则无效。
伪元素可以用于添加文字，或者小图标。

css引入伪类和伪元素概念是为了格式化文档树以外的信息。修饰不在文档树中的部分，比如，一句话中的第一个字母，或者是列表中的第一个元素。

```

## CSS 中哪些属性可以继承

```css
每一个属性在定义中都给出了这个属性是否具有继承性，一个具有继承性的属性会在没有指定值的时候，会使用父元素的同属性的值来作为自己的值。

一般具有继承性的属性有，字体相关的属性，font-size和font-weight等。文本相关的属性，color和text-align,
表格的一些布局属性、列表属性如list-style等。还有光标属性cursor、元素可见性visibility。

当一个属性不是继承属性的时候，通过将它的值设置为inherit来使它从父元素那获取同名的属性值来继承。
```

## BFC的概念, 哪些元素可以触发BFC

> BFC 即 Block Formatting Context (块格式化上下文)， 是Web页面的可视化CSS渲染的一部分，是块盒子的布局过程发生的区域

简单来说就是一个封闭的黑盒子，里面元素的布局不会影响外部。

## 脱离文档流的方式

- float
- position: absolute
- position: fixed

## position 的值定位

```css
absolute
生成绝对定位的元素，相对于值不为static的第一个父元素的padding box进行定位，也可以理解为离自己这一级元素最近的一级position设置为absolute或者relative的父元素的padding box的左上角为原点的。

fixed
生成绝对定位的元素，相对于浏览器窗口进行定位。

relative
生成相对定位的元素，相对于其元素本身所在正常位置static进行定位。

static
默认值。没有定位，元素出现在正常的流中（忽略top,bottom,left,right,z-index声明）。

sticky
平时元素根据正常文档流进行定位，然后相对它的最近滚动祖先和 最近块级祖先基于top, right, bottom, 和 left的值进行偏移。偏移值不会影响任何其他元素的位置。该值总是创建一个新的层叠上下文（stacking context）。平时正常，滚动到一定程度就fixed
```

## display 有哪些值？说明他们的作用

- block
  块类型。默认宽度为父元素宽度，可设置宽高，换行显示。
- none
  元素不显示，并从文档流中移除。
- inline
  行内元素类型。默认宽度为内容宽度，不可设置宽高，同行显示。
- inline-block
  默认宽度为内容宽度，可以设置宽高，同行显示。
- list-item
  像块类型元素一样显示，并添加样式列表标记。
- table
  此元素会作为块级表格来显示。
- inherit
  规定应该从父元素继承display属性的值。

## float 的元素，display 是什么

```css
display 为 block
```

## inline-block、inline 和 block 的区别；为什么 img 是 inline 还可以设置宽高

Block 是块级元素，独占一行，

Inline：设置 width 和 height 无效

Inline-block：能设置宽度高度，

```css
img 是可替换元素。

在 CSS 中，可替换元素（replaced element）的展现效果不是由 CSS 来控制的。这些元素是一种外部对象，它们外观的渲染，是独立于 CSS 的。
简单来说，它们的内容不受当前文档的样式的影响。CSS 可以影响可替换元素的位置，但不会影响到可替换元素自身的内容。
例如 `<iframe>` 元素，可能具有自己的样式表，但它们不会继承父文档的样式。

典型的可替换元素有： <iframe> <video> <embed> <img>

有些元素仅在特定情况下被作为可替换元素处理，例如：
  <input> "image" 类型的 <input> 元素就像 <img> 一样可替换
  <option> <audio> <canvas> <object>
  CSS 的 content 属性用于在元素的 ::before 和 ::after 伪元素中插入内容。使用 content 属性插入的内容都是匿名的可替换元素。
```

## flex 的属性有哪些

```JS
flex布局是CSS3新增的一种布局方式，我们可以通过将一个元素的display属性值设置为flex从而使它成为一个flex容器，它的所有子元素都会成为它的项目。

一个容器默认有两条轴，一个是水平的主轴，一个是与主轴垂直的交叉轴。我们可以使用flex-direction来指定主轴的方向。我们可以使用justify-content来指定元素在主轴上的排列方式，使用align-items来指定元素在交叉轴上的排列方式。还可以使用flex-wrap来规定当一行排列不下时的换行方式。

对于容器中的项目，我们可以使用order属性来指定项目的排列顺序，还可以使用flex-grow来指定当排列空间有剩余的时候，项目的放大比例。还可以使用flex-shrink来指定当排列空间不足时，项目的缩小比例。
```

> 以下 6 个属性设置在容器上。

- flex-direction 属性决定主轴的方向（即项目的排列方向）。
- flex-wrap 属性定义，如果一条轴线排不下，如何换行。
- flex-flow 属性是 flex-direction 属性和 flex-wrap 属性的简写形式，默认值为 nowrap。
- justify-content 属性定义了项目在主轴上的对齐方式。
- align-items 属性定义项目在交叉轴上如何对齐。
- align-content 属性定义了多根轴线的对齐方式。如果项目只有一根轴线，该属性不起作用。

> 以下 6 个属性设置在项目上：

- order 属性定义项目的排列顺序。数值越小，排列越靠前，默认为 0。
- flex-grow 属性定义项目的放大比例，默认为 0，即如果存在剩余空间，也不放大。
- flex-shrink 属性定义了项目的缩小比例，默认为 1，即如果空间不足，该项目将缩小。
- flex-basis 指定了 flex 元素在主轴方向上的初始大小。它的默认值为 auto，即项目的本来大小。
- flex 属性是 flex-grow，flex-shrink 和 flex-basis 的简写，默认值为 01auto。
- align-self 属性允许单个项目有与其他项目不一样的对齐方式，可覆盖 align-items 属性。默认值为 auto，表示继承父元素的 align-items 属性，如果没有父元素，则等同于 stretch。

## display: flex 和 display: inline-flex

display: flex会将盒子自动变化block
适用于需要占据一整行空间的布局。

display: inline-flex,盒子容器本身表现与inline-block类似
适用于需要与其他内联元素一起流动的布局。周围的文本会环绕在其旁边。


## `visibility: hidden`, `opacity: 0`，`display: none`

```css
visibility: hidden，不会改变页面布局，不会触发绑定事件；

opacity: 0，不会改变页面布局，能触发事件的；

display: none，把元素删除，会改变文档流布局，不会触发事件。

```

## 知道怎么去减少重绘和重排吗，让文档脱离文档流有哪些方法

DOM 的变化影响到了预算内宿的几何属性比如宽高，浏览器重新计算元素的几何属性，其他元素的几何属性也会受到影响，浏览器需要重新构造渲染树，这个过程称之为重排，浏览器将受到影响的部分重新绘制在屏幕上 的过程称为重绘，引起重排重绘的原因有：

- 添加或者删除可见的 DOM 元素，
- 元素尺寸位置的改变
- 浏览器页面初始化，
- 浏览器窗口大小发生改变，重排一定导致重绘，重绘不一定导致重排，

减少重绘重排的方法有：

- 不在布局信息改变时做 DOM 查询，
- 使用 cssText,className 一次性改变属性
- 使用 fragment
- 对于多次重排的元素，比如说动画。使用绝对定位脱离文档流，使其不影响其他元素

## z-index 是干什么用的？默认值是什么？与 z-index: 0 的区别

> z-index 属性设置元素的堆叠顺序，且只在属性position: relative/absolute/fixed 的时候才生效。
> `z-index: auto` 是默认值，与`z-index: 0`是有区别的：
> `z-index: 0` 会创建一个新的堆叠上下文，而 `z-index: auto` 不会创建新的堆叠上下文

总结：
1. 当Z-index的值设置为auto时,不建立新的堆叠上下文,当前堆叠上下文中生成的div的堆叠级别与其父项的框相同。
2. 当Z-index的值设置为一个整数时,该整数是当前堆叠上下文中生成的div的堆栈级别。该框还建立了其堆栈级别的本地堆叠上下文。这意味着后代的z-index不与此元素之外的元素的z-index进行比较。

## CSS 实现垂直居中

- 定位 + 负边距 元素绝对定位，top:50%，margin-top：-（高度/2）

- 高度不确定用transform：translateY（-50%）

- 父元素display:flex,align-items:center;

- display: table-cell+vertical-align：middle；

- 绝对居中

  ```css
  div {
    width: 300px;
    height: 300px;
    background-color: red;
    position: absolute;
    left: 0;
    right: 0;
    top: 0;
    bottom: 0;
    margin: auto;
  }
  ```

## margin塌陷问题

是在父级相对于浏览器进行定位时但子级没有相对于父级定位，子级相对于父级就像塌陷了一样，父子嵌套元素垂直方向的margin，父子元素是结合在一起的，他们两个会取其中最大的值

或者兄弟元素上下边距会重叠在一起，取较大值。

解决方法：触发bfc，绝对定位，overflow：hidden，display：inline-block

## 图片底部与父容器之间有一段空隙

原因：行内块级元素会和文字的基线对齐。

解决方法：
（1）给图片添加 vertical-align middle | top | bottom


## 移动端适配的方法

> 起因:手机设备屏幕尺寸不一，做移动端的Web页面，需要考虑安卓/IOS的各种尺寸设备上的兼容，针对移动端设备的页面，设计与前端实现怎样做能更好地适配不同屏幕宽度的移动设备；

1. flex 弹性布局,多列布局：column-count：3；网格布局

2. viewport 适配
    
  initial-scale = 屏幕的宽度 / 设计稿的宽度
  <meta name="viewport" content="width=750,initial-scale=0.5">

3. rem 弹性布局,使用百分号单位，或视口单位，或者vw，vh单位
4. 媒体查询：@media screen and (min-width:800px){}
- 响应式图像使用了picture元素的srcset和sizes 特性提供多种资源


## rem 和 em 的区别

- em
  子元素字体大小的 em 是相对于父元素字体大小
  元素的width/height/padding/margin用em的话是相对于该元素的font-size

- rem
  rem 是全部的长度都相对于根元素，根元素是html元素。
  通常做法是给html元素设置一个字体大小，然后其他元素的长度单位就为rem。


## vw 和 vh 的概念

vw（Viewport Width）、vh(Viewport Height)是基于视图窗口的单位

- vw:1vw 等于视口宽度的1%
- vh:1vh 等于视口高度的1%
- vmin: 选取 vw 和 vh 中最小的那个,即在手机竖屏时，1vmin=1vw
- vmax:选取 vw 和 vh 中最大的那个 ,即在手机竖屏时，1vmax=1vh

### js操作css样式
1. 直接修改节点style属性
```js
// style属性名是驼峰语法
  const el = document.getElementById("test-div");
  el.style.backgroundColor = "red";
  el.style.fontSize = "30px";
// style.cssText 批量赋值
  el.style.cssText ="background-color: green !important; font-size: 40px;"

  //style是一个只读属性，表面能赋值成功，但不会生效
  el.style =  {color:"red"}
```
2. 节点className属性，classList
```js
// 修改class类名
  el.className='classA';
// classList下有add增, remove删, contains查, toggle转换
  el.classList.toggle("test-div");
  el.classList.add('class-add');
 
```
3. 动态创建style标签或link标签


## 控制继承的属性

+ inherit 设置该属性会使子元素属性和父元素相同。实际上，就是 "开启继承".
+ initial 设置属性值和浏览器默认样式相同。
+ unset 将属性重置为自然值，如果属性是自然继承那么就是 `inherit`，否则和 `initial`一样


## 图片 base64 编码的优点和缺点

```css
base64编码是一种图片处理格式，通过特定的算法将图片编码成一长串字符串，在页面上显示的时候，可以用该字符串来代替图片的url属性。

优点是：减少一个图片的HTTP请求

缺点是：
（1）根据base64的编码原理，编码后的大小会比原文件大小大1/3，如果把大图片编码到html/css中，不仅会造成文件体积的增加，影响文件的加载速度，还会增加浏览器对html或css文件解析渲染的时间。

（2）使用base64无法直接缓存，要缓存只能缓存包含base64的文件，比如HTML或者CSS，这相比域直接缓存图片的效果要差很多。

一般一些网站的小图标可以使用base64图片来引入。
```

## 如果需要手动写动画,最小时间间隔是多久，为什么

```css
多数显示器默认频率是60Hz，即1秒刷新60次，所以理论上最小间隔为1/60*1000ms＝16.7ms
```

## 阐述一下 CSSSprites

```css
将一个页面涉及到的所有图片都包含到一张大图中去，然后利用CSS的background-image，background-repeat，background-position的组合进行背景定位。
利用CSSSprites能很好地减少网页的http请求，从而很好的提高页面的性能；CSSSprites能减少图片的字节。

优点：
  减少HTTP请求数，极大地提高页面加载速度
  增加图片信息重复度，提高压缩比，减少图片大小
  更换风格方便，只需在一张或几张图片上修改颜色或样式即可实现

缺点：
  图片合并麻烦
  维护麻烦，修改一个图片可能需要重新布局整个图片，样式
```

## 画一条 0.5px 的线

```css
采用meta viewport的方式：initial-scale=0.5

采用border-image的方式：svg

采用transform:scaleY(0.5)的方式
```

## 如何实现单行／多行文本溢出的省略（...）

```css
/*单行文本溢出*/
p {
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

/*多行文本溢出*/
p {
  position: relative;
  line-height: 1.5em;
  /*高度为需要显示的行数*行高，比如这里我们显示两行，则为3*/
  height: 3em;
  overflow: hidden;
}

p:after {
  content: "...";
  position: absolute;
  bottom: 0;
  right: 0;
  background-color: #fff;
}
/*方法2*/
p{display:-webkit-box;

-webkit-box-orient:vertical;

-webkit-line-clamp: 3;//此处的数字可变 此处是三行省略

overflow:hidden;
}
```

## 常见的元素隐藏方式

1. 使用 display:none;隐藏元素，渲染树不会包含该渲染对象，因此该元素不会在页面中占据位置，也不会响应绑定的监听事件。

2. 使用 visibility:hidden;隐藏元素。元素在页面中仍占据空间，但是不会响应绑定的监听事件。

3. 使用 opacity:0;将元素的透明度设置为 0，以此来实现元素的隐藏。元素在页面中仍然占据空间，并且能够响应元素绑定的监听事件。

4. 通过使用绝对定位将元素移除可视区域内，以此来实现元素的隐藏。

5. 通过 z-index 负值，来使其他元素遮盖住该元素，以此来实现隐藏。

6. 通过 clip/clip-path 元素裁剪的方法来实现元素的隐藏，这种方法下，元素仍在页面中占据位置，但是不会响应绑定的监听事件。

7. 通过 transform:scale(0,0)来将元素缩放为 0，以此来实现元素的隐藏。这种方法下，元素仍在页面中占据位置，但是不会响应绑定的监听事件。

## CSS3 @font-face 

@font-face 语句是 css 中的一个功能模块，用于实现网页字体多样性(设计者可随意指定字体，不需要考虑浏览者电脑上是否安装)。
语法：

- 字体的名称
- 字体文件包含在您的服务器上的某个地方，如果字体文件是在不同的位置，请使用完整的 URL 比如：url('http://www.w3cschool.css/css3/Sansation_Light.ttf')

```css
@font-face {
  font-family: myFirstFont;
  src: url('Sansation_Light.ttf'),
      url('Sansation_Light.eot'); /* IE9 */
}
```

## CSS 实现隔行变色

```css
/* 方法一 */
li:nth-child(odd) {background:#ff0000;}
li:nth-child(even) {background:#0000ff;}

/* 方法二 */
li:nth-of-type(odd) { background:#00ccff;} /* 奇数行 */
li:nth-of-type(even) { background:#ffcc00;} /* 偶数行 */
```

注意：nth-child() 和 nth-of-type() 的区别

- nth-child() 选择属于其父元素的第 n 个子元素，
- nth-of-type() 选择属于其父元素的第 n 个特定类型的子元素,只考虑特定类型的子元素。。



## CSS 画三角形

```css
div {
  width: 0;
  height: 0;
  border-width: 20px;
  border-style: solid;
  border-color: transparent transparent red transparent;
}
```

## CSS 画扇形

```css
div {
  height: 0;
  width: 0;
  border: 100px solid transparent;
  border-radius: 50%;
  border-left-color: red;
}
```

## CSS 实现两列固定，中间自适应的布局


> 左右各占200px，中间随着窗口的调整自适应

HTML代码如下：

```html
<div class="div">
  <div class="left">left</div>
  <div class="right">right</div>
  <div class="main">main</div>
</div>
```

- 方法一：通过定位的方式

  ```css
  .wrapper {
      position: relative;
  }
    
  .left {
      position: absolute;
      left: 0;
      top: 0;
      width: 300px;
      height: 500px;
      background-color: lightgreen;
  }
    
  .right {
      position: absolute;
      right: 0;
      top: 0;
      width: 200px;
      height: 500px;
      background-color: lightskyblue;
  }
    
  .center {
      height: 500px;
      margin: 0 200px 0 300px;
      background-color: tomato;
  }
  ```
  
- 方法二：flex布局
  CSS样式：

  ```css
  html, body {
    height: 100%;
  }
  .div {
    height: 100%; /* 想要实现高度百分百必须让父元素都有高度 */
    display: flex;
  }
  .left  {
    flex: 0 0 200px;
    order: 1; /* order定义显示的顺序 */
    background-color: red;
  }
  .right{
    flex: 0 0 200px;
    order: 3;
    background-color: red;
  }
  .main{
    flex: auto;
    order: 2;
    background-color: green;
  }
  ```


## 屏幕里面内容未占满的时候footer固定在屏幕可视区域的底部。占满的时候显示在网页的最底端

- 方式一

```html
<style>
    html,
    body {
        height: 100%;
        margin: 0;
        padding: 0;
    }

    .page {
        box-sizing: border-box;
        min-height: 100%;
        padding-bottom: 300px;
        background-color: gray;
        /* height: 2000px; */
    }

    footer {
        height: 300px;
        margin-top: -300px;
        background-color: #ccc;
    }
</style>

<div class="page">
    主要页面
</div>
<footer>底部</footer>
```

## 移动端 300ms 延迟的原因以及解决方案

> 因为移动端会有双击缩放的这个操作，因此浏览器在 click 之后要等待 300ms，看用户有没有下一次点击，来判断这次操作是不是双击。

1. 通过 meta 标签禁用网页的缩放。

    ```html
    <meta name="viewport" content="user-scalable=no">
    ```

2. 更改默认的视口宽度

    ```html
    <meta name="viewport" content="width=device-width">
    ```


## CSS 选择器的解析规则

从右向左，这样会提高查找选择器所对应的元素的效率

## 关于伪类 LVHA 的解释

```js
a标签有四种状态：链接访问前、链接访问后、鼠标滑过、激活，分别对应四种伪类:link、:visited、:hover、:active；

当链接未访问过时：

（1）当鼠标滑过a链接时，满足:link和:hover两种状态，要改变a标签的颜色，就必须将:hover伪类在:link伪类后面声明；
（2）当鼠标点击激活a链接时，同时满足:link、:hover、:active三种状态，要显示a标签激活时的样式（:active），必须将:active声明放到:link和:hover之后。因此得出LVHA这个顺序。

当链接访问过时，情况基本同上，只不过需要将:link换成:visited。

只有:link和:visited可以交换位置。
```

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

