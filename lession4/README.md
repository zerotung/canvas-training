# 绘制文本

## 绘制文本

canvas 提供两种方法来渲染文本：

```javascript
fillText(text, x, y [, maxWidth]);
// 在指定的位置填充指定文本，绘制最大宽度可选
strokeText(text, x, y [, maxWidth]);
// 在指定的位置绘制文本边框，绘制最大宽度可选
```

## 有样式的文本

有更多属性可以让你改变 canvas 显示文本的方式：

```javascript
font = value; // 使用和 CSS font 属性相同的语法，默认 10px sans-serif
textAlign = value; // 可选 start, end, left, right or center，默认 start
textBaseline = value; // 可选 top, hanging, middle, alphabetic, ideographic, bottom，默认 alphabetic
direction = value; // 可选 ltr, rtl, inherit，默认 inherit
```

## 先进的文本测量

当你需要获得更多的文本细节时，下面的方法可以给你测量文本的方法。

```javascript
measureText() // 返回一个 TextMetrics 对象的宽度、所在像素
```

# 使用图像

canvas 可以用于动态的图像合成或者作为图形的背景，以及游戏界面等等。浏览器支持的任意格式的外部图片都可以使用，甚至将其他 canvas 元素生成的图片作为图片源。

引入图片到 canvas 需要以下两步基本操作：

1. 获得一个指向 HTMLImageElement 的对象或者另一个 canvas 元素的引用作为源，也可以通过提供一个 URL 的方法来使用图片
2. 使用 drawImage() 函数将图片绘制到画布上

## 获得需要绘制的图片

canvas 的 API 可以使用下面这些类型中的一种作为图片源：

```javascript
HTMLImageElement // 这些图片由 Image() 函数构造出来，或者任何的<img>元素
HTMLVideoElement // 用一个HTML的<video>元素作为图片源，可以从视频中抓取当前帧作为一个图像
HTMLCanvasElement // 可以使用另一个<canvas>元素作为你的图片源
ImageBitmap // 这是一个高性能的位图，可以低延迟的绘制，可以从上述源中生成
```

有几种方式可以获取到我们需要在 canvas 上使用的图片。

### 使用相同页面内的图片

我们可以通过下列方法的一种来获得与 canvas 相同页面内的图片的引用：

- document.images 集合
- `document.getElementsByTagName()` 或 `document.getElementById()`

### 使用其他域名下的图片

在 HTMLImageElement 上使用 crossOrigin 属性，可以请求加载其他域名上的图片。如果图片的服务器允许跨域访问这个图片，那么你可以使用这个图片而不污染 canvas，否则，使用这个图片将会污染 canvas。

### 使用其他 canvas 元素

和引用页面内的图片类似。但是引入的应该是已经准备好的 canvas。

一个常用的应用就是将第二个 canvas 作为另一个大的 canvas 的缩略图。

### 由零开始创建图像

我们可以用脚本创建一个新的 HTMLImageElement 对象。可以使用很方便的 `Image()` 构造函数。

```javascript
var img = new Image();   // 创建一个<img>元素
img.src = 'myImage.png'; // 设置图片源地址
```

当脚本执行后，图片开始装载。你应该用 load 时间来保证不会在加载完毕之前使用这个图片。

### 通过 data:url 方式嵌入图像

Data urls 允许用一串 Base64 编码的字符串的方式来定义一个图片。其优点就是图片内容即时可用，无须再访问服务器。（还有一个优点是，可以将 CSS，JavaScript，HTML 和图片全部封装在一起，迁移起来方便。）缺点是图像无法缓存，图片大的话内嵌 url 数据会很长。

### 使用视频帧

你还可以使用 `<video>` 中的视频帧。例如，如果你有一个 ID 为 myvideo 的 `<video>` 元素：

```javascript
function getMyVideo() {
  var canvas = document.getElementById('canvas');
  if (canvas.getContext) {
    var ctx = canvas.getContext('2d');
    
    return document.getElementById('myvideo');
  }
}
```

它将为这个视频返回 HTMLVideoElement 对象，作为我们的 Canvas 图片源。

## 绘制图片

一旦获得了源图对象，我们就可以使用 `drawImage` 方法将它渲染到 canvas 里。`drawImage` 方法有三种形态：

- `drawImage(image, x, y)` 
- `drawImage(image, x, y, width, height)` 
- `drawImage(image, sx, sy, sWidth, sHeight, dx, dy, dWidth, dHeight)` 

其中，image 是 image 或者 canvas 对象，x 和 y 是其在目标 canvas 里的起始坐标。width 和 height 用来控制画入 canvas 时缩放的大小。s* 定义图像源切片的位置和大小，d* 定义切片目标显示的位置和大小。

