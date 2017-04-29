# 使用样式和颜色

## 色彩 colors

如果我们想要给图形上色，有两个重要的属性可以做到：fillStyle 和 strokeStyle。

```javascript
fillStyle = color // 设置图形的填充颜色
strokeStyle = color // 设置图形轮廓的颜色
```

color 可以是表示 CSS 颜色值的字符串，渐变对象或者图案对象。

### fillStyle 示例

在本示例里，我会再度使用两层 for 循环来控制绘制方格阵列。用了两个变量 i 和 j 来为每一个方格产生唯一的 RGB 色彩值。

```javascript
function draw() {
  var ctx = document.getElementById('canvas').getContext('2d');
  for (var i = 0; i < 6; i++) {
    for (var j = 0; j < 6; j++) {
      ctx.fillStyle = 'rgb(' + Math.floor(255 - 42.5 * i) + ',' +
        Math.floor(255 - 42.5 * j) + ',0';
      ctx.fillRect(j * 25, i * 25, 25, 25);
    }
  }
}
```

### strokeStyle 示例

这个示例与上面的有点类似，但是这次用到的是 strokeStyle 属性，用 arc 方法来画圆。

```javascript
function draw() {
  var ctx = document.getElementById('canvas').getContext('2d');
  for (var i = 0; i < 6; i++) {
    for (var j = 0; j < 6; j++) {
      ctx.strokeStyle = 'rgb(0,' + Math.floor(255 - 42.5 * i) + ',' + Math.floor(255 - 42.5 * j) + ')';
      ctx.beginPath();
      ctx.arc(12.5 + j * 25, 12.5 + i * 25, 10, 0, Math.PI * 2, true);
      ctx.stroke();
    }
  }
}
```

## 透明度 Transparency

我们可以用 Canvas 绘制半透明的图形。通过设置 globalAlpha 属性或者使用一个半透明的颜色作为轮廓或填充样式。

`globalAlpha` 属性会影响到 canvas 里面所有图形的透明度。在需要绘制大量拥有相同透明度的图形时候相当高效。使用 rgba() 方法可操作性更强一点。

### globalAlpha 示例

在这个例子里，将用四色格作为背景，设置 `globalAlpha` 为 `0.2` 后，在上面画一系列半径递增的半透明圆。最终结果是一个径向渐变效果。圆叠加得越更多，原先所画的圆的透明度会越低。通过增加循环次数，画更多的圆，背景图的中心部分会完全消失。

```javascript
function draw() {
  var ctx = document.getElementById('canvas').getContext('2d');
  // 画背景
  ctx.fillStyle = '#FD0';
  ctx.fillRect(0,0,75,75);
  ctx.fillStyle = '#6C0';
  ctx.fillRect(75,0,75,75);
  ctx.fillStyle = '#09F';
  ctx.fillRect(0,75,75,75);
  ctx.fillStyle = '#F30';
  ctx.fillRect(75,75,75,75);
  ctx.fillStyle = '#FFF';

  // 设置透明度值
  ctx.globalAlpha = 0.2;

  // 画半透明圆
  for (var i=0;i<7;i++){
      ctx.beginPath();
      ctx.arc(75,75,10+10*i,0,Math.PI*2,true);
      ctx.fill();
  }
}
```

###  rgba 示例

第二个例子和上面那个类似，不过不是画圆，而是画矩形。

```javascript
function draw() {
  var ctx = document.getElementById('canvas').getContext('2d');

  // 画背景
  ctx.fillStyle = 'rgb(255,221,0)';
  ctx.fillRect(0,0,150,37.5);
  ctx.fillStyle = 'rgb(102,204,0)';
  ctx.fillRect(0,37.5,150,37.5);
  ctx.fillStyle = 'rgb(0,153,255)';
  ctx.fillRect(0,75,150,37.5);
  ctx.fillStyle = 'rgb(255,51,0)';
  ctx.fillRect(0,112.5,150,37.5);

  // 画半透明矩形
  for (var i=0;i<10;i++){
    ctx.fillStyle = 'rgba(255,255,255,'+(i+1)/10+')';
    for (var j=0;j<4;j++){
      ctx.fillRect(5+i*14,5+j*37.5,14,27.5)
    }
  }
}
```

## 线型 Line styles

可以通过一系列属性来设置线的样式。

```javascript
lineWidth = value; // 设置线条宽度
lineCap = type; // 设置线条末端样式
lineJoin = type; // 设定线条与线条间接合处的样式
miterLimit = value; // 限制当两条线相交时交接处最大长度(指先挑交接处内角顶点到外角顶点的长度)
getLineDash(); // 返回一个包含当前虚线样式，长度为非负偶数的数组
setLineDash(); // 设置当前虚线样式
lineDashOffset = value; // 设置虚线样式的起始偏移量
```

### lineWidth

这个属性决定线条的粗细，属性必须为正数，默认为1.0。

线宽是指给定路径的中心到两边的粗细，即在路径两边各绘制线宽的一半。因为画布坐标并不和像素直接对应，当需要获得精确的水平或垂直线的时候需要特别注意。

```javascript
function draw() {
  var ctx = document.getElementById('canvas').getContext('2d');
  for (var i = 0; i < 10; i++) {
    ctx.lineWidth = 1 + i;
    ctx.beginPath();
    ctx.moveTo(5 + i * 14, 5);
    ctx.lineTo(5 + i * 14, 140);
    ctx.stroke();
  }
}
```

![img](https://developer.mozilla.org/@api/deki/files/601/=Canvas-grid.png)

### lineCap

这个属性决定了线段端点显示的样子，包括 `butt` `round` `square` 。

- `butt` 与辅助线齐平
- `round` 在端点处加上了半径为一半线宽的半圆
- `square` 在端点处加上了等款且高度为线宽一半的方块。

### lineJoin

这个属性决定了图形中两线段连接处显示的样子。包括 `round` `bevel` `miter` 。

- `round` 边角处被磨圆，半径与线等宽
- `bevel` 平边
- `miter` 会在连接处外侧延伸至交点（收到 miterLimit 属性制约）

### miterLimit

线段的外侧边缘会延伸交汇于一点上。线段直接夹角比较大的，交点不会太远，但当夹角减少时，交点距离会呈指数级增大。`miterLimit` 属性就是用来设定外延交点与连接点的最大距离，如果交点距离大于此值，连接效果会变成了 bevel。

## 使用虚线

用 `setLineDash` 方法和 `lineDashOffset` 属性来制定虚线样式。`setLineDash` 方法接受一个数组，来指定线段与间隙的交替；`lineDashOffset` 属性设置起始偏移量。

```javascript
var ctx = document.getElementById('canvas').getContext('2d');
var offset = 0;

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

# 渐变 Gradients

就好像一般的绘图软件一样，我们可以用线性或者径向的渐变来填充或描边。我们用下面的方法新建一个 `canvasGradient` 对象，并且赋给图形的 `fillStyle` 或 `strokeStyle` 属性。

```javascript
/* 创建 canvasGradient 对象 */
createLinearGradient(x1, y1, x2, y2);
// 接受4个参数，表示渐变的起点和终点
createRadialGradient(x1, y1, r1, x2, y2, r2);
// 接受6个参数，表示原点和圆的半径

/* 为 canvasGradient 对象上色 */
gradient.addColorStop(position, color);
// 接受2个参数，position 必须是一个0.0到1.0之间的数值，表示渐变中颜色所在的相对位置。color 参数必须是一个有效的 CSS 颜色值

```

创建出 canvasGradient 对象后，我们就可以用 addColorStop 方法给它上色了。

 ```javascript
var lineargradient = ctx.createLinearGradient(0,0,150,150);
lineargradient.addColorStop(0,'white');
lineargradient.addColorStop(1,'black');
 ```

## createLinearGradient 的例子

第一种是背景色渐变，你会发现，我给同一位置设置了两种颜色，你也可以用这来实现突变的效果，就像这里从白色到绿色的突变。第二种渐变，我并不是从 0.0 位置开始定义色标，因为那并不是那么严格的。在 0.5 处设一黑色色标，渐变会默认认为从起点到色标之间都是黑色。

你会发现，`strokeStyle` 和 `fillStyle` 属性都可以接受 `canvasGradient` 对象。

```javascript
function draw() {
  var ctx = document.getElementById('canvas').getContext('2d');

  // Create gradients
  var lingrad = ctx.createLinearGradient(0,0,0,150);
  lingrad.addColorStop(0, '#00ABEB');
  lingrad.addColorStop(0.5, '#fff');
  //lingrad.addColorStop(0.5, '#26C000');
  //lingrad.addColorStop(1, '#fff');

  var lingrad2 = ctx.createLinearGradient(0,50,0,95);
  lingrad2.addColorStop(0.5, '#000');
  lingrad2.addColorStop(1, 'rgba(0,0,0,0)');

  // assign gradients to fill and stroke styles
  ctx.fillStyle = lingrad;
  ctx.strokeStyle = lingrad2;
  
  // draw shapes
  ctx.fillRect(10,10,130,130);
  ctx.strokeRect(50,50,50,50);
}
```

## createRadialGradient 的例子

这个例子，我定义了 4 个不同的径向渐变。由于可以控制渐变的起始与结束点，所以我们可以实现一些比（如在 Photoshop 中所见的）经典的径向渐变更为复杂的效果。（经典的径向渐变是只有一个中心点，简单地由中心点向外围的圆形扩张）

```javascript
function draw() {
  var ctx = document.getElementById('canvas').getContext('2d');

  // 创建渐变
  var radgrad = ctx.createRadialGradient(45,45,10,52,50,30);
  radgrad.addColorStop(0, '#A7D30C');
  radgrad.addColorStop(0.9, '#019F62');
  radgrad.addColorStop(1, 'rgba(1,159,98,0)');
  
  var radgrad2 = ctx.createRadialGradient(105,105,20,112,120,50);
  radgrad2.addColorStop(0, '#FF5F98');
  radgrad2.addColorStop(0.75, '#FF0188');
  radgrad2.addColorStop(1, 'rgba(255,1,136,0)');

  var radgrad3 = ctx.createRadialGradient(95,15,15,102,20,40);
  radgrad3.addColorStop(0, '#00C9FF');
  radgrad3.addColorStop(0.8, '#00B5E2');
  radgrad3.addColorStop(1, 'rgba(0,201,255,0)');

  var radgrad4 = ctx.createRadialGradient(0,150,50,0,140,90);
  radgrad4.addColorStop(0, '#F4F201');
  radgrad4.addColorStop(0.8, '#E4C700');
  radgrad4.addColorStop(1, 'rgba(228,199,0,0)');
  
  // 画图形
  ctx.fillStyle = radgrad4;
  ctx.fillRect(0,0,150,150);
  ctx.fillStyle = radgrad3;
  ctx.fillRect(0,0,150,150);
  ctx.fillStyle = radgrad2;
  ctx.fillRect(0,0,150,150);
  ctx.fillStyle = radgrad;
  ctx.fillRect(0,0,150,150);
}
```

# 图案样式 Patterns

```javascript
createPattern(image, type);
// 接受2个参数。image 可以是一个 Image 对象的引用，或者另一个 canvas 对象。type 可以取值为：repeat, repeat-x, repeat-y, no-repeat。
```

创建一个图案然后赋给了 `fillStyle` 属性。唯一要注意的是，使用 Image 对象的 `onload` handler 来确保设置图案之前图像已经装载完毕。

```javascript
function draw() {
  var ctx = document.getElementById('canvas').getContext('2d');

  // 创建新 image 对象，用作图案
  var img = new Image();
  img.src = 'images/wallpaper.png';
  img.onload = function(){

    // 创建图案
    var ptrn = ctx.createPattern(img,'repeat');
    ctx.fillStyle = ptrn;
    ctx.fillRect(0,0,150,150);
  }
}
```

# 阴影 Shadows

```javascript
shadowOffsetX = float;
shadowOffsetX = float;
// 用来设定阴影在 X 和 Y 轴上的延伸距离，不受变换矩阵影响。
shadowBlur = float;
// 用于设定阴影的模糊程度，其数值并不跟像素数量挂钩，也不受变换矩阵的影响
shadowColor = color;
// 标准 CSS 颜色值，用与设定阴影颜色效果。
```

# Canvas 填充规则

当我们用到 fill 时，可以选择一个填充规则，该填充规则根据某处在路径的外面或者里面来决定该出是否被填充，这对于自己与自己路径相交或者路径被嵌套时是有用的。两个可能的值为：`nonzero` `evenodd` 。

这里我们用填充规则 `evenodd` ：

```javascript
function darw() {
  var ctx = document.getElementById('canvas').getContext('2d');
  ctx.beginPath();
  ctx.arc(50, 50, 30, 0, Math.PI*2, true);
  ctx.arc(50, 50, 15, 0, Math.PI*2, true);
  ctx.fill("evenodd");
}
```

