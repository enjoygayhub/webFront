# CSS

## flex 容器

一个容器默认有两条轴，一个是水平的主轴，一个是垂直的交叉轴（主轴方向由 flex-direction 决定）。

> 以下 6 个属性设置在容器上。

- flex-direction 属性决定主轴的方向（即项目的排列方向）。
- flex-wrap 属性定义，如果一条轴线排不下，如何换行。
- flex-flow 属性是 flex-direction 属性和 flex-wrap 属性的简写形式，默认值为 row nowrap。
- justify-content 属性定义了项目在主轴上的对齐方式。
- align-items 属性定义项目在交叉轴上如何对齐。
- align-content 属性定义了多根轴线的对齐方式。如果项目只有一根轴线，该属性不起作用。

> 以下 6 个属性设置在子项目上

- order 属性定义项目的排列顺序。数值越小，排列越靠前，默认为 0。
- flex-grow 属性定义项目的放大比例，默认为 0，即如果存在剩余空间，也不放大。
- flex-shrink 属性定义了项目的缩小比例，默认为 1，即如果空间不足，该项目将缩小。
- flex-basis 指定了 flex 元素在主轴方向上的初始大小。它的默认值为 auto，即项目的本来大小。
- flex 属性是 flex-grow，flex-shrink 和 flex-basis 的简写，默认值为 0 1 auto。
- align-self 属性允许单个项目有与其他项目不一样的对齐方式，可覆盖 align-items 属性。默认值为 auto，表示继承父元素的 align-items 属性，如果没有父元素，则等同于 stretch。

## display: flex 和 display: inline-flex

display: flex会将盒子变为块级，
适用于需要占据一整行空间的布局。

display: inline-flex,盒子容器本身表现与inline-block类似
适用于需要与其他内联元素一起流动的布局。周围的文本会环绕在其旁边。

## CSS 盒模型

盒模型总共包括4个部分：

- content：内容，容纳着元素的“真实”内容，例如文本
- padding：内边距
- border：边框
- margin：外边距

- `box-sizing: content-box` (默认)

  > 通过CSS样式设置的width的大小只是content的大小

- `box-sizing: border-box` （常用）

  > 通过CSS样式设置的width的大小是content + padding + border的和

## 设置一个元素的背景颜色，背景颜色会填充哪些区域

与background-clip属性有关:
border-box: 背景颜色填充内容区、内边距区域和边框区域（默认）。
padding-box: 背景颜色填充内容区和内边距区域。
content-box: 背景颜色仅填充内容区。

## margin/padding 设置百分比是相对谁的

总结：margin/padding设置百分比**都是**相对于**父盒子的宽度**(width属性)来计算量的

## CSS 选择器优先级

选择器按优先级先后排列：!important>内联>id>class = 属性 = 伪类 >标签 = 伪元素 > 通配符 \*

## 伪类与伪元素的区别

使用单冒号来表示伪类，用双冒号来表示伪元素。
伪类一般匹配的是元素的一些特殊状态，如hover、link ，active等，
而伪元素一般匹配的特殊的位置，比如after、before等。
伪元素默认行内元素，css中其必须有content属性，就算是content: ""也必须写，否则无效。
css引入伪类和伪元素概念是为了格式化文档树以外的信息。修饰不在文档树中的部分，比如，一句话中的第一个字母，或者是列表中的第一个元素。

## CSS 中哪些属性可以继承

字体相关的属性，font-size和font-weight等。文本相关的属性，color和text-align,
表格的一些布局属性、列表属性如list-style等。还有光标属性cursor、元素可见性visibility。

当一个属性不是继承属性的时候，通过将它的值设置为inherit来继承。

## BFC的概念

> BFC 即 Block Formatting Context (块格式化上下文)， 是Web页面的可视化CSS渲染的一部分，是块盒子的布局过程发生的区域

简单来说就是一个封闭的黑盒子，里面元素的布局不会影响外部。

## 脱离文档流的方式

- float
- position: absolute
- position: fixed

## position 的值定位

- absolute
  生成绝对定位的元素，相对于值不为static的第一个父元素（position 为 absolute、relative 或 fixed）的 padding box 进行定位，
  即以离自己最近的、position 不为 static 的祖先元素的 padding box 左上角为原点。
- fixed
  生成绝对定位的元素，相对于浏览器窗口进行定位（若祖先元素设置了 transform/filter/perspective，则相对该祖先定位）。
- relative
  生成相对定位的元素，相对于其自身在正常文档流中的原位置（static 时的位置）进行定位。
- static
  默认值。没有定位，元素出现在正常的流中（忽略top,bottom,left,right,z-index声明）。
- sticky
  平时根据正常文档流定位，滚动到一定程度后表现为 fixed：相对最近的滚动祖先基于 top/right/bottom/left 偏移，偏移值不影响其他元素的位置。该值总是创建一个新的层叠上下文。

## display 有哪些

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
- flex / inline-flex
  弹性盒子。

## float 的元素，display 是什么

display 为 block

## inline-block、inline 和 block 的区别；为什么 img 是 inline 还可以设置宽高

Block 是块级元素，独占一行，
Inline：设置 width 和 height 无效

Inline-block：能设置宽度高度，

img 是可替换元素。
在 CSS 中，可替换元素（replaced element）的展现效果不是由 CSS 来控制的。
这些元素是一种外部对象，外观的渲染，是独立于 CSS 的。
典型的可替换元素有： <iframe> <video> <embed> <img>

## z-index 是干什么用的？默认值是什么？与 z-index: 0 的区别

> z-index 属性设置元素的堆叠顺序，且只在属性position: relative/absolute/fixed/sticky 的时候才生效（flex/grid 子项也可生效）。
> `z-index: auto` 是默认值，与`z-index: 0`是有区别的：
> `z-index: 0` 会创建一个新的堆叠上下文，而 `z-index: auto` 不会创建新的堆叠上下文

## CSS 实现垂直居中

- 2024年新增属性 align-content: center

- 定位 + 负边距 元素绝对定位，top:50%; margin-top: -(高度/2)

- 高度不确定用 top:50%; transform: translateY(-50%)

- 父元素display:flex; align-items:center;

- display: table-cell + vertical-align: middle;

- 高度确定情形下，绝对定位，margin: auto

## margin塌陷问题

发生在相邻（父子、兄弟）元素垂直方向上：margin 会合并取其中较大的值；父子嵌套时，子元素的 margin-top 会“穿透”到父元素上，表现为子级像塌陷了一样。

或者兄弟元素上下边距会重叠在一起，取较大值。

解决方法：触发bfc

## 图片底部与父容器之间有一段空隙

原因：行内块级元素会和文字的基线对齐。

解决方法：给图片添加 vertical-align: middle | top | bottom

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

## 控制继承的属性

- inherit 设置该属性会使子元素属性和父元素相同。实际上，就是 "开启继承".
- initial 设置属性值和浏览器默认样式相同。
- unset 将属性重置为自然值，如果属性是自然继承那么就是 `inherit`，否则和 `initial`一样

## 常见的元素隐藏方式

1. 使用 display:none;渲染树不会包含该渲染对象，

2. 使用 visibility:hidden;元素在页面中仍占据空间，不会响应绑定的监听事件。

3. 使用 opacity:0;元素在页面中仍然占据空间，并且能够响应元素绑定的监听事件。

4. 通过使用绝对定位将元素移除可视区域内。

5. 通过 z-index 负值，来使其他元素遮盖住该元素

6. 通过 clip/clip-path 元素仍在页面中占据位置，但是不会响应绑定的监听事件。

7. 通过 transform:scale(0,0)来将元素缩放为 0，元素仍在页面中占据位置，但是不会响应绑定的监听事件。

## CSS 实现隔行变色

```css
/* 方法一 */
li:nth-child(odd) {
  background: #ff0000;
}

/* 方法二 */
li:nth-of-type(even) {
  background: #ffcc00;
} /* 偶数行 */
```

注：nth-child() 选择的是所有子元素，而 nth-of-type() 选择的是特定类型的子元素。
nth-of-type 只按标签类型计数，不认类名。

## 图片 base64 编码

base64编码是一种图片处理格式，通过特定的算法将图片编码成一长串字符串，在页面上显示的时候，可以用该字符串来代替图片的url属性。

优点是：减少一个图片的HTTP请求

缺点是：编码后的大小会比原文件大小大1/3，文件体积的增加，影响的加载速度，还会增加浏览器对html或css文件解析渲染的时间。

## 动画最小时间间隔是多久

多数显示器默认频率是60Hz，即1秒刷新60次，所以理论上最小间隔为1/60\*1000ms = 16.7ms

## CSS 选择器的解析规则

核心解析规则：CSS 选择器遵循「从右往左（关键选择器优先）」匹配，同时有「ID > 类 > 标签」的优先级权重规则；
设计原因：从右往左解析是为了最小化 DOM 遍历次数，提升渲染性能；优先级规则是为了明确样式冲突的解决逻辑，降低开发复杂度；
