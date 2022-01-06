---
title: canvas实现简易签字板
date: 2022-01-06 11:41:51
categories: canvas
tags:
- canvas
---

最近看到有些在线文档可以进行签名，以防以后项目中会用到，提前用canvas练习一下，先看下最后实现的效果：
![](https://gitee.com/huqian025/my-images/raw/master/demos/canvas%E5%AE%9E%E7%8E%B0%E7%AE%80%E6%98%93%E7%AD%BE%E5%AD%97%E6%9D%BF/%E7%AD%BE%E5%AD%97%E6%9D%BF%E6%95%88%E6%9E%9C.gif)

#### 思路
1. 在canva中监听鼠标按下事件（`mousedown`），获取按下时鼠标到canvas左上角的距离
2. 以鼠标按下的点为canvas画笔的起点，监听鼠标移动事件（`mousemove`），根据鼠标移动到的位置实时绘制线条
3. 监听鼠标弹起事件（`mouseup`），移除对mousemove事件的监听，防止鼠标弹起继续绘制

#### 搭建html结构
html结构比较简单，直接使用一个canvas标签，设置其背景色，宽度和高度。背景色是为了方便看效果，宽度默认300px，高度默认150px，设置宽高时不用带单位px
```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>简易签名板</title>
</head>

<body>
    <canvas style="background-color: skyblue;" id="canvas" width="400" height="200"></canvas>

    <script src="./canvas.js"></script>
</body>

</html>
```

#### 在canvas.js中实现功能
1. 获取canvas对象，获取2D画笔
```js
const canvas = document.querySelector('#canvas')
if (canvas.getContext) {
  const ctx = canvas.getContext('2d');
}
```
2. 根据鼠标移动绘制路径
```js
// 监听鼠标按下事件
canvas.onmousedown = function(mousedownEvent) {
  // 获取绘制起点，即鼠标点击到canvas元素左上角的距离
  const startX = mousedownEvent.offsetX;
  const startY = mousedownEvent.offsetY;
  ctx.beginPath();
  // 将画笔移动到起点
  ctx.moveTo(startX, startY);

  // 监听鼠标移动事件
  canvas.onmousemove = function(mousemoveEvent) {
    // 跟随鼠标移动来移动画笔
    ctx.lineTo(mousemoveEvent.offsetX, mousemoveEvent.offsetY);
    // 实时绘制
    ctx.stroke();
  }

  // 监听鼠标弹起事件
  canvas.onmouseup = function() {
    canvas.onmousemove = null
    // 闭合路径，使用之后线条起点和终点会自动连起来
    // ctx.closePath();
    // 鼠标弹起时再绘制
    // ctx.stroke();
  }

  // 监听鼠标移出canvas区域事件
  canvas.onmouseout = function() {
    // 鼠标移出时，停止对mousemove事件的监听，否则在移出时鼠标弹起事件监听失效
    canvas.onmousemove = null
  }
}
```
**注意点clientX/Y和offsetX/Y**
![](https://gitee.com/huqian025/my-images/raw/master/demos/canvas%E5%AE%9E%E7%8E%B0%E7%AE%80%E6%98%93%E7%AD%BE%E5%AD%97%E6%9D%BF/offsetX%E3%80%81clientX%E3%80%81screenX.png)

#### 扩展：将canvas转化为图片
* 方法一：使用[HTMLCanvasElement.toDataURL()](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLCanvasElement/toDataURL)获取canvas图片的dataURL
* 方法二：使用[HTMLCanvasElement.toBlob()](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLCanvasElement/toBlob)创建Blob对象，再使用[URL.createObjectURL()](https://developer.mozilla.org/zh-CN/docs/Web/API/URL/createObjectURL)获取指向该Blob对象的URL
```js
// 按钮点击事件
btn.onclick = function() {
  // 方法一
  const canvasData = canvas.toDataURL()
  const img = document.createElement('img')
  img.src=canvasData;
  body.appendChild(img)

  // 方法二
  // canvas.toBlob(function(blob) {
  //   var url = URL.createObjectURL(blob);
  //   const img = document.createElement('img')
  //   img.src=url;
  //   // 图片加载成功后清除url，避免占用内存
  //   img.onload = function() {
  //     URL.revokeObjectURL(url)
  //   };
  //   body.appendChild(img)
  // })
}
```
![](https://gitee.com/huqian025/my-images/raw/master/demos/canvas%E5%AE%9E%E7%8E%B0%E7%AE%80%E6%98%93%E7%AD%BE%E5%AD%97%E6%9D%BF/%E7%AD%BE%E5%AD%97%E6%9D%BF%E5%AF%BC%E5%87%BA%E4%B8%BA%E5%9B%BE%E7%89%87.gif)
