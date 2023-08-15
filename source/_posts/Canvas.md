---
title: Canvas学习
---
### 1. Canvas的基本使用

1. 使用`<canvas></canvas>`标签
2. 获取canvas的DOM对象
3. 使用canvas的DOMG对象创建一个画笔对象
4. 使用canvas提供的API绘画

```html
 // 1.
<canvas width="500" height="500" id="canvas"></canvas>
<script>
   // 2.
   const canvas = document.getElementById('canvas')
   // 3. 
   const context = canvas.getContext('2d')
   // 4. 
   // 画矩形
   // context.fillRect(100, 100, 200, 200)
   // 画直线
   // 起点
   context.moveTo(0,80)
   // 终点
   context.lineTo(500,100)
   // 开始绘制
   context.stroke()
</script>
```

### 2. canvas宽高的设置

canvas的宽高不能使用style来调节，只能使用属性

1. 标签

   ```html
   <canvas width="500" height="500"></canvas>
   ```

2. DOM

   ```js
   const canvas = document.getElementById('canvas')
   canvas.width = 500
   canvas.height = 500
   ```

### 3. 绘制直线

```js
const canvas = document.getElementById('canvas')
const context = canvas.getContext('2d')
// 起点
context.moveTo(0,80)
// 终点
context.lineTo(500,100)
// 开始绘制
context.stroke()
```

### 4. 绘制折线

折线就是在直线的基础上，设置多个转折的`lineTo()`

```js
const canvas = document.getElementById('canvas')
const context = canvas.getContext('2d')
// 起点
context.moveTo(100,100)
// 终点
context.lineTo(200,400)
context.lineTo(300,100)
context.lineTo(400,400)
// 开始绘制
context.stroke()
```

### 5. 设置线条宽度

设置 `lineWidth`属性即可

```js
context.lineWidth = 10
```

### 6. 修改线条颜色

设置`strokeStyle`属性

```js
context.strokeStyle = 'red'
```

#### 6.1 线性渐变

1. 使用 context 对象的`createLinearGradient()`方法来创建一个线性渐变对象

   ```js
   const grandient = context.createLinearGradient(x1, y1, x2, y2)
   ```

   > (x1,y1)为渐变起始位置,(x2,y2)为结束位置

2. 使用 grandient 对象的 `addColorStop()` 方法来设置渐变颜色

   ```js
   grandient.addColorStop(num,color)
   ```

   > num为渐变的区间,值为: 0~1,color为颜色

3. 赋值给 `context.strokeStyle`

   ```js
   context.strokeStyle = grandient
   ```

#### 6.2 径向渐变

1. 创建径向渐变对象

   ```js
   const grandient = context.createRadialGradient(x1, y1,z1,x2,y2,z2)
   ```

   > 径向渐变的参数是两个⚪的坐标,分别对相应渐变起始位置

2. 使用 grandient 对象的 `addColorStop()` 方法来设置渐变颜色

   ```js
   grandient.addColorStop(num,color)
   ```

   > num为渐变的区间,值为: 0~1,color为颜色

3. 赋值给 `context.fillStyle`

   ```js
   context.fillStyle = grandient
   ```

#### 6.3 锥形渐变

1. 创建锥形对象

   ```js
   const grandient = context.createConicGradient(startAngle,x,y)
   ```

   > 1. startAngle: 渐变的起始角度（弧度制）, 在使用时通常先转换为"角度制"
   >
   >    ```js
   >    const grandient = context.createConicGradient(startAngle*(Math.PI/180),0,0)
   >    ```
   >
   >    此时就转换为了角度
   >
   > 2. (x,y): 圆心的位置

2. 使用 grandient 对象的 `addColorStop()` 方法来设置渐变颜色

   ```js
   grandient.addColorStop(num,color)
   ```

   > num为渐变的区间,值为: 0~1,color为颜色

3. 赋值给 `context.fillStyle`

   ```js
   context.fillStyle = grandient
   ```

### 7. 绘制矩形

使用`fillRect() `方法绘制矩形

```js
const canvas = document.getElementById('canvas')
const context = canvas.getContext('2d')
// 绘制矩形
context.fillRect(x,y,width,height)
```

> x: 矩形左上角的 x 坐标
>
> y: 矩形左上角的 y 坐标
>
> width: 矩形的宽度，以像素计
>
> height: 矩形的高度，以像素计

### 8. 重复元图像

1. 创建 `Image` 对象

   ```js
   let image = new Image()
   image.src = './img.png'
   ```

2. 在图像**加载完成时**添加事件,创建元图像对象 

   ```js
   image.onload=function(){
       let p = context.createPattern(image,repeat)
   }
   ```

   > createPattern()方法有两个参数:
   >
   > 1. image:图像对象
   > 2. repeat有四种:
   >    - repeat: 重复
   >    - repeat-x: 只在x轴上重复
   >    - repeat-y: 只在y轴上重复
   >    - no-repeat: 不重复

3. 赋值给 `context.fillStyle`  

   ```js
   context.fillStyle = p
   ```

### 9. 绘制圆弧

1. 使用 `context` 对象的 `arc()` 方法

    ```js
    context.arc(x,y,radius,startAngle,endAngle,anticlockwise)
    ```

    > 1. (x,y):  弧线的圆心位置
    >
    > 2. radius：圆弧的半径
    >
    > 3. startAngle： 起始的角度（弧度制），需要转换为角度
    >
    > 4. endAngle： 结束的角度（弧度制），需要转换为角度
    >
    >    ```js
    >    angle*(Math.PI/180)
    >    ```
    >
    > 5. anticlockwise：是否顺时针绘制（默认为顺时针）
    >
    >    - true：逆时针
    >    - false：顺时针

2. 调用`context.stroke()`方法进行绘制

   ```js
   context.stroke()
   ```

### 10. 设置断点

> 在通常情况下，想同时绘制多个图形时，会发现他们是连在一起的，所以需要告诉`context` 重新开始一个新的路径来绘制新图案

1. 开始路径

   ```js
   context.beginPath()
   ```

2. 结束路径

   ```js
   context.closePath()
   ```

### 11. 绘制椭圆

1. 使用`context` 对象的`ellipse()` 方法

   ```js
   context.ellipse(x,y,radiusX,radiusY，rotation,startAngle,endAngle,anticlockwise)
   ```

   > 1. (x,y):  椭圆的圆心位置
   >
   > 2. radiusX：X轴的半径
   >
   > 3. radiusY：Y轴的半径
   >
   > 4. startAngle： 起始的角度（弧度制），需要转换为角度
   >
   > 5. endAngle： 结束的角度（弧度制），需要转换为角度
   >
   >    ```js
   >    angle*(Math.PI/180)
   >    ```
   >
   > 6. rotation：椭圆旋转的角度（弧度制，需要转换）
   >
   > 7. anticlockwise：是否顺时针绘制（默认为顺时针）
   >
   >    - true：逆时针
   >    - false：顺时针

2. 调用`context.stroke()`方法进行绘制

   ```js
   context.stroke()
   ```

### 12. 填充

> 在调用`fill()`方法的基础上， 使用`context` 对象的`fill()` 方法，能够填充我们画的图形

#### 12.1 基本使用

```js
context.fill()
```

​	**注意：**特别是在画线的时，会直接使用直线把起始和末尾点链接起来

#### 12.2 填充的样式设置

使用`context`对象的`fillStyle`属性，可以设置颜色

```js
context.fillStyle = 'blue'
```

**注意**：在使用`fill()`前设置`fillStyle`

### 13. 图形阴影

1. 设置阴影的颜色

   ```js
   context.shadowColor = 'black'
   ```

   > shadowColor属性用于控制阴影颜色

2. 设置阴影的偏移量

   ```js
   context.shadowOffsetX
   ```

   > 阴影在X轴上的偏移量：
   >
   > - 正数：向右偏移
   > - 负数：向左偏移

   ```js
   context.shadowOffsetY
   ```

   > 阴影在Y轴上的偏移量：
   >
   > - 正数：向下偏移
   > - 负数：向上偏移

3. 设置阴影模糊度

   ```js
   context.shadowBlur = 20 
   ```

   > 数值越大越模糊

### 14. 贝塞尔曲线

- 二阶贝塞尔曲线

  1. 设置起点

     ```js
     context.moveTo(100,100)
     ```

  2. 设置二阶贝塞尔曲线路径：quadraticCurveTo

     ```js
     context.quadraticCurveTo(cpx,cpy,x,y)
     ```

     > (cpx,cpy)：控制点的坐标
     >
     > (x,y)：终点的坐标

  3. 绘制

     ```js
     context.stroke()
     ```

     **起点、控制点、终点**绘制成二阶贝塞尔曲线

- 三阶贝塞尔曲线

  > 三阶贝塞尔曲线比二阶的多一个控制点

  1. 设置起点

     ```js
     context.moveTo(100,100)
     ```

  2. 设置三阶贝塞尔曲线路径：bezierCurveTo

     ```js
     context.bezierCurveTo(cp1x,cp1y,cp2x,cp2y，x,y)
     ```

     > (cp1x,cp1y)：控制点的坐标
     >
     > (cp2x,cp2y)：控制点的坐标
     >
     > (x,y)：终点的坐标

  3. 绘制

     ```js
     context.stroke()
     ```

     **起点、控制点1、控制点2、终点**绘制成二阶贝塞尔曲线

### 15. 绘制图片

1. 创建图片对象

   ```js
   let img = new Image()
   img.src = './xxx'
   ```

2. 在**图片加载完成**时添加事件

   ```js
   img.onload = function(){
       context.drawImage()
   }
   ```

   调用 `context` 对象的`drawImage()`方法有两种情况：

   1. `context.drawImage(img,x,y,w,h)`

       > 1. img：创建的图片对象
       > 2. (x,y)：图片左上角的起始坐标
       > 3. w、h：图片显示的宽高，**如果没有传入则按照图片原大小显示**
       
   2. `context.drawImage(img,sx,sy,sw,sh,dx,dy,dw,dh)`
   
       > 该情况用于**裁剪图片**
       >
       > 1. img：创建的图片对象
       > 2. (sx,sy)：图片裁剪的起始坐标
       > 3. sw、sh：裁剪的宽、高
       > 4. (dx,dy)：裁剪好的图片渲染的左上角的起始坐标
       > 5. dw、dh：裁剪后渲染的图片的宽高，可控制图片的大小

### 16. 绘制文字

#### 16.1 绘制文字的两个API

1. `fillText(content,x,y,[maxwiedth])`：

   该API绘制的文字是实心填充的

   > 1. content：文字内容
   > 2. (x,y)：距离左上角的初始坐标
   > 3. maxwidth：文字显示的最大宽度（可选）

2. `strokeText(content,x,y,[maxwidth])`

   该API绘制的文字是由线绘制的，没有填充

   > 1. content：文字内容
   > 2. (x,y)：距离左上角的初始坐标
   > 3. maxwidth：文字显示的最大宽度（可选）

#### 16.2 调节文字样式

使用`context`对象的`font`属性

```js
context.font = value
```

> value是string类型的参数，其规范和CSS的font样式一样
>
> 默认值为：'10px sans-serif'
>
> 1. 必须包含两个参数：
>    - font-size：字体大小
>    - font-family：字体样式
> 2. 可选参数有：
>    - font-style：在`font-family`样式下字体样式
>      - normal：常规
>      - italic：斜体
>      - oblique：斜体的基础上可以设置倾斜角度
>    - font-variant：
>    - font-weight：字体粗细程度
>    - line-height：行高

**注意：**

- `font-style`, `font-variant` 和 `font-weight` 必须在 `font-size` 之前
- `line-height` 必须跟在 `font-size` 后面，由 "/" 分隔，例如 "`16px/3`"
- `font-family` 必须最后指定

#### 16.3 文字的颜色

使用`context`对象两个属性：

1. `fillStyle`

   ```js
   context.fillStyle = 'red'
   ```

2. `strokeStyle`

   ```js
   context.strokeStyle = 'red'
   ```

也可以设置渐变

```js
const g = context.createLinearGradient(0,0,100,0)
g.addColorStop(0,'red')
g.addColorStop(0.25,'orange')
g.addColorStop(0.5,'yellow')
g.addColorStop(0.75,'green')
g.addColorStop(1,'pink')
context.font = '50px Arial red'
context.fillStyle = g
context.strokeStyle = g
```

#### 17. 滤镜

使用`context`对象的`filter`属性

```js
context.filter = value
```

> value有一下选值：
>
> 1. none：默认值，没有滤镜
> 2. blur(num)：模糊，num可设置模糊程度
> 3. brightness(num)：亮度，num为百分数%
> 4. contrast(num)：对比度，num为百分数%
> 5. grayscale(num)：灰度，num为百分数%
> 6. sepia(num)：褐度，num为百分数%
> 7. hue-rotate(num)：颜色旋转，num为角度deg
> 8. invert(num)：颜色反向（黑白反转），num为百分数%
> 9. opacity(num)：透明度，num为百分数%
> 10. saturate(num)：饱和度，num为百分数%

### 18. 变换效果

所有的变换指的是**画笔原点**的变换

#### 18.1 translate位移

```js
context.translate(x,y)
```

> x：在X轴上位移的距离
>
> y：在Y轴上位移的距离

位移的变换是基于**原点**的变换，默认原点是（0,0），左上角

#### 18.2 rotate旋转

```js
context.rotate(angle)
```

> angle：旋转的角度，为弧度制，在使用时可以转换

```js
context.rotate(45*(Math.PI/180)) // 旋转45°
```

#### 18.3 scale缩放

```js
context.scale(x,y)
```

> x：在X方向上的缩放，1为默认值，不缩放
>
> y：在Y方向上的缩放，1为默认值，不缩放

#### 18.4 transform方法

`transform()`方法是上述三个方法的结合体

```js
context.transform(a,b,c,d,e,f)
```

> a：水平缩放
>
> b：垂直倾斜
>
> c：水平倾斜
>
> d：垂直缩放
>
> e：水平移动
>
> f：垂直移动

### 19. 绘制动画

> canvas动画实现利用了循环机制，过一段时间切换一帧，从而实现动画

浏览器提供了一种循环API`requestAnimationFrame()`

```js
requestAnimationFrame(fn)
```

> 该方法传入参数为：方法名

使用：

```js
// 1. 创建一个方法
function loop(){
    console.log(1)
    // 3. 在函数内部递归调用requestAnimationFrame，实现循环
	requestAnimationFrame(loop)
}
// 2. 在外层使用requestAnimationFrame，传入该方法
requestAnimationFrame(loop)
```

则通过以上循环机制，我们可以实现一个简单的动画：将一个矩形进行水平移动

```html
<!-- 1. 创建canvas -->
<canvas width="500" height="500" id="canvas"></canvas>
<script>
    <!-- 2. 获取canvas实例 -->
    const canvas = document.getElementById('canvas')
    <!-- 3. 创建画布 -->
    const context = canvas.getContext('2d')
    <!-- 4. i控制矩形的移动 -->
    let i = 0
    <!-- 循环 -->
    function loop () {
        if(i > 400){
            i = 0
        }
        i++
        <!-- 6. 每次移动过后清除上一次画的矩形 -->
        context.clearRect(0,0,canvas.width,canvas.height)
        <!-- 5. 画矩形，起始位置用i控制 -->
        context.fillRect(i, 0, 100, 100)
        requestAnimationFrame(loop)
    }
    requestAnimationFrame(loop)
</script>
```

### 20. 星球转动动画实战

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=, initial-scale=1.0">
  <title>Document</title>
  <style>
    canvas {
      background-color: rgba(0, 0, 0, 0.7);
      margin: auto;
      display: block;
    }
  </style>
</head>

<body>

  <canvas width="600" height="500" id="canvas"></canvas>
  <script>
    const canvas = document.getElementById('canvas')
    const context = canvas.getContext('2d')
    let i = 0
    function loop () {
      context.save()
      context.clearRect(0, 0, 600, 500)
      if (i > 360) {
        i = 0
      }
      i++
      // 太阳
      context.beginPath()
      context.arc(300, 250, 60, 0, 2 * Math.PI)
      context.fillStyle = 'yellow'
      context.shadowColor = 'yellow'
      context.shadowBlur = 20
      context.fill()
      context.closePath()

      // 地球
      context.beginPath()
      // 将画笔移到正中间
      context.translate(300, 250)
      context.rotate(i * Math.PI / 180)
      context.arc(200, 0, 12, 0, 2 * Math.PI)
      context.fillStyle = 'blue'
      context.shadowColor = 'blue'
      context.shadowBlur = 5
      context.fill()
      context.closePath()

      // 月亮
      context.beginPath()
      // 将画笔移到正中间
      context.translate(200, 0)
      context.rotate(i * Math.PI / 180)
      context.arc(50, 0, 6, 0, 2 * Math.PI)
      context.fillStyle = 'white'
      context.shadowColor = 'white'
      context.shadowBlur = 2
      context.fill()
      context.closePath()

      context.restore()
      requestAnimationFrame(loop)
    }
    requestAnimationFrame(loop)
  </script>
</body>

</html>
```

