# 1. 基本用法

## 1.1 canvas 标签

```html
<canvas id="cv" width="150" height="150"></canvas>
```

`<canvas>` 标签只有两个属性**——** `width`和 `height`。当没有设置宽度和高度的时候，canvas会初始化宽度为300像素和高度为150像素。

当使用 css 定义宽度和高度时，如果 css 尺寸与初始画布比例不一致，它会出现扭曲。

## 1.2 兼容性

在 `canvas` 内部可以提供替代内容，用于不支持 `canvas` 的浏览器显示。支持 `canvas` 的浏览器则会忽略`canvas` 中的内容。

```html
<canvas id="clock" width="150" height="150">
  <img src="images/clock.png" width="150" height="150" alt=""/>
</canvas>
```

## 1.3 渲染上下文

`getContent` 用来获取 `canvas` 的渲染上下文和它的绘画功能。

`getContext()` 接受一个参数，即上下文的类型。

```js
var canvas = document.getElementById('tutorial');
if(!canvas.getContext) {
  // 处理不支持 canvas 时的代码
  return
}
var ctx = canvas.getContext('2d');
```



# 2. 绘制图形

**栅格**的起点为左上角（坐标为（0,0））。所有元素的位置都相对于起点定位。

## 2.1 矩形

`canvas` 只支持两种图形绘制：矩形和路径。所有其他类型的图形都是通过一条或者多条路径组合而成的。

1. `fillRect(x, y, width, height)` ：绘制一个填充的矩形

   ```js
   const canvas = document.getElementById('cv')
   const ctx = canvas.getContext('2d');
   ctx.fillRect(10, 10, 80, 20)
   ```

​	![image-20220504162957597](https://gitee.com/haibolian/screenshot/raw/master/images/image-20220504162957597.png)	

2. `strokeRect(x, y, width, height)`：绘制一个矩形边框

   stroke：笔画

   ```js
   ctx.strokeRect(10, 10, 80, 20)
   ```

​	![image-20220504163207564](https://gitee.com/haibolian/screenshot/raw/master/images/image-20220504163207564.png)

3. `clearRect(x, y, width, height)`：通过把像素设置为透明以达到擦除一个矩形区域的目的

   ```js
   ctx.fillRect(10, 10, 80, 20)
   ctx.clearRect(10, 10, 30, 10)
   ```

​	![image-20220504163654333](https://gitee.com/haibolian/screenshot/raw/master/images/image-20220504163654333.png)

4. `rect(x, y, width, height)`：绘制一个左上角坐标为（x,y），宽高为width以及height的矩形。

**综合使用**：

```js
ctx.fillRect(25, 25, 100, 100);
ctx.clearRect(45, 45, 60, 60);
ctx.strokeRect(50, 50, 50, 50);
```

​	![image-20220504163939751](https://gitee.com/haibolian/screenshot/raw/master/images/image-20220504163939751.png)



## 2.2 路径

**步骤**

1. 首先，创建路径起始点
2. 使用画图命令画出路径
3. 把路径封闭
4. 通过描边或填充路径区域来渲染图形

**API**

1. `beginPath()`：新建一条路径，生成之后，图形绘制命令被指向到路径上生成路径。
2. `moveTo(x, y)`：将笔触移动到指定的坐标x以及y上。当canvas初始化或者`beginPath()`调用后，通常会使用`moveTo()`函数设置起点。也能够使用`moveTo()`绘制一些不连续的路径。
3. `closePath()`：闭合路径之后图形绘制命令又重新指向到上下文中。
4. `stroke()`：通过线条来绘制图形轮廓。
5. `fill()`：通过填充路径的内容区域生成实心的图形。

> **注意：当你调用fill()函数时，所有没有闭合的形状都会自动闭合，所以你不需要调用closePath()函数。但是调用stroke()时不会自动闭合**。



## 2.3 线

`lineTo(x, y)`：绘制一条从当前位置到指定x以及y位置的直线。

**填充**

```js
ctx.beginPath();
ctx.moveTo(200, 0)
ctx.lineTo(100, 100)
ctx.lineTo(300, 100)
ctx.closePath() // 此行可注释，调用fill()函数时，所有没有闭合的形状都会自动闭合。
ctx.stroke()
```

![image-20220504170709909](https://gitee.com/haibolian/screenshot/raw/master/images/image-20220504170709909.png)

**描边**

当使用 `stroke` 时，`closePath` 不可注释

```js
ctx.beginPath();
ctx.moveTo(200, 0)
ctx.lineTo(100, 100)
ctx.lineTo(300, 100)
// ctx.closePath()
ctx.stroke()
```

![image-20220504170856212](https://gitee.com/haibolian/screenshot/raw/master/images/image-20220504170856212.png)



## 2.4 圆弧

`arc(x, y, radius, startAngle, endAngle, anticlockwise)`

圆弧路径的圆心在 *(x, y)* 位置，半径为 *r* ，根据*anticlockwise* （默认为顺时针）指定的方向从 *startAngle* 开始绘制，到 *endAngle* 结束。

* `x`：圆弧中心（圆心）的 x 轴坐标。
* `y`：圆弧中心（圆心）的 y 轴坐标。

* `radius`：圆弧的半径。

* `startAngle`：圆弧的起始点， **x轴方向开始计算**，单位以弧度表示。

* `endAngle`：圆弧的终点， 单位以弧度表示。
* `anticlockwise` （可选），可选的 `Boolean` 值 ，如果为 `true`，逆时针绘制圆弧，反之，顺时针绘制。 

```js
ctx.beginPath();
ctx.arc(100, 60, 50, 0, 2 * Math.PI)
ctx.stroke()
```

![image-20220504171957167](https://gitee.com/haibolian/screenshot/raw/master/images/image-20220504171957167.png)	

**`startAngle`** ：圆弧起始点，x 轴方向开始计算

```js
ctx.beginPath();
ctx.arc(100, 60, 50, 0, 1.5 * Math.PI)
ctx.stroke()
```

![image-20220504172503263](https://gitee.com/haibolian/screenshot/raw/master/images/image-20220504172503263.png)

`arcTo(x1, y1, x2, y2, radius)`：使用当前的描点(前一个moveTo或lineTo等函数的止点)。根据当前描点与给定的控制点1连接的直线，和控制点1与控制点2连接的直线，作为使用指定半径的圆的**切线**，画出两条切线之间的弧线路径。

![arcTo](https://gitee.com/haibolian/screenshot/raw/master/images/arcTo.png)

**注意**

在使用 `arc` 和 `rect ` 时，需要利用 `beginPath()` 声明开启新的绘制。例如

```js
ctx.rect(200, 50, 150, 50)
ctx.arc(50, 50, 25, 0, 2 * Math.PI)
ctx.stroke()
```

![image-20220504213232959](https://gitee.com/haibolian/screenshot/raw/master/images/image-20220504213232959.png)

它会用一根线将两个图形的起点连接起来。`rect`、`arc` 画完之后画笔并未 *抬起来* ，而是划到另一个起点。

```js
ctx.rect(200, 50, 150, 50)
ctx.stroke() // beginPath 前需要把图形画出来，否则不显示
ctx.beginPath()
ctx.arc(50, 50, 25, 0, 2 * Math.PI)
ctx.stroke()
```

![image-20220504213746413](https://gitee.com/haibolian/screenshot/raw/master/images/image-20220504213746413.png)



## 2.5 二次贝塞尔曲线与三次贝塞尔曲线

[二次贝塞尔曲线调试](http://blogs.sitepointstatic.com/examples/tech/canvas-curves/quadratic-curve.html)

[三次贝塞尔曲线调试](https://cubic-bezier.com/)

二次及三次贝塞尔曲线一般用来绘制复杂有规律的图形。

`quadraticCurveTo(cp1x, cp1y, x, y)`： 绘制二次贝塞尔曲线，`cp1x,cp1y`为一个控制点，`x,y为`结束点。

`bezierCurveTo(cp1x, cp1y, cp2x, cp2y, x, y)`： 绘制三次贝塞尔曲线，`cp1x`, `cp1y` 为控制点一，`cp2x`, `cp2y` 为控制点二，`x`,`y`为结束点。

![Canvas_curves](https://gitee.com/haibolian/screenshot/raw/master/images/Canvas_curves.png)

```js
ctx.moveTo(75, 25);
ctx.quadraticCurveTo(25, 25, 25, 62.5);
ctx.quadraticCurveTo(25, 100, 50, 100);
ctx.quadraticCurveTo(50, 120, 30, 125);
ctx.quadraticCurveTo(60, 120, 65, 100);
ctx.quadraticCurveTo(125, 100, 125, 62.5);
ctx.quadraticCurveTo(125, 25, 75, 25);
ctx.stroke();
```

这个图像是从左上角开始画的

![image-20220504175211085](https://gitee.com/haibolian/screenshot/raw/master/images/image-20220504175211085.png)	

## 2.6 Path2D

> **Experimental:** **这是一个实验中的功能**
> 此功能某些浏览器尚在开发中，请参考[浏览器兼容性表格](https://developer.mozilla.org/zh-CN/docs/Web/API/Path2D#浏览器兼容性)以得到在不同浏览器中适合使用的前缀。由于该功能对应的标准文档可能被重新修订，所以在未来版本的浏览器中该功能的语法和行为可能随之改变。

`Path2D(path / d)` 可以选择另一条路径作为参数（创建一个拷贝），或者选择 [SVG path](https://developer.mozilla.org/en-US/docs/Web/SVG/Tutorial/Paths) 数据构成的字符串。

`path`：可选，当调用另一个 `Path2D` 对象时，会创建一个 `path` 变量的拷贝。

`d`：当调用 [SVG path](https://developer.mozilla.org/en-US/docs/Web/SVG/Tutorial/Paths) 数据构成的字符串时，会根据描述创建一个新的路径。

```js
const ctx = canvas.getContext('2d');
ctx.beginPath();
const rectangle = new Path2D()
rectangle.rect(50, 20, 100, 50)
ctx.stroke(rectangle)

const circle = new Path2D()
circle.arc(200, 100, 50, 0, 2 * Math.PI)
ctx.fill(circle)
```

![image-20220504205349810](https://gitee.com/haibolian/screenshot/raw/master/images/image-20220504205349810.png)	



# 3. 样式和颜色

## 3.1 色彩

`fillStyle = color`：设置图形的填充颜色。

`strokeStyle = color`：设置图形轮廓的颜色。

默认情况下，线条和填充颜色都是黑色（#000000）

```js
for (let i = 0; i < 6; i++) {
  for (let j = 0; j < 6; j++) {
    ctx.fillStyle = `rgb(${i * 45.5}, ${j * 45.5}, 0)`
    ctx.fillRect(10 + i * 25, 10 + j * 25, 25, 25)
  } 
}
for (let i = 0; i < 6; i++) {
  for (let j = 0; j < 6; j++) {
    ctx.beginPath()
    ctx.strokeStyle = `rgb(${i * 45.5}, ${j * 45.5}, 0)`
    ctx.arc(200 + i * 25, 15 + j * 25, 10, 0, 2 * Math.PI)
    ctx.stroke()
  } 
}
```

![image-20220504211939307](https://gitee.com/haibolian/screenshot/raw/master/images/image-20220504211939307.png)

## 3.2 透明度

1. `canvas` 中设置透明度的属性是 `globalAlpha` ，取值范围和 `css` 的 `opacity` 类似。例如

```js
ctx.globalAlpha = 0.2;
```

2. 当然，也可以在设置色彩时，使用带透明度的 rgba 或者十六进制来实现透明效果。

采用第二种方法会具有更好的可操作性和灵活性，因为它可以分别设置轮廓和填充样式。

## 3.3 线型 Line styles

* `lineWidth`： 设置线条宽度

* `lineCap`： 设置线条末端样式

  * `butt`： 与辅助线齐平的（默认）

  * `round`： 端点处加上了半径为一半线宽的半圆

  * `square`：端点处加上了等宽且高度为一半线宽的方块。

    ![Canvas_linecap](https://gitee.com/haibolian/screenshot/raw/master/images/Canvas_linecap.png)	

* `lineJoin`： 设定线条与线条间接合处的样式。

  * `round`：边角处被磨圆了，圆的半径等于线宽
  * `bevel`
  * `miter`：线段会在连接处外侧延伸直至交于一点，延伸效果受到 `miterLimit` 属性的制约。

* `miterLimit`：限制当两条线相交时交接处最大长度；所谓交接处长度（斜接长度）是指线条交接处内角顶点到外角顶点的长度。

  [miterLimit 用到时再看吧，感觉现在看了也会忘](https://developer.mozilla.org/zh-CN/docs/Web/API/Canvas_API/Tutorial/Applying_styles_and_colors#transparency)

## 3.4 虚线

 `setLineDash`：接受一个数组，来指定线段与间隙的交替；

`lineDashOffset`：设置起始偏移量，可以实现一些动画效果

```js
let offset = 0;

function draw() {
  ctx.clearRect(0,0, canvas.width, canvas.height);
  ctx.setLineDash([4, 2]);
  ctx.lineDashOffset = -offset;
  ctx.strokeRect(10,10, 100, 100);
}

function march() {
  offset++;
  if (offset > 16) {
    offset = 0;
  }
  draw();
  setTimeout(march, 20);
}

march();
```

![image-20220504222513573](https://gitee.com/haibolian/screenshot/raw/master/images/image-20220504222513573.png)	

## 3.5 渐变

[todo](https://developer.mozilla.org/zh-CN/docs/Web/API/Canvas_API/Tutorial/Applying_styles_and_colors#gradients)