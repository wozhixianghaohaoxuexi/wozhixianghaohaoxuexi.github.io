---
title: css3新特性
date: 2022-02-14 11:34:59
categories:
  - css
tags:
  - css3
---

### 一、新增选择器

#### 1、属性选择器

属性选择器可以根据元素特定的属性来选择元素。这样就可以不用借助于类或者id选择器

![属性选择器](https://gitee.com/huqian025/my-images/raw/master/css/css3新特性/属性选择器.png)

**注意：类选择器、属性选择器、伪类选择器，权重为10**

![选择器权重](https://gitee.com/huqian025/my-images/raw/master/css/css3新特性/选择器权重.png)

#### 2、结构伪类选择器

结构伪类选择器主要是根据文档结构来选择元素，常用于根据父级选择器里面的子元素

![结构伪类选择器](https://gitee.com/huqian025/my-images/raw/master/css/css3新特性/结构伪类选择器.png)
**[注意：:last-child CSS伪类代表父元素的最后一个子元素。如.item:last-child,选中并不是指.item类中的最后一个元素, 而是既是.item又是父元素的最后一个子元素的元素](https://segmentfault.com/a/1190000020109948)**

* nth-child(n)选择某个父元素的一个或多个特定的子元素（括号内的参数必须为n，不能用a...）
    * n可以是数字，关键字和公式
    * n如果是数字，就选择第n个元素，里面的数字从1开始
    * n可以是关键字：even偶数，odd奇数
    * n可以是公式：常见的公式如下（如果n是公式，则从0开始计算，但是第0个元素或者超出了元素的个数会被忽略）

![nth-child(n)](https://gitee.com/huqian025/my-images/raw/master/css/css3新特性/nth-child(n).png)
![nth-child权重](https://gitee.com/huqian025/my-images/raw/master/css/css3新特性/nth-child权重.png)

* nth-child(n)和nth-of-type(n)的区别：nth-child先找第n个孩子，再判断其类型。nth-of-type先选出满足类型的孩子，再找第n个孩子

![nth-child示例](https://gitee.com/huqian025/my-images/raw/master/css/css3新特性/nth-child示例.png)

#### 3、伪元素选择器（重点）

伪元素选择器可以帮助我们利用css创建新标签元素，而不需要html标签，从而简化html结构

![伪元素选择器](https://gitee.com/huqian025/my-images/raw/master/css/css3新特性/伪元素选择器.png)

* 注意：
    * before和after创建一个元素，但是属于行内元素
    * 新创建的这个元素在文档树中是找不到的，所以我们称为伪元素
    * 语法：`element::before{}`
    * before和after必须有content属性
    * before在父元素内容的前面创建元素，after在父元素内容的后面插入元素
    * 伪元素选择器和标签选择器一样，权重为1

![before和after伪元素](https://gitee.com/huqian025/my-images/raw/master/css/css3新特性/before和after伪元素.png)

##### 3.1、使用场景1：配合字体图标

* 实现效果：

![伪元素使用场景1](https://gitee.com/huqian025/my-images/raw/master/css/css3新特性/伪元素使用场景1.png)

* 实现方法：

![伪元素使用场景1_2](https://gitee.com/huqian025/my-images/raw/master/css/css3新特性/伪元素使用场景1_2.png)
![伪元素使用场景1_3](https://gitee.com/huqian025/my-images/raw/master/css/css3新特性/伪元素使用场景1_3.png)

##### 3.2、使用场景2：仿土豆效果

* 实现效果：

![伪元素使用场景2](https://gitee.com/huqian025/my-images/raw/master/css/css3新特性/伪元素使用场景2.png)

* 实现方法：

![伪元素使用场景2_2](https://gitee.com/huqian025/my-images/raw/master/css/css3新特性/伪元素使用场景2_2.png)
![伪元素使用场景2_3](https://gitee.com/huqian025/my-images/raw/master/css/css3新特性/伪元素使用场景2_3.png)

##### 3.3、使用场景3：伪元素清除浮动本质

伪元素清除浮动，相当于是额外标签法升级和优化

![伪元素使用场景3](https://gitee.com/huqian025/my-images/raw/master/css/css3新特性/伪元素使用场景3.png)
![伪元素使用场景3_2](https://gitee.com/huqian025/my-images/raw/master/css/css3新特性/伪元素使用场景3_2.png)

### 二、css3盒子模型（box-sizing）

* css3中可以通过box-sizing来指定盒模型

    1. `box-sizing: content-box`盒子大小为 width+padding+border（默认）

    2. `box-sizing: border-box`盒子大小为 width

如果盒子模型我们改为了 `box-sizing: border-box`，那padding和border就不会撑大盒子（前提padding和border不会超过width宽度）

### 三、图片模糊处理（css3滤镜filter）

filter css属性将模糊或颜色偏移等图形效果应用于元素

* 语法：

![filter语法](https://gitee.com/huqian025/my-images/raw/master/css/css3新特性/filter语法.png)

* 示例：

![filter示例代码](https://gitee.com/huqian025/my-images/raw/master/css/css3新特性/filter示例代码.png)
![filter示例效果](https://gitee.com/huqian025/my-images/raw/master/css/css3新特性/filter示例效果.png)

* 补充：backdrop-filter：CSS 属性可以让你为一个元素后面区域添加图形效果（如模糊或颜色偏移）。 因为它适用于元素背后的所有元素，为了看到效果，必须使元素或其背景至少部分透明。

### 四、计算盒子宽度（calc函数）

calc() 此css函数让你在声明css属性值时执行一些计算

* 语法：

![calc语法](https://gitee.com/huqian025/my-images/raw/master/css/css3新特性/calc语法.png)

* 示例：

![calc示例代码](https://gitee.com/huqian025/my-images/raw/master/css/css3新特性/calc示例代码.png)
![calc示例效果](https://gitee.com/huqian025/my-images/raw/master/css/css3新特性/calc示例效果.png)

### 五、过渡transition（重点）

* 语法：`transition: 要设置过渡的属性 花费时间 运动曲线 何时开始;`
  
    1. 属性：想要变化的css属性，宽度、高度、背景颜色、内外边距都可以。如果想要所有的属性都变化过渡，写一个all就可以了
    
    2. 花费时间：单位是秒（必须写单位），比如0.5s

    3. 运动曲线：默认是ease（可以省略）
    
    4. 何时开始：单位是秒（必须写单位），可以设置延迟触发时间，默认是0s（可以省略）
    
* 使用口诀：谁要过渡给谁加

![过渡1](https://gitee.com/huqian025/my-images/raw/master/css/css3新特性/过渡1.png)
![过渡2](https://gitee.com/huqian025/my-images/raw/master/css/css3新特性/过渡2.png)

### 六、2D转换transform

#### 1、移动translate

2D移动是2D转换里面的一种功能，可以改变元素在页面中的位置，类似定位

##### 1.1、语法
```css
transform: translate(x, y); 
/* 或者分开写 */
transform: translateX(n);
transform: translateY(n);
```

##### 1.2、重点

* 定义2D转换中的移动，沿着X和Y轴移动元素
* translate最大的优点：不会影响到其他元素的位置
* translate中的百分比单位是相对于自身元素的 `transform: translate(50%, 50%)`
* 对行内标签没有效果

![transform_1](https://gitee.com/huqian025/my-images/raw/master/css/css3新特性/transform_1.png)

#### 2、旋转rotate

2D旋转指的是让元素在2维平面内顺时针旋转或者逆时针旋转

##### 2.1、语法
`transform: rotate(度数)`

##### 2.2、重点

* rotate里面跟度数，单位是deg，比如`rotate(45deg)`
* 角度为正时，顺时针；为负时，逆时针
* 默认旋转的中心点是元素的中心点

#### 3、转换中心点transform-origin

我们可以设置元素转换的中心点

##### 3.1、语法
`transform-origin: x y;`

##### 3.2、重点

* 注意后面的参数x和y用空格隔开
* x y默认转换的中心点是元素的中心点（50% 50%）
* 还可以给x y设置像素或方位名词（top buttom left right center）

#### 4、缩放scale

缩放，顾名思义可以放大和缩小。只要给元素添加了这个属性就能控制它放大还是缩小

##### 4.1、语法
`transform: scale(x, y);`

##### 4.2、重点

* 注意其中的x和y用逗号分割
* `transform: scale(1, 1)`：宽和高都放大一倍，相当于没有放大
* `transform: scale(2, 2)`：宽和高都放大了2倍
* `transform: scale(2, 2)`：只写一个参数，第二个参数则和第一个参数一样，相当于scale(2, 2)
* `transform: scale(0.5, 0.5)`：缩小
* scale缩放最大的优势：可以设置转换中心点缩放，默认以中心点缩放，而不影响其他盒子

#### 5、2D转换综合写法

* 同时使用多个装换，其格式为：`transform: translate() rotate() scale()`
* 其顺序会影响转换的效果（先旋转会改变坐标轴方向）
* 当我们同时有位移和其他属性时，要将位移放在最前面

### 七、动画animation

动画（animation）是css3中具有颠覆性的特征之一，可通过设置多个节点来精确控制一个或一组动画，常用来实现复杂的动画效果。相比于过渡，动画可以实现更多变化，更多控制，连续自动播放等效果

#### 1、动画的基本使用

* 制作动画分为两步：
  
    1. 先定义动画
    
    2. 再使用（调用）动画
```css
/* 使用keyframes定义动画（类似定义类选择器） */
@keyframes 动画名称 {
    0% {
        width: 100px;
    }
    100% {
        width: 200px;
    }
}
/* animation-name指定使用动画的名称，animation-duration指定动画执行总时长 */
div {
    animation-name: move;
    animation-duration: 1s;
}
```

* 动画序列
    * 0%是动画的开始，100%是动画的完成，这样的规则就是动画序列
    * 在@keyframes中规定某项css样式，就能创建由当前样式逐渐改为新样式的动画效果
    * 动画是使元素从一种样式逐渐变化为另一种样式的效果。我们可以改变任意多的样式，任意多的次数
    * 请用百分比来规定变化发生的时间，或使用关键词“from”和“to”，等同于0%和100%

#### 2、动画常见属性

![动画常见属性](https://gitee.com/huqian025/my-images/raw/master/css/css3新特性/动画常见属性.png)

#### 3、动画属性简写

animation: 动画名称 持续时间 运动曲线 何时开始 播放次数 是否反方向 动画起始或者结束状态
`animation: move 5s linear 2s infinite alternate forwards;`

* 简写属性不包含`animation-play-state`
* 暂停动画：`animation-play-state: puased;`经常和鼠标经过等其他配合使用
* 想要动画走回来，而不是直接跳回来：`animation-direction: alternate`
* 盒子动画结束后，停在结束位置：`animation-fill-mode: forwards`

#### 4、速度曲线细节

![动画速度曲线](https://gitee.com/huqian025/my-images/raw/master/css/css3新特性/动画速度曲线.png)

* steps 就是分几步来完成我们的动画，有了steps就不要再写ease或者linear了

### 八、3D转换transform

#### 1、三维坐标系

三维坐标系其实就是指立体空间，立体空间是由3个轴共同组成的

* x轴：水平向右。x右边是正值，左边是负值
* y轴：垂直向下。y下面是正值，上面是负值
* z轴：垂直屏幕。往外面是正值，往里面是负值

#### 2、3D移动translate3d

3D移动是在2D的基础上多加了一个可以移动的方向，就是z轴方向

* `transform: translateX(100px);`：仅仅是在x轴上移动
* `transform: translateY(100px);`：仅仅是在y轴上移动
* `transform: translateZ(100px);`：仅仅是在z轴上移动（注意：translateZ一般用px单位）
* `transform: translate3d(x, y, z);`：其中x、y、z指要移动的轴的方向、距离。不能省略，没有就写0

#### 3、透视perspective

* 透视可以理解为视距，值越小，用户与3D空间z平面越近，视觉效果更强；值越大，用户与3D空间z平面越远，视觉效果越小
* 透视写在被观察元素的父盒子上，单位是px
* translateZ写在被观察的元素上，仅仅是在z轴上移动，有了透视才能看到translateZ引起的变化

![透视perspective](https://gitee.com/huqian025/my-images/raw/master/css/css3新特性/透视perspective.png)

#### 4、3D旋转rotate3d

3D旋转指可以让元素在三维平面内沿着x轴、y轴、z轴或者自定义轴进行旋转

* `transform: rotateX(45deg);`：沿着x轴正方向旋转45度
* `transform: rotateY(45deg);`：沿着y轴正方向旋转45度
* `transform: rotateZ(45deg);`：沿着z轴正方向旋转45度
* `transform: rotate3d(x, y, z, deg);`：沿着自定义轴旋转，deg为角度（了解）`transform: rotate3d(1, 0, 0, 45deg); 沿x轴旋转45度`

* 判断元素旋转方向（左手法则）：
    * 左手的大拇指指向旋转轴的正方向
    * 其余手指的弯曲方向就是该元素沿旋转轴的旋转方向

#### 5、3D呈现transform-style

* 控制子元素是否开启三维立体环境
* `transform-style: flat;`子元素不开启3d立体空间，默认的
* `transform-style: preserve-3d;`子元素开启立体空间
* 代码写给父级，但影响的是子盒子
* 这个属性很重要，后面必用

### 九、浏览器私有前缀

浏览器私有前缀是为了兼容老版本的写法，比较新版本的浏览器无须添加

#### 1、私有前缀

* `-moz-`：代表firefox浏览器私有属性
* `-ms-`：代表ie浏览器私有属性
* `-webkit-`：代表safari、chrome私有属性
* `-o-`：代表opera私有属性

#### 2、提倡写法
```css
-moz-border-radius: 10px;
-webkit-border-radius: 10px;
-o-border-radius: 10px;
border-radius: 10px;
```

### 十、translate与绝对定位

* translate()是transform的一个值。改变transform或opacity不会触发浏览器重新布局（reflow）或重绘（repaint），只会触发复合（compositions）。而改变绝对定位会触发重新布局，进而触发重绘和复合。transform使浏览器为元素创建一个 GPU 图层，但改变绝对定位会使用到 CPU。 因此translate()更高效，可以缩短平滑动画的绘制时间。
* 当使用translate()时，元素仍然占据其原始空间（有点像position：relative），这与改变绝对定位不同。

### 十一、渐变

#### 1、线性渐变

* 语法
```css
background: linear-gradient(渐变方向, start-color, ..., end-color);
/* 从 右 到 左 渐变 */
background: linear-gradient(to left, red, blue);
/* 从 右下 到 左上 渐变 */
background: linear-gradient(to left top, red, blue);
```

* 起始方向可以是：方位名词或者度数，如果省略默认就是从上到下

#### 2、径向渐变

* 语法
```css
background: radial-gradient(渐变形状，start-color, ..., end-color)
/* 颜色均匀 的 椭圆 径向渐变 */
background: radial-gradient(red, yellow, green);
/* 颜色均匀 的 圆形 径向渐变 */
background: radial-gradient(circle, red, yellow, green);
/* 颜色不均匀 的 椭圆 径向渐变 */
background: radial-gradient(red 5%, yellow 15%, green 60%);
```

* 渐变形状的值为 circle（圆形） 和 ellipse（椭圆，默认值） 
