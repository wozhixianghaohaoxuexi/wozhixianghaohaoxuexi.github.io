---
title: bootstrap响应式布局
date: 2022-02-14 11:35:28
categories:
  - css
tags:
  - bootstrap
---

### 一、响应式开发

#### 1、响应式开发原理

* 就是使用媒体查询针对不同宽度的设备进行布局和样式的设置，从而适配不同的设备

![响应式开发原理](https://gitee.com/huqian025/my-images/raw/master/css/bootstrap响应式布局/响应式开发原理.png)

#### 2、响应式布局容器

响应式需要一个父级作为布局容器，来配合子级元素来实现变化效果。原理就是在不同屏幕下，通过媒体查询来改变布局容器的大小，再改变里面子元素的排列方式和大小，从而实现不同屏幕下，看到不同的页面布局和样式变化

![响应式布局容器](https://gitee.com/huqian025/my-images/raw/master/css/bootstrap响应式布局/响应式布局容器.png)

### 二、bootstrap前端框架

#### 1、bootstrap简介

bootstrap来自twitter，是基于html、css和javascript的前端框架，它简洁灵活，使得web开发更加快捷

![bootstrap简介_1](https://gitee.com/huqian025/my-images/raw/master/css/bootstrap响应式布局/bootstrap简介_1.png)

![bootstrap简介_2](https://gitee.com/huqian025/my-images/raw/master/css/bootstrap响应式布局/bootstrap简介_2.png)

#### 2、bootstrap基本使用

bootstrap使用四部曲：

1. 创建文件夹结构
   ![bootstrap_1](https://gitee.com/huqian025/my-images/raw/master/css/bootstrap响应式布局/bootstrap_1.png)

2. 创建html骨架结构
   ![bootstrap_2](https://gitee.com/huqian025/my-images/raw/master/css/bootstrap响应式布局/bootstrap_2.png)

```html
<meta charset="utf-8">
<meta http-equiv="X-UA-Compatible" content="IE=edge">
<meta name="viewport" content="width=device-width, initial-scale=1">
<!-- 上述3个meta标签*必须*放在最前面，任何其他内容都*必须*跟随其后！ -->
<title>Bootstrap 101 Template</title>

<!-- Bootstrap -->
<link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/3.4.1/css/bootstrap.min.css" integrity="sha384-HSMxcRTRxnN+Bdg0JdbxYKrThecOKuH5zCYotlSAcp1+c8xmyTe9GYg1l9a69psu" crossorigin="anonymous">

<!-- HTML5 shim 和 Respond.js 是为了让 IE8 支持 HTML5 元素和媒体查询（media queries）功能 -->
<!-- 警告：通过 file:// 协议（就是直接将 html 页面拖拽到浏览器中）访问页面时 Respond.js 不起作用 -->
<!--[if lt IE 9]>
  <script src="https://cdn.jsdelivr.net/npm/html5shiv@3.7.3/dist/html5shiv.min.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/respond.js@1.4.2/dest/respond.min.js"></script>
<![endif]-->
```

3. 引入相关样式文件
   ![bootstrap_3](https://gitee.com/huqian025/my-images/raw/master/css/bootstrap响应式布局/bootstrap_3.png)

4. 书写内容
   ![bootstrap_4](https://gitee.com/huqian025/my-images/raw/master/css/bootstrap响应式布局/bootstrap_4.png)

#### 3、布局容器

bootstrap需要为页面内容和栅格系统包裹一个.container容器，bootstrap预先定义好了这个类，叫.container

![bootstrap布局容器](https://gitee.com/huqian025/my-images/raw/master/css/bootstrap响应式布局/bootstrap布局容器.png)

### 三、bootstrap栅格系统

#### 1、栅格系统简介

* 栅格系统是指将页面布局划分为等宽的列，然后通过列数的定义来模块化页面布局。
* bootstrap提供了一套响应式、移动设备优先的流式栅格系统，随着屏幕或视口（viewport）尺寸的增加，系统自动分为12列
* bootstrap里面的container宽度是固定的，但是不同屏幕下，container的宽度不同，我们再把container划分为12等份

#### 2、栅格选项参数

![栅格选项参数](https://gitee.com/huqian025/my-images/raw/master/css/bootstrap响应式布局/栅格选项参数.png)

* 每一列默认有左右15px的padding
* 如果孩子的份数相加等于12，则孩子能占满整个container的宽度
* 如果孩子的份数相加小于12，则不会占满整个container的宽度，会有空白
* 如果孩子的份数相加大于12，则超出的那一列会另起一行显示

![栅格使用示例](https://gitee.com/huqian025/my-images/raw/master/css/bootstrap响应式布局/栅格使用示例.png)

#### 3、列嵌套

栅格系统内置的栅格系统将内容再次嵌套，简单理解就是一个列内在分成若干份小列

![列嵌套](https://gitee.com/huqian025/my-images/raw/master/css/bootstrap响应式布局/列嵌套.png)

* 列嵌套最好加一行row，这样可以取消父元素的padding值，而且高度自动和父级一样高

![列嵌套示例](https://gitee.com/huqian025/my-images/raw/master/css/bootstrap响应式布局/列嵌套示例.png)

#### 4、列偏移

列偏移使用.col-md-offset-\*类可以将列向右偏移，这些类实际是为当前元素添加了左侧的边距（margin）

![列偏移示例](https://gitee.com/huqian025/my-images/raw/master/css/bootstrap响应式布局/列偏移示例.png)

![列偏移示例效果](https://gitee.com/huqian025/my-images/raw/master/css/bootstrap响应式布局/列偏移示例效果.png)

#### 5、列排序

通过使用.col-md-push-\*和.col-md-pull-\*类就可以很容易的改变列的顺序

![列排序](https://gitee.com/huqian025/my-images/raw/master/css/bootstrap响应式布局/列排序.png)

#### 6、响应式工具

为了加快对移动设备友好的页面开发工作，利用媒体查询功能，并使用这些工具类可以方便的针对不同的设备展示或隐藏页面内容

![响应式工具](https://gitee.com/huqian025/my-images/raw/master/css/bootstrap响应式布局/响应式工具.png)

与之相反的是visible-xs visible-sm visible-md visible-lg是显示某个页面内容
