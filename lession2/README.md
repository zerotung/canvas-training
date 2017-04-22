# Canvas 里的坐标系统

上一节我们做好了使用 Canvas 的准备工作，但并没有深入介绍如何使用 Canvas。在深入学习之前，我们有必要先了解 Canvas 里面的坐标系统。

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

