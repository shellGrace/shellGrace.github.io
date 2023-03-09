---
date: 2020-06-15
title: flutter UI
categories:
  - flutter
featured_image: '-'
---
### 文本字体
```css
font: 900 24px Georgia;

fontFamily: 'Georgia',
fontSize: 24,
fontWeight: FontWeight.bold,
```

### 背景色
```
background-color: #e0e0e0;

color: Colors.grey[300],
```
### 居中
```
display: flex;
align-items: center;
justify-content: center;

child: Center(
    child: Text(
      'Lorem ipsum',
      style: bold24Roboto,
    ),
  ),
```
### 容器宽度
对嵌套的 Container 来说，如果其父元素宽度小于子元素宽度，则子元素会调整尺寸以匹配父元素大小。
这句话的意思是 不需要像web那样单独设置 width:100% 。会自适应父元素

```
width: 100%;
max-width: 240px;

child: Container(
      width: 240,
 )
```
### 绝对定位
默认情况下，widget 相对于其父元素定位。
想要通过 x-y 坐标指定一个 widget 的绝对位置，可以把它放在一个 Positioned widget 中，而 Positioned 则需被放在一个 Stack widget 中。
```
position: relative; // 父元素
// 子
position: absolute; 
top: 24px;
left: 24px;

child: Stack(
    children: [
      Positioned(
        // red box
        left: 24,
        top: 24,
        )
```
### 旋转
想要旋转一个 widget，请将它放在 Transform widget 中。使用 Transform widget 的 alignment 和 origin 属性分别来指定转换原点（支点）的相对和绝对位置信息。

对于简单的 2D 旋转，例如在 Z 轴上旋转的，创建一个新的 Matrix4 对象，并使用它的 rotateZ() 方法以弧度单位（角度 × π / 180）指定旋转系数。
```
transform: rotate(15deg);

child: Transform(
      alignment: Alignment.center,
      transform: Matrix4.identity()..rotateZ(15 * 3.1415927 / 180),
      child: Container()
      )
```
### 缩放
想要缩放一个 widget，请同样将它放在一个 Transform widget 中。使用 Transform widget 的 alignment 和 origin 属性分别来指定缩放原点（支点）的相对和绝对信息。
对于沿 x 轴的简单缩放操作，新建一个 Matrix4 对象并用它的 scale() 方法来指定缩放因系数。
当你缩放一个父 widget 时，它的子 widget 也会相应被缩放。
```
transform: scale(1.5);

Transform(
      alignment: Alignment.center,
      transform: Matrix4.identity()..scale(1.5),
      child: Container()
      )
```
### 背景变换
想要将线性颜色渐变在 widget 的背景上应用，请将它嵌套在一个 Container widget 中。接着将一个 BoxDecoration 对象传递至 Container 的 decoration，然后使用 BoxDecoration 的 gradient 属性来变换背景填充内容。
变换「角度」基于 Alignment (x, y) 取值来定：
如果开始和结束的 x 值相同，变换将是垂直的 (0°180°)
如果开始和结束的 y 值相同，变换将是水平的 (90°270°)

垂直变换
```
background: linear-gradient(180deg, #ef5350, rgba(0, 0, 0, 0) 80%);

Container(
      // red box
      decoration: const BoxDecoration(
        gradient: LinearGradient(
          begin: Alignment.topCenter,
          end: Alignment(0.0, 0.6),
          colors: <Color>[
            Color(0xffef5350),
            Color(0x00ef5350),
          ],
        ),
      ),
   )
```
水平变换
```
background: linear-gradient(90deg, #ef5350, rgba(0, 0, 0, 0) 80%);

Container(
decoration: const BoxDecoration(
        gradient: LinearGradient(
          begin: Alignment(-1.0, 0.0),
          end: Alignment(0.6, 0.0),
          colors: <Color>[
            Color(0xffef5350),
            Color(0x00ef5350),
          ],
        ),
      ),
  )
```

### 圆角
想要在矩形上实现圆角，请用 BoxDecoration 对象的 borderRadius 属性。新建一个 BorderRadius 对象来指定各个圆角的半径大小。
```
border-radius: 8px;

decoration: BoxDecoration(
        color: Colors.red[400],
        borderRadius: const BorderRadius.all(
          Radius.circular(8),
        ), 
      ),
```
### box shadows
在 Flutter 中，每个属性与其取值都是单独指定的。请使用 BoxDecoration 的 boxShadow 属性来生成一系列 BoxShadow widget。你可以定义一个或多个 BoxShadow widget，这些 widget 共同用于设置阴影深度、颜色等
```
// css
box-shadow: 0 2px 4px rgba(0, 0, 0, 0.8),
              0 6px 20px rgba(0, 0, 0, 0.5);
            
 // flutter
 decoration: BoxDecoration(
        color: Colors.red[400],
        boxShadow: const <BoxShadow>[
          BoxShadow(
            color: Color(0xcc000000),
            offset: Offset(0, 2),
            blurRadius: 4,
          ),
          BoxShadow(
            color: Color(0x80000000),
            offset: Offset(0, 6),
            blurRadius: 20,
          ),
        ], 
      ),
```
### 圆
用 CSS 生成圆可以用一个变通方案，即将矩形的四边 border-radius 均设成 50%。
虽然 BoxDecoration 的 borderRadius 属性支持这样设置，但 Flutter 提供了一个 shape 属性用于实现同样的目的，它的类型是 BoxShape 枚举。
```
.red-circle {
    background-color: #ef5350; /* red 400 */
    padding: 16px;
    color: #ffffff;
    text-align: center;
    width: 160px;
    height: 160px;
    border-radius: 50%;
}

child: Container(
      // red circle
      decoration: BoxDecoration(
        color: Colors.red[400],
        shape: BoxShape.circle, 
      ),
      padding: const EdgeInsets.all(16),
      width: 160,
      height: 160,
     
      child: Text(
        'Lorem ipsum',
        style: bold24Roboto,
        textAlign: TextAlign.center, 
      ),
    ),
```
### 文字间距
```
letter-spacing: 4px;

child: const Text(
        'Lorem ipsum',
        style: TextStyle(
          color: Colors.white,
          fontSize: 24,
          fontWeight: FontWeight.w900,
          letterSpacing: 4, 
        ),
      ),
```
### 省略号处理溢出的文本
  ```
  overflow: hidden;
    display: -webkit-box;
    -webkit-box-orient: vertical;
    -webkit-line-clamp: 2;
    
    child: Text(
        'Lorem ipsum dolor sit amet, consec etur',
        style: bold24Roboto,
        overflow: TextOverflow.ellipsis,
        maxLines: 1, 
      ),
  ```
