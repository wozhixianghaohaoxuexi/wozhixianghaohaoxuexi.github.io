---
title: flex弹性布局
date: 2022-02-14 11:25:01
categories:
  - css
tags:
  - 布局
---

####  一、容器属性

##### 1. display：是否采用flex布局

* flex：指定容器采用flex布局，容器元素默认为块级元素
* inline-flex：指定容器采用flex布局，容器元素为行内元素

##### 2. flex-direction：主轴的方向，及项目的排列方向

* row（默认值）：主轴为水平方向，起点在左端
* row-reverse：主轴为水平方向，起点在右端
* column：主轴为垂直方向，起点在上沿
* column-reverse：主轴为垂直方向，起点在下沿

##### 3. flex-wrap：项目如何换行

* nowrap（默认）：不换行
* wrap：换行，第一行在上方
* wrap-reverse：换行，第一行在下方

##### 4. flex-flow：flex-direction，flex-wrap属性的简写

* 例如：flex-flow: row wrap;

##### 5. justify-content：主轴上的对齐方式

* flex-start（默认值）：左对齐
* flex-end：右对齐
* center： 居中
* space-between：两端对齐，项目之间的间隔都相等
* space-around：每个项目两侧的间隔相等。所以，项目之间的间隔比项目与边框的间隔大一倍
* space-evenly：主轴所有间隔相等。项目之间、项目与边框之间的间隔都相等

##### 6. align-items：项目在交叉轴上的对齐方式

* flex-start：交叉轴的起点对齐
* flex-end：交叉轴的终点对齐
* center：交叉轴的中点对齐
* baseline: 项目的第一行文字的基线对齐
* stretch（默认值）：如果项目未设置高度或设为auto，将占满整个容器的高度
* normal：在flex布局中，效果和stretch一样

##### 7. align-content：决定多行的flex-items在交叉轴上的对齐方式，即多根轴线的对齐方式（如果项目只有一根轴线，该属性不起作用）

* flex-start：与交叉轴的起点对齐
* flex-end：与交叉轴的终点对齐
* center：与交叉轴的中点对齐
* space-between：与交叉轴两端对齐，轴线之间的间隔平均分布
* space-around：每根轴线两侧的间隔都相等。所以，轴线之间的间隔比轴线与边框的间隔大一倍
* space-evenly：主轴所有间隔相等。项目之间、项目与边框之间的间隔都相等
* stretch（默认值）：轴线占满整个交叉轴

##### 8. align-items 和 align-content 区别

* 都是设置侧轴（交叉轴，非主轴）上的对齐方式
* align-items适用于单行情况下，只有上对齐、下对齐、居中和拉伸
* align-content适用于换行（多行）的情况下（单行情况无效），可以设置上对齐、下对齐、居中、拉伸以及平均分配剩余空间等属性
* 总结就是单行找align-items，多行找align-content

#### 二、项目属性

##### 1. order：项目的排列顺序

* order数值越小，排列越靠前，默认为0，-1比0小

##### 2. flex-grow：项目的放大比例

* 默认为0，即如果存在剩余空间也不放大，可以设置任意非负数字（正小数、正整数、0）。当flex-container在max axis方向上有剩余size时，flex-grow属性才会生效。flex-items扩展后的最终size不能超过max-width/max-height
* 如果所有flex-items的flex-grow总和sum超过1，每个flex-items扩展的size为：flex-container的剩余size * flex-grow / sum
* 如果所有flex-items的flex-grow总和sum不超过1，每个flex-items扩展的size为：flex-container的剩余size * flex-grow

##### 3. flex-shrink：项目的缩小比例

* 默认为1，即如果空间不足，将该项目缩小，可以设置任意非负数字（正小数、正整数、0）。当flex-items在max axis方向上超过了flex-container的size，flex-shrink属性才会生效。flex-items收缩后的最终size不能小于min-width/min-height
* 如果所有flex-items的flex-shrink总和sum超过1，每个flex-items收缩的size为：flex-items超出flex-container的size * flex-shrink / sum
* 如果所有flex-items的flex-shrink总和sum不超过1，每个flex-items收缩的size为：flex-items超出flex-container的size * flex-shrink

##### 4. flex-basis：在分配多余空间之前，项目占据的主轴空间

* 浏览器根据这个属性，计算主轴是否有多余空间，默认值为auto，即项目本来的大小
* 决定flex-items最终base size的因素，从优先级高到低：max-width/max-height/min-width/min-height -> flex-basis -> width/height -> 内容本身的size

##### 5. flex：flex-grow，flex-shrink，flex-basis属性的简写

* 默认值为0 1 auto，后两个属性可选
* 该属性有两个快捷值：auto -> 1 1 auto，none -> 0 0 auto
* 优先使用该属性，而不是单独写三个分离的属性，因为浏览器会推算相关值

##### 6. align-self：单个项目与其他项目不一样的对齐方式，可覆盖align-items属性

* 默认值为auto，表示继承父元素的align-items属性，如果没有父元素，则等同于stretch
* 该属性可能取6个值，除了auto，其他都与align-items属性完全一致


> 参考文档：[阮一峰老师的flex布局教程](https://www.ruanyifeng.com/blog/2015/07/flex-grammar.html)
