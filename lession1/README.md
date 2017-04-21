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