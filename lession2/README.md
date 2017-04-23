# 使用 Canvas 绘制图形

## 绘制矩形

canvas 只支持一种原生的图形绘制：矩形。其他图形绘制需要生成一条路径。canvas 提供三种方法绘制矩形：

```javascript
fillRect(x, y, width, height); // 绘制一个填充的矩形
strokeRect(x, y, width, height); // 绘制一个矩形的边框
clearRect(x, y, width, height); // 清楚指定矩形区域，让清楚部分完全透明
```

## 绘制路径

图形的基本元素是路径。路径是通过不同颜色和宽度的线段或曲线相连形成的不同形状的点的集合。一个路径，甚至一个子路径，都是闭合的。使用路径绘制图形需要一些额外的步骤。

1. 首先，你要创建路径起始点
2. 然后你使用画图命令去画出路径
3. 之后你把路径封闭
4. 一旦路径生成，你就能通过描边或填充路径区域来渲染图形

需要用到的函数：

```javascript
beginPath(); // 新建一条路径，生成之后，图形绘制命令被指向到路径上生成路径
closePath(); // 闭合路径之后图形绘制命令又重新指向到上下文中
stroke(); // 通过线条来绘制图形轮廓
fill(); // 通过填充路径的内容区域生成实心的图形
```

### 移动笔触

当canvas初始化或者 `beginPath()` 调用后，你通常会使用 `moveTo()` 函数设置起点。我们也能够使用 `moveTo()` 绘制一些不连续的路径。

```javascript
moveTo(x, y) // 将笔触移动到指定的坐标 x 以及 y 上
```

### 线

绘制直线，需要用到的方法是 `lineTo()` 。

```javascript
lineTo(x, y); // 绘制一条从当前位置到指定 x 以及 y 位置的直线
```

### 圆弧

绘制圆弧或者圆，我们使用 `arc()` 方法。当然可以使用 `arcTo()` ，不过这个的实现并不是那么的可靠，所以我们这里不作介绍。

```javascript
arc(x, y, radius, startAngle, endAngle, anticlockwise);
arcTo(x1, y1, x2, y2, radius);
// 根据给定的控制点和半径画一段圆弧，再以直线连接两个控制点
```

`arc()` 方法的参数：画一个以(x, y)为圆心，以 radius 为半径的圆弧(圆)，从 startAngle 开始到 endAngle 结束，按照时钟方向(默认顺时针)来生成，为 true 时是逆时针方向。

需要注意的是：arc() 函数中的角度单位是弧度，不是度数。角度与弧度的 JS 表达式`radians = (Math.PI / 180) * degrees` 。

下面两个 `for` 循环，生成圆弧的行列（x,y）坐标。每一段圆弧的开始都调用 `beginPath()`。代码中，每个圆弧的参数都是可变的，实际生活中，我们并不需要这样做。

```javascript
function draw() {
  var canvas = document.getElementById('canvas');
  if (canvas.getContext){
    var ctx = canvas.getContext('2d');

    for(var i=0;i<4;i++){
      for(var j=0;j<3;j++){
        ctx.beginPath();
        var x              = 25+j*50;               // x 坐标值
        var y              = 25+i*50;               // y 坐标值
        var radius         = 20;                    // 圆弧半径
        var startAngle     = 0;                     // 开始点
        var endAngle       = Math.PI+(Math.PI*j)/2; // 结束点
        var anticlockwise  = i%2==0 ? false : true; // 顺时针或逆时针

        ctx.arc(x, y, radius, startAngle, endAngle, anticlockwise);

        if (i>1){
          ctx.fill();
        } else {
          ctx.stroke();
        }
      }
    }
  }
}
```

### 贝塞尔（bezier）以及二次贝塞尔

一般用来绘制复制有规律的图形。

```javascript
quadraticCurveTo(cp1x, cp1y, x, y);
bezierCurveTo(cp1x, cp1y, cp2x, cy2y, x, y)
```

贝塞尔曲线有一个开始、结束点以及一个控制点，而二次贝塞尔曲线使用两个控制点。

参数 x、y 在这两个方法里面均为结束点坐标。其余是控制点坐标。下面的例子通过贝塞尔曲线绘制心形。

```javascript
function draw() {
  var canvas = document.getElementById('canvas');
  if (canvas.getContext){
    var ctx = canvas.getContext('2d');

    //二次曲线
    ctx.beginPath();
    ctx.moveTo(75,40);
    ctx.bezierCurveTo(75,37,70,25,50,25);
    ctx.bezierCurveTo(20,25,20,62.5,20,62.5);
    ctx.bezierCurveTo(20,80,40,102,75,120);
    ctx.bezierCurveTo(110,102,130,80,130,62.5);
    ctx.bezierCurveTo(130,62.5,130,25,100,25);
    ctx.bezierCurveTo(85,25,75,37,75,40);
    ctx.fill();
  }
}
```

## Path2D 对象

你可以使用一系列的路径和绘画命令把对象“画”在画布上。为了简化代码和提高性能，Path2D 对象已可以在较新版本的浏览器中使用，用来缓存或记录绘画命令，这样你将能快速地回顾路径。

### 产生一个 Path2D 对象

`Path2D()` 会返回一个新初始化的 Path2D 对象（可能将某一个路径作为变量—创建一个它的副本，或者将一个包含 SVG path 数据的字符串作为变量）。

```javascript
new Path2D(); //空的 Path 对象
new Path2D(path); // 克隆 Path 对象
new Path2D(d); // 从 SVG 建立 Path 对象
```

所有的路径方法都可以在 Path2D 中使用。Path2D API 添加了 `addPath` 作为将 path 结合起来的方法。当你想从几个元素中来创建对象时，这将会很实用。比如：

```javascript
Path2D.addPath(path [, transform]); // 添加了一条路径到当前路径(可能添加一个变换矩阵)
```

新的Path2D API有另一个强大的特点，就是使用SVG path data来初始化canvas上的路径。这将使你获取路径时可以以SVG或canvas的方式来重用它们。

这条路径将先移动到点 `(M10 10)` 然后再水平移动80个单位 `(h 80)`，然后下移80个单位 `(v 80)`，接着左移80个单位 `(h -80)`，再回到起点处 (`z`)。

```javascript
var p = new Path2D("M10 10 h 80 v 80 h -80 Z");
```