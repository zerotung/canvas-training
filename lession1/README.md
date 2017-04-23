#  Canvas 入门准备

## canvas 元素

`<canvas>` 也是 HTML 中的一个元素，可以给这个元素添加一些 HTML 属性，比如使用 width 和 height 来控制其大小，也可以通过 style 给它设置一些基本样式。同样也可以给它添加 id 名，在 JavaScript 中能更好的操作它。

要添加一个 canvas 只需要在 `<body>` 标签中添加一个 `<canvas>` 标签就行了，如下：

```html
<canvas id="myCanvas" width="400" height="400">
	Your browser dose not support HTML5 Canvas.
</canvas>
```

也可以通过 JavaScript 的 `document.createElement('canvas')` 创建一个 `<canvas>` ，并且使用 `document.body.appendChild(canvas)` 把创建的 canvas 插入到 body 中。

## 文档对象模型（DOM）

文档对象模型代表了在 HTML 页面上的所有对象。它是语言中产且平台中立的。它允许页面的内容和样式被 Web 浏览器渲染之后再次更新。用户可以通过 JavaScript 访问 DOM。

在开始使用 `<canvas>` 之前，需要先了解两个特定的 DOM 对象：window 和 document。

- window 对象是 DOM 的最高一级，需要对这个对象进行检查来确保开始使用 canvas 应用程序之前，已经加载了所有的资源和代码
- document 对象包含所有在 HTML 页面上的 HTML 元素。需要对这个对象进行检索来找出用 JavaScript 操作 canvas 的实例

也就是说，将 canvas 放入 Web 页面时，第一件事就是，检查整个页面是否已经加载，并且开始操作前是否所有 HTML 元素都已经展现。

我们可以通过监听 window 的 load 事件：

```javascript
window.addEventListener('load', function() {}, false);
```

## 引用 Canvas 元素

要给一个 canvas 进行操作就是需要一个 Canvas 对象的引用。可以在 `canvasApp()` 函数中先定义一个新变量，用于保存 Canvas 对象的引用。

```javascript
function canvasApp() {
  var myCanvas = document.getElementById('myCanvas');
}
```

### 检测浏览器是否支持 Canvas

在使用 Canvas 的 API 之前，可以先做一个浏览器检测。比如：

```javascript
function canvasApp() {
  var myCanvas = document.getElementById('myCanvas');
  if (!myCanvas || !myCanvas.getContext) { return; }
  // 这里开始绘制
}
```

为了提高代码阅读能力，可以将上面的代码进行一个封装。

```javascript
function canvasSupport(e) {
  return !!e.getcontext;
}
function canvasApp() {
  var myCanvas = document.getElementById('myCanvas');
  if (!canvasSupport(myCanvas)) { return; }
  // 这里开始绘制
}
```

### 获得 2D 环境

在使用 canvas 的相关 API，除了要获得 canvas 对象之外，还需要得到 2D 环境的引用，才能够操作它。HTML5 的 canvas 被设计可以与多个环境工作，包括一个建议的 3D 环境。不过我们先学的是 2D 环境。

要获取 canvas 中的 2D 环境，只需要通过 canvas 的 `getContext()` 方法即可，给这个方法传入一个 2d 的值。并将这个环境赋予给一个变量，比如 ctx：

```javascript
function canvasApp() {
  var myCanvas = document.getElementById('myCanvas');
  if(!canvasSupport(myCanvas)) { return; }
  var ctx = myCanvas.getContext('2d');
}
```

如此一来，我们就可以通过 HTML5 canvas 中的一些 API 进行一些操作。比如绘制一个矩形：

```javascript
function canvasApp() {
  var myCanvas = document.getElementById('myCanvas');
  if(!canvasSupport(myCanvas)) { return; }
  var ctx = myCanvas.getContext('2d');
  ctx.fillStyle = '#f36';
  ctx.fillRect(10, 10, 200, 200);
}
```

## 为 Canvas 封装 JavaScript 代码

canvas 功能是独占的，不太会受到页面上其他内容的影响，反之也是如此。如果想在同一个页面上放置多个 canvas，那么在定义 JavaScript 代码时必须将对应的代码分开。

为了避免出现这个问题，可以将变量和函数都封装在另一个函数中。比如前面代码中的 canvasApp() 函数包含整个 Canvas 应用程序。另外还可以封装一个 drawScreen() 函数，用来对 Canvas 应用进行操作。

```javascript
window.addEventListener('load', function() {canvasApp();}, false);

function canvasSupport(e) {
  return !!e.getContext;
}

function canvasApp() {
  var myCanvas = document.getElementById('myCanvas');
  if(!canvasSupport(myCanvas)) { return ; }
  var ctx = myCanvas.getContext('2d');
  function drawScreen() {
    // do something
  }
  drawScreen();
}
```

这样就对 Canvas 进行了封装，要绘制一个矩形，只需要在函数 `drawScreen()` 进行操作。

## Canvas 坐标系统

在 Canvas 中有 2D 和 3D 之分，可以通过 `getContext('2d')` 让 Canvas 得到一个 2D 环境。

在 Canvas 中 2D 环境中其坐标系统和 Web 坐标系统是一致的。坐标原点在 canvas 画布的左上角。分为 x 和 y 两个轴。

比如，我们在 `drawScreen()` 函数中绘制一个矩形

```javascript
function drawScreen() {
  ctx.fillStyle = '#f36';
  ctx.fillRect(15, 15, 20, 20);
}
```

在上面的代码中，我们再 canvas 画布中以坐标(15, 15)，绘制了一个宽高为 20px 的红色矩形。

canvas 除了默认的坐标系统之外，还有一个概念叫 **canvas 坐标系转换** 。这个概念在后面的课程中会专门介绍。

## 3D 坐标系统

3D 坐标系统多了一个 z 轴，用来描述深度。比如说一个物体在绘制时，在屏幕之类或之外多远的距离。

## 使用 Canvas 绘制一个坐标系统

前面介绍了Canvas中的坐标系统。那么我们来看看怎么使用 canvas 来绘制一个坐标系统。

```javascript
function drawScreen() {
  // 横线与竖线的间距
  var dx = 50; var dy = 50;
  
  // 初始坐标原点
  var x = 0; var y = 0;
  var w = myCanvas.width; var h = myCanvas.height;
  
  // 单个步长所表示的长度
  var xy = 10;
  
  ctx.lineWidth = 1;
  
  // 画横线
  while (y < h) {
    y = y + dy;
    ctx.moveTo(x, y);
    ctx.lineTo(w, y);
    ctx.stroke();
    
    // 横坐标的数字
    ctx.font = "1px Calibri";
    ctx.fillText(xy, x, y);
    xy += 10;
  }
  
  // 画竖线
  y = 0;
  xy = 10;
  while (x < w) {
    x = x + dx;
    ctx.moveTo(x, y);
    ctx.lineTo(x, h);
    ctx.stroke();
    
    // 纵坐标的数字
    ctx.font = "1px Calibri";
    ctx.fillText(xy, x, 10);
    xy += 10;
  }
}
```

