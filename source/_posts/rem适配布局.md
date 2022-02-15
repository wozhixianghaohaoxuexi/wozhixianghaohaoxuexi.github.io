---
title: rem适配布局
date: 2022-02-14 11:35:20
categories:
  - css
tags:
  - rem
  - 媒体查询
  - less
---

### 一、rem基础

* rem（root em）是一个相对单位，类似与em，em是父元素字体大小，rem是html元素字体大小
    * 比如，根元素（html）设置font-size=12px，非根元素设置width: 2rem; 则换成px表示就是24px
* rem的优点就是可以通过修改html里面的文字大小来改变页面中元素的大小，可以整体控制

### 二、媒体查询

#### 1、什么是媒体查询

* 媒体查询（Media Query）是css3新语法，使用@media查询，可以针对不同的媒体类型定义不同的样式
* `@media可以针对不同的屏幕尺寸设置不同的样式`
* 当你重置浏览器大小的过程中，页面也会根据浏览器的宽度和高度重新渲染页面

#### 2、语法规范
```css
@media mediatype and|not|only (media feature) {
    css-code;
}
```

* 用@media开头，注意@符号
* mediatype 媒体类型

![媒体类型](https://gitee.com/huqian025/my-images/raw/master/css/rem适配布局/媒体类型.png)

* 关键字 and not only
* media feature 媒体特性，必须有小括号包含

![媒体特性](https://gitee.com/huqian025/my-images/raw/master/css/rem适配布局/媒体特性.png)

#### 3、媒体查询+rem实现元素动态大小变化

* rem单位是跟着html走的，有了rem页面元素可以设置不同大小尺寸
* 媒体查询可以根据不同设备宽度来修改样式
* 媒体查询+rem就可以实现不同设备宽度，页面元素大小的动态变化
```css
@media screen and (min-width: 320px) {
    html {
        font-size: 50px;
    }
}

@media screen and (min-width: 640px) {
    html {
        font-size: 100px;
    }
}

.top {
    height: 1rem;
    font-size: .5rem;
    background-color: green;
    color: #fff;
    text-align: center;
    line-height: 1rem;
}
```

#### 4、引入资源

* 当样式比较繁多的时候，我们可以针对不同的媒体使用不同的stylesheets（样式表）
* 原理：在link中判断设备的尺寸，然后引入不同的css文件
```html
<!-- 建议媒体查询从小到大 -->
<!-- 针对不同的屏幕尺寸，调用不同的css文件 -->
<link rel="stylesheet" href="style320.css" media="screen and (min-width: 320px)">
<link rel="stylesheet" href="style640.css" media="screen and (min-width: 640px)">
```

### 三、less基础

#### 1、css弊端

css是一门非程序式语言，没有变量、函数、SCOPE（作用域）等概念

* css需要书写大量看似没有逻辑的代码，css冗余度是比较高的
* 不方便维护及扩展，不利于复用
* css没有很好的计算能力
* 非前端开发工程师来讲，往往因为缺少css编写经验而很难写出组织良好且易于维护的css代码项目

#### 2、less介绍

Less（Leaner Style Sheets）是一门css预处理语言，它扩展了css的动态特性

#### 3、less使用

首先新建一个后缀名为.less的文件，在这个less文件里面书写less语句

##### 3.1、less变量

`@变量名: 变量值;`

变量命名规范：

* 必须有@为前缀

* 不能包含特殊字符
* 不能以数字开头
* 大小写敏感

![less变量](https://gitee.com/huqian025/my-images/raw/master/css/rem适配布局/less变量.png)

##### 3.2、less编译

* 方法一：vsCode安装easy less插件，保存less会自动编译为同目录下同名css
* 方法二：通过npm安装less（`全局：npm install less -g，局部：npm install less --save-dev` ），编译`lessc less文件名 css文件名`（例如：`lessc bootstrap.less bootstrap.css`）

##### 3.3、less嵌套

* 子元素的样式直接写在父元素里面
* 如果有伪类、交集选择器、伪元素选择器，内层选择器的前面需要加&
* 内层选择器的前面没有加&符号，则它被解析为父选择器的后代

![less嵌套](https://gitee.com/huqian025/my-images/raw/master/css/rem适配布局/less嵌套.png)

##### 3.4、less运算

任何数字、颜色或者变量都可以参与运算，less提供了加（+）、减（-）、乘（\*）、除（/）算术运算

![less运算](https://gitee.com/huqian025/my-images/raw/master/css/rem适配布局/less运算.png)

**注意：**
1. 运算符的左右两侧必须敲一个空格隔开
2. 两个数参与运算，如果只有一个数有单位，则最后的结果就以这个单位为准
3. 两个数参与运算，如果2个数都有单位，而且它们的单位不一样，最后的结果以第一个单位为准

### 四、rem适配方案

#### 1、rem实际开发适配方案

1. 按照设计稿与设备宽度的比例，动态计算并设置html根标签的font-size大小（媒体查询）
2. css中，设计稿元素的宽、高、相对位置等取值，按照同等比例换算成rem为单位的值

#### 2、rem适配方案技术使用（市场主流）

![rem适配方案技术使用](https://gitee.com/huqian025/my-images/raw/master/css/rem适配布局/rem适配方案技术使用.png)

#### 3、rem适配方案1（rem+媒体查询+less）

##### 3.1、设计稿常见尺寸宽度

![设计稿常见尺寸宽度](https://gitee.com/huqian025/my-images/raw/master/css/rem适配布局/设计稿常见尺寸宽度.png)

##### 3.2、动态设置html标签font-size大小

![动态设置html标签font-size](https://gitee.com/huqian025/my-images/raw/master/css/rem适配布局/动态设置html标签font-size.png)

##### 3.3、元素大小取值方法

* 最后的公式：页面元素的rem值=页面元素值（px）/（屏幕宽度 / 划分的份数）
* 屏幕宽度 / 划分的份数，就是html的font-size大小，页面元素的rem值=页面元素值（px）/ html的font-size字体大小

#### 4、rem适配方案2（flexible.js+rem）

![flexible.js+rem](https://gitee.com/huqian025/my-images/raw/master/css/rem适配布局/flexible.js+rem.png)

vsCode px转换rem插件（cssrem）

* 默认html的font-size为16px
* 在setting.json中添加`"cssrem.rootFontSize": 75`，将font-size改为75px，重启vscode
