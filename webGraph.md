# webGraph

## SVG

引用：可以直接在html中编辑svg标签显示；也可以通过<object><img><iframe> 等标签引入.svg文件。还可以通过js动态创建。

例子：

```html
<svg baseProfile="full" width="300" height="200" viewBox ="0 0 600 400>

  <rect width="100%" height="100%" fill="red" />

  <circle cx="150" cy="100" r="80" fill="green" />
                                               
  <ellipse cx="75" cy="75" rx="20" ry="5" stroke="red" fill="transparent" stroke-width="5"/>

  <line x1="10" x2="50" y1="110" y2="150" stroke="orange" fill="transparent" stroke-width="5"/>   
  <polyline points="60 110 65 120 70 115 75 130 80 125 85 140 90 135 95 150 100 145"
      stroke="orange" fill="transparent" stroke-width="5"/>

  <polygon points="50 160 55 180 70 180 60 190 65 205 50 195 35 205 40 190 30 180 45 180"
      stroke="green" fill="transparent" stroke-width="5"/>

  <path d="M20,230 Q40,205 50,230 T90,230" fill="none" stroke="blue" stroke-width="5"/>
	
  <text x="150" y="125" font-size="60" text-anchor="middle" fill="white">SVG</text>
```

viewBox ：可以控制绘制比例，例子中宽300高200，通过viewBox设定后以为该区域代表600X400,变相缩小了所有图形的1/2.

+ <rect> 中含义x：矩形左上角的x位置

  y：矩形左上角的y位置

  width：矩形的宽度

  height：矩形的高度

  rx：圆角的x方位的半径

  ry：圆角的y方位的半径
  
+ circle 中 r：圆的半径
	cx：圆心的x位置
	cy：圆心的y位置
	
+ ellipse 中rx：椭圆的x半径
	ry：椭圆的y半径
	cx：椭圆中心的x位置
	cy：椭圆中心的y位置
	
+ line 直线 中 x1：起点的x位置
	y1：起点的y位置
	x2：终点的x位置
	y2：终点的y位置
	
+ Polyline是一组连接在一起的直线，折线。

+ `polygon`和折线很像，它们都是由连接一组点集的直线构成。不同的是，`polygon`的路径在最后一个点处自动回到第一个点。

+ `path`中最常见的形状。

  - `fill`属性设置对象内部的颜色
  - `stroke`属性设置绘制对象的线条的颜色。
  - 属性`fill-opacity`控制填充色的不透明度，
  - 属性`stroke-opacity`控制描边的不透明度。
  - `stroke-linecap`属性的值有三种可能值：
    - `butt`用直边结束线段，它是常规做法，线段边界90度垂直于描边的方向、贯穿它的终点。
    - `square`的效果差不多，但是会稍微超出`实际路径`的范围，超出的大小由`stroke-width`控制。
    - `round`表示边框的终点是圆角，圆角的半径也是由`stroke-width`控制的。
  - `stroke-linejoin`属性，用来控制两条描边线段之间，用什么方式连接。
    - 取值miter，直角；round，圆角；bevel：切线
  - `stroke-dasharray`属性，将虚线类型应用在描边上，stroke-dasharray=‘’5，5‘’
  - d;`stroke-dasharray`;`stroke-dashoffset` 可以用于css过度与动画

## canvas

```html
<canvas id="tutorial" width="150" height="150"></canvas>

```

canvas会初始化宽度为300像素和高度为150像素。

```js
var canvas = document.getElementById('tutorial');
var ctx = canvas.getContext('2d');
```

[`fillRect(x, y, width, height)`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/fillRect)

绘制一个填充的矩形

[`strokeRect(x, y, width, height)`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/strokeRect)

绘制一个矩形的边框

[`clearRect(x, y, width, height)`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/clearRect)

清除指定矩形区域，让清除部分完全透明。

### 绘制路径

```
beginPath()
```

新建一条路径，生成之后，图形绘制命令被指向到路径上生成路径。

```
closePath()
```

闭合路径之后图形绘制命令又重新指向到上下文中。

```
stroke()
```

通过线条来绘制图形轮廓。不会自动闭合，描边

```
fill()
```

通过填充路径的内容区域生成实心的图形。会自动闭合填充

```js
//绘制三角行
ctx.beginPath();
    ctx.moveTo(75, 50); //起点
    ctx.lineTo(100, 75); //直线
    ctx.lineTo(100, 25);
    ctx.fill();//自动闭合
```

[`arc(x, y, radius, startAngle, endAngle, anticlockwise)`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/arc)

画一个以（x,y）为圆心的以radius为半径的圆弧（圆），从startAngle开始到endAngle结束，按照anticlockwise给定的方向（默认为顺时针）来生成。

```
quadraticCurveTo(cp1x, cp1y, x, y)
```

绘制二次贝塞尔曲线，`cp1x,cp1y`为一个控制点，`x,y为`结束点。

```
bezierCurveTo(cp1x, cp1y, cp2x, cp2y, x, y)
```

绘制三次贝塞尔曲线，`cp1x,cp1y`为控制点一，`cp2x,cp2y`为控制点二，`x,y`为结束点。

## [线型 Line styles](https://developer.mozilla.org/zh-CN/docs/Web/API/Canvas_API/Tutorial/Applying_styles_and_colors#line_styles)

可以通过一系列属性来设置线的样式。

- [`lineWidth = value`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/lineWidth)

  设置线条宽度。

- [`lineCap = type`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/lineCap)

  设置线条末端样式。

- [`lineJoin = type`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/lineJoin)

  设定线条与线条间接合处的样式。

- [`miterLimit = value`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/miterLimit)

  限制当两条线相交时交接处最大长度；所谓交接处长度（斜接长度）是指线条交接处内角顶点到外角顶点的长度。

- [`getLineDash()`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/getLineDash)

  返回一个包含当前虚线样式，长度为非负偶数的数组。

- [`setLineDash(segments)`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/setLineDash)

  设置当前虚线样式。

- [`lineDashOffset = value`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/lineDashOffset)

  设置虚线样式的起始偏移量。

### 渐变

新建一个 `canvasGradient` 对象，并且赋给图形的 `fillStyle` 或 `strokeStyle` 属性。

- [`createLinearGradient(x1, y1, x2, y2)`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/createLinearGradient)

  createLinearGradient 方法接受 4 个参数，表示渐变的起点 (x1,y1) 与终点 (x2,y2)。

- [`createRadialGradient(x1, y1, r1, x2, y2, r2)`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/createRadialGradient)

  createRadialGradient 方法接受 6 个参数，前三个定义一个以 (x1,y1) 为原点，半径为 r1 的圆，后三个参数则定义另一个以 (x2,y2) 为原点，半径为 r2 的圆。

[`gradient.addColorStop(position, color)`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasGradient/addColorStop)

addColorStop 方法接受 2 个参数，`position` 参数必须是一个 0.0 与 1.0 之间的数值，表示渐变中颜色所在的相对位置。例如，0.5 表示颜色会出现在正中间。`color` 参数必须是一个有效的 CSS 颜色值（如 #FFF， rgba(0,0,0,1)，等等）。

### 创建图形模板

[`createPattern(image, type)`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/createPattern)

该方法接受两个参数。Image 可以是一个 `Image` 对象的引用，或者另一个 canvas 对象。`Type` 必须是下面的字符串值之一：`repeat`，`repeat-x`，`repeat-y` 和 `no-repeat`。

## [阴影 Shadows](https://developer.mozilla.org/zh-CN/docs/Web/API/Canvas_API/Tutorial/Applying_styles_and_colors#阴影_shadows)

- [`shadowOffsetX = float`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/shadowOffsetX)

  `shadowOffsetX` 和 `shadowOffsetY `用来设定阴影在 X 和 Y 轴的延伸距离，它们是不受变换矩阵所影响的。负值表示阴影会往上或左延伸，正值则表示会往下或右延伸，它们默认都为 `0`。

- [`shadowOffsetY = float`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/shadowOffsetY)

  shadowOffsetX 和 `shadowOffsetY `用来设定阴影在 X 和 Y 轴的延伸距离，它们是不受变换矩阵所影响的。负值表示阴影会往上或左延伸，正值则表示会往下或右延伸，它们默认都为 `0`。

- [`shadowBlur = float`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/shadowBlur)

  shadowBlur 用于设定阴影的模糊程度，其数值并不跟像素数量挂钩，也不受变换矩阵的影响，默认为 `0`。

- [`shadowColor = color`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/shadowColor)

  shadowColor 是标准的 CSS 颜色值，用于设定阴影颜色效果，默认是全透明的黑色。

### fill mode

```
ctx.fill("evenodd");//
ctx.fill("nonezero");//默认
```

## 使用图片

**`drawImage(image, x, y)`**

其中 `image` 是 image 或者 canvas 对象，`x` 和 `y 是其在目标 canvas 里的起始坐标。`

[`drawImage(image, x, y, width, height)`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/drawImage)

这个方法多了2个参数：`width` 和 `height，`这两个参数用来控制 当向canvas画入时应该缩放的大小

[`drawImage(image, sx, sy, sWidth, sHeight, dx, dy, dWidth, dHeight)`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/drawImage)

第一个参数和其它的是相同的，都是一个图像或者另一个 canvas 的引用。其它8个参数最好是参照右边的图解，前4个是定义图像源的切片位置和大小，后4个则是定义切片的目标显示位置和大小。

# 变形

[`save()`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/save)

保存画布(canvas)的所有状态

[`restore()`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/restore)

save 和 restore 方法是用来保存和恢复 canvas 状态的，回到上一个状态

```
translate(x, y)
```

`translate `方法接受两个参数。x 是左右偏移量，y 是上下偏移量，如右图所示。

- `rotate(angle)`

  这个方法只接受一个参数：旋转的角度(angle)，它是顺时针方向的，以弧度为单位的值。

旋转的中心点始终是 canvas 的原点，如果要改变它，我们需要用到 `translate `方法。

```
scale(x, y)
```

`scale ` 方法可以缩放画布的水平和垂直的单位。两个参数都是实数，可以为负数，x 为水平缩放因子，y 为垂直缩放因子，如果比1小，会缩小图形， 如果比1大会放大图形。默认值为1， 为实际大小。

transform（a,b,c,d,e,f）合并上诉6种变形
