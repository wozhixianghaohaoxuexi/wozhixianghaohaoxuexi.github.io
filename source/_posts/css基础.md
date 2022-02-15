---
title: css基础
date: 2022-02-14 11:34:39
categories:
  - css
tags:
---

### 一、css基本用法

#### 1、css语法
```
选择器 {
    属性名: 属性值;
    属性名: 属性值;
}
```

#### 2、css应用方式

##### 2.1、行内样式，使用HTML标签的style属性定义
```html
<h2 style="color: red;">行内样式</h2>
```

##### 2.2、内部样式，通过页面中的style标签定义
```html
<style>
    h2 {
        color: red;
    }
</style>
```

##### 2.3、外部样式，引入外部css文件
```html
<!-- 方法一 -->
<link rel="stylesheet" type="text/css" href="css文件路径">

<!-- 方法二 -->
<style>
    @import "css文件路径";
    @import url("css文件路径");
<style>
```

### 二、选择器

#### 1、基础选择器

##### 1.1、标签选择器

* 直接使用标签名称
```css
p {
    color: red;
}
```

##### 1.2、类选择器

* 使用.
```css
.class1 {
    color: red;
}
```

##### 1.3、ID选择器

* 使用#，且唯一
```css
#id1 {
    color: red;
}
```

##### 1.4、通配符选择器

* 使用*
```css
* {
    color: red;
}
```

#### 2、复杂选择器

##### 2.1、复合选择器

* 标签选择器和类选择器、标签选择器和id选择器，一起使用（不用空格或逗号）
* 必须同时满足两个条件样式才生效
```css
h1.class1 {
}
h1#id1 {
}
```

##### 2.2、多元素（并集）选择器

* E,F
* 同时匹配所有E元素和F元素，E和F之间用逗号隔开

##### 2.3、后代选择器

* E F
* 匹配所有属于E元素后代的F元素，E和F之间用空格隔开（匹配所有后代）

##### 2.4、子元素选择器

* E>F
* 匹配所有E元素子代的F元素（只匹配子代）

##### 2.5、兄弟元素选择器

* E~F
    * 一般兄弟选择器，匹配E元素只有所有的同级元素F
* E+F
    * 紧邻兄弟选择器，匹配紧随E元素之后的同级元素F

```html
<style>
// 只匹配第四个p标签
.p3+p {
  background-color: purple;
}
// 匹配.p3之后的所有同级p标签
.p3~p {
    background-color: purple;
}

// .p3之后的紧邻元素中没有.p5，所以没有匹配项
.p3+.p5 {
  background-color: purple;
}
// 匹配.p3之后的所有同级.p5
.p3~.p5 {
  background-color: purple;
}
</style>

<div id="box">
    <p>1</p>
    <p class="p5">2</p>
    <p class="p3">3</p>
    <p>4</p>
    <p class="p5">5</p>
</div>
```

##### 2.6、属性选择器

* E[att=val]
* 匹配所有att属性等于“val”的E元素

##### 2.7、伪类选择器（一般用于a标签）

* :link 未访问的链接
* :visited 已访问的链接
* :hover 鼠标悬浮在链接上
* :active 选定的链接

##### 2.8、伪元素选择器

* ::first-letter 第一个字符的样式
* ::first-line 第一行的样式
* ::before 在元素内容最前面添加的内容，需要配合content属性使用
* ::after 在元素内容最后面添加的内容，需要配合content属性使用
```css
p:before {
    content: '添加的内容';
}
```

#### 3、选择器优先级

* 行内样式>id选择器>类选择器>标签选择器
* !important，使某个样式有最高优先级（比行内样式还要高，一般用在css覆盖javascript设置上）
```css
.class1 {
    color: red !important;
}
```

### 三、常用css属性

#### 1、字体属性

![字体属性](https://gitee.com/huqian025/my-images/raw/master/css/css%E5%9F%BA%E7%A1%80/%E5%AD%97%E4%BD%93%E5%B1%9E%E6%80%A7.png)

#### 2、文本属性

![文本属性](https://gitee.com/huqian025/my-images/raw/master/css/css%E5%9F%BA%E7%A1%80/%E6%96%87%E6%9C%AC%E5%B1%9E%E6%80%A7.png)

#### 3、背景属性

![背景属性](https://gitee.com/huqian025/my-images/raw/master/css/css%E5%9F%BA%E7%A1%80/%E8%83%8C%E6%99%AF%E5%B1%9E%E6%80%A7.png)

#### 4、列表属性

![列表属性](https://gitee.com/huqian025/my-images/raw/master/css/css%E5%9F%BA%E7%A1%80/%E5%88%97%E8%A1%A8%E5%B1%9E%E6%80%A7.png)

#### 5、表格属性

![表格属性](https://gitee.com/huqian025/my-images/raw/master/css/css%E5%9F%BA%E7%A1%80/%E8%A1%A8%E6%A0%BC%E5%B1%9E%E6%80%A7.png)

### 四、元素实际占用空间

**元素实际占用空间 = 宽/高 + 内边距 + 边框 + 外边距**

### 五、[定位position（重点）](https://www.cnblogs.com/qianguyihao/p/8296748.html)

![定位类型](https://gitee.com/huqian025/my-images/raw/master/css/css%E5%9F%BA%E7%A1%80/%E5%AE%9A%E4%BD%8D%E7%B1%BB%E5%9E%8B.png)

#### 1、相对定位relative（不脱标，占位置）

* 相对定位：让元素相对于自己原来的位置，进行位置调整（可用于盒子的位置微调）
* 相对定位特点：
    1. 它是相对于自己原本的位置来移动的（移动位置的时候参照点是自己原来的位置）
    2. 在标准流中占有位置，后面的盒子仍然以标准流的方式对待它（不脱标，标准流中占有位置）
* 相对定位用途：
    1. 微调元素
    2. 做绝对定位的参考
* 相对定位的定位值（值为正数，负数则相反）
    * left：盒子右移
    * right：盒子左移
    * top：盒子下移
    * bottom：盒子上移

#### 2、绝对定位absolute（脱标，不占位置）

* 绝对定位：定义横纵坐标，原点在父容器的左上角或左下角。横坐标用left表示，纵坐标用top或者bottom表示。
* 绝对定位之后，标签就不区分所谓的行内元素、块级元素了，不需要display:block就可以设置宽高了

##### 2.1、以浏览器为参考点（当祖先元素没有定位时）

* 横坐标用left描述
* 纵坐标如果用top描述，那参考点就是页面的左上角，而不是浏览器的左上角
* 纵坐标如果用bottom描述，那参考点就是浏览器首屏窗口尺寸
![绝对定位参考点](https://gitee.com/huqian025/my-images/raw/master/css/css%E5%9F%BA%E7%A1%80/%E7%BB%9D%E5%AF%B9%E5%AE%9A%E4%BD%8D%E5%8F%82%E8%80%83%E7%82%B91.png)  ![绝对定位参考点](https://gitee.com/huqian025/my-images/raw/master/css/css%E5%9F%BA%E7%A1%80/%E7%BB%9D%E5%AF%B9%E5%AE%9A%E4%BD%8D%E5%8F%82%E8%80%83%E7%82%B92.png)

![绝对定位参考点问题](https://gitee.com/huqian025/my-images/raw/master/css/css%E5%9F%BA%E7%A1%80/%E7%BB%9D%E5%AF%B9%E5%AE%9A%E4%BD%8D%E5%8F%82%E8%80%83%E7%82%B9%E9%97%AE%E9%A2%98.png)

##### 2.2、以盒子为参考点

* 一个绝对定位的元素，如果父辈元素中也出现了定位（无论是绝对定位、相对定位、固定定位）的元素，那将以该祖先元素为参考点
    * 最近的已定位的祖先元素，不一定是父亲
    * 祖先元素不一定是相对定位
    * 绝对定位的儿子，无视参考盒子的padding
    ![绝对定位以盒子为参考点](https://gitee.com/huqian025/my-images/raw/master/css/css%E5%9F%BA%E7%A1%80/%E7%BB%9D%E5%AF%B9%E5%AE%9A%E4%BD%8D%E4%BB%A5%E7%9B%92%E5%AD%90%E4%B8%BA%E5%8F%82%E8%80%83%E7%82%B9.png)
    
##### 2.3、让绝对定位的盒子居中

* 如果想让一个标准文档流中的盒子居中（水平方向看），可以将其设置margin: 0 auto;属性
* 可当盒子使用绝对定位时，它已经脱离了标准文档流，让它居中可以使用如下方法（left: 50%; margin-left: 负的宽度的一半）：
```css
div {
    width: 600px;
    height: 60px;
    position: absolute; /* 绝对定位的盒子 */ 
    left: 50%;            /* 首先让左边线居中 */
    top: 0;
    margin-left: -300px; /* 然后左移宽度（600px）的一半*/
}
```

#### 3、固定定位fixed（脱标，不占位置）

* 固定定位：相对浏览器窗口进行定位，无论页面怎么滚动，这个盒子显示的位置不变（IE6不兼容）
* 固定定位特点：
    1. 以浏览器的可视窗口为参照点移动元素
        * 跟父元素没有任何关系
        * 不随滚动条滚动
    2. 固定定位不再占有原先的位置
        * 固定定位也是脱标的，其实固定定位也可以看做是一种特殊的绝对定位
* 用途：
    * 网页右下角的“返回顶部”
    * 顶部导航条（假设顶部导航条的高度是60px，为了防止其他的内容被导航条覆盖，我们要给body标签设置60px的padding-top）

##### 3.1、固定定位小技巧（固定在版心右侧位置）

小技巧：

1. 让固定定位的盒子`left: 50%;`，走到浏览器可视区（也可以看做版心）的一半位置
2. 让固定定位的盒子`margin-left: 版心宽度的一半距离`，多走版心宽度的一半位置，就可以让固定定位的盒子贴着版心右侧对齐了

![固定定位小技巧](https://gitee.com/huqian025/my-images/raw/master/css/css%E5%9F%BA%E7%A1%80/%E5%9B%BA%E5%AE%9A%E5%AE%9A%E4%BD%8D%E5%B0%8F%E6%8A%80%E5%B7%A7.png)

#### 4、z-index属性

* z-index属性（不能加单位）：表示谁压着谁，数值大的压盖数值小的
* 特性：
    * 数值大的位于上层，数值小的位于下层
    * z-index值没有单位，就是一个正整数，默认值为auto
    * 如果大家都没有z-index值，或z-index值一样，那么在HTML代码里写在后面就能在上面压住别人。定位了的元素永远能够压住没有定位的元素
    * 只有定位了的元素才有z-index值
    * 从父现象：如果父亲1比父亲2大，那么即使儿子1比儿子2小，儿子1也在上层

#### 5、粘性定位 sticky

* 粘性定位可以被认为是相对定位和固定定位的混合。先相对定位，页面滚动到一定距离变为固定定位（一般使用js实现，而不是sticky）
* 粘性定位的特点：
    1. 以浏览器的可视窗口为参照点移动元素（固定定位特点）
    2. 粘性定位占有原来的位置（相对定位的特点）
    3. 必须添加top、left、right、bottom其中一个才有效
* 跟页面滚动搭配使用。兼容性较差，IE不支持

#### 6、定位的拓展

##### 6.1、定位特殊特性

绝对定位和固定定位也和浮动类似

1. 行内元素添加绝对或者固定定位，可以直接设置高度和宽度
2. 块级元素添加绝对或者固定定位，如果不给宽度和高度，默认大小是内容的大小

##### 6.2、脱标的盒子不会触发外边距塌陷

* 浮动元素、绝对定位（固定定位）元素都不会触发外边距合并的问题

##### 6.3、绝对定位（固定定位）会完全压住盒子

* 浮动元素只会压住它下面的标准流盒子，但是不会压住下面标准流盒子里面的文字（图片）
    * 浮动之所以不会压住文字，因为浮动产生的目的最初是为了做文字环绕效果的，文字会围绕浮动的元素
* 绝对定位（固定定位）会压住下面标准流所有的内容

##### 6.4、如果一个盒子有left属性也有right属性，则默认会执行left属性（top和buttom会执行top属性）

### 六、[浮动（重点）](https://www.cnblogs.com/qianguyihao/p/7297736.html)

#### 1、标准文档流

##### 1.1、标准文档流的特性

* 1）空白折叠现象，无论多少个空格、换行、tab，都会折叠为一个空格
* 2）高低不齐，底边对齐（如文字和图片高度不同时，采用底部对齐）
* 3）自动换行

##### 1.2、行内元素和块级元素的区别

* 行内元素：
    * 与其他行内元素并排
    * 不能设置宽高，默认的宽度就是文字的宽度
* 块级元素：
    * 独占一行，不能与其他元素并列
    * 可以设置宽高，默认宽度为父元素的100%

![行内元素与块级元素](https://gitee.com/huqian025/my-images/raw/master/css/css%E5%9F%BA%E7%A1%80/%E8%A1%8C%E5%86%85%E5%85%83%E7%B4%A0%E4%B8%8E%E5%9D%97%E7%BA%A7%E5%85%83%E7%B4%A0.png)

##### 1.3、行内元素和块级元素的相互转换

* 块级元素转行内元素：display: inline;
* 行内元素转块级元素：display: block;

##### 1.4、脱离标准流三种方式

* 浮动
* 绝对定位
* 固定定位

#### 2、浮动的性质（不占位置）

##### 2.1、浮动元素会脱标

* 脱离标准文档流的控制（浮）移动到指定位置（动），俗称脱标
* 浮动的盒子不再保留原先的位置

![浮动演示1](https://gitee.com/huqian025/my-images/raw/master/css/css%E5%9F%BA%E7%A1%80/%E6%B5%AE%E5%8A%A8%E6%BC%94%E7%A4%BA1.png)

##### 2.2、浮动元素一行显示且元素顶部对齐

如果多个盒子都设置了浮动，则它们会按照属性值一行内显示且顶端对齐排列

**注意**：浮动的元素是互相贴靠在一起的（不会有缝隙），如果父级宽度装不下这些浮动的盒子，多出的盒子会另起一行对齐

![浮动演示2](https://gitee.com/huqian025/my-images/raw/master/css/css%E5%9F%BA%E7%A1%80/%E6%B5%AE%E5%8A%A8%E6%BC%94%E7%A4%BA2.png)
![浮动演示3](https://gitee.com/huqian025/my-images/raw/master/css/css%E5%9F%BA%E7%A1%80/%E6%B5%AE%E5%8A%A8%E6%BC%94%E7%A4%BA3.png)

##### 2.3、浮动元素具有行内块元素特性

任何元素都可以浮动，不管原先是什么模式的元素，添加浮动后具有行内块元素相似的特性

* 如果块级元素没有设置宽度，默认宽度和父元素一样宽，但是添加浮动后，它的大小根据内容决定
* 浮动的元素中间没有缝隙，是紧挨在一起的
* 行内元素同理

#### 3、浮动的清除（清除浮动元素造成的影响）

##### 3.0、为什么要清除浮动

* 由于父级盒子很多情况下不方便给高度，但是子盒子浮动又不占用位置，最后父级盒子高度为0时，就会影响下面的标准流盒子
* 由于浮动元素不再占用原文档流的位置，所以它会对后面的元素排版产生影响

![清除浮动](https://gitee.com/huqian025/my-images/raw/master/css/css%E5%9F%BA%E7%A1%80/%E6%B8%85%E9%99%A4%E6%B5%AE%E5%8A%A8.png)

##### 3.1、清除浮动本质

* 清除浮动的本质是清除浮动元素造成的影响
* 如果父盒子本身有高度，则不需要清除浮动
* 清除浮动后，父级就会根据浮动的子盒子自动检测高度。父级有了高度，就不会影响下面的标准流了

##### 3.2、方法一：额外标签法（不常用）

额外标签法也称为隔墙法。

* 使用：在浮动元素末尾添加一个空标签，添加样式`clear: both`，例如`<div style="clear: both"></div>`，或者其他标签（如<br/>等）
* 优点：通俗易懂，书写方便
* 缺点：添加许多无意义的标签，结构化较差

**注意**：要求这个新的空标签必须是块级元素

![额外标签法1](https://gitee.com/huqian025/my-images/raw/master/css/css%E5%9F%BA%E7%A1%80/%E9%A2%9D%E5%A4%96%E6%A0%87%E7%AD%BE%E6%B3%951.png)
![额外标签法2](https://gitee.com/huqian025/my-images/raw/master/css/css%E5%9F%BA%E7%A1%80/%E9%A2%9D%E5%A4%96%E6%A0%87%E7%AD%BE%E6%B3%952.png)

##### 3.3、方法二：父元素overflow（重点）

* 使用：给父元素添加overflow属性，将其属性值设置为hidden（常用）、auto或scroll
* 优点：代码简介
* 缺点：无法显示溢出的部分
* overflow:hidden能够清除浮动的原因是开启了BFC,利用BFC计算高度时浮动元素也参与计算的特性

![父元素overflow1](https://gitee.com/huqian025/my-images/raw/master/css/css%E5%9F%BA%E7%A1%80/%E7%88%B6%E5%85%83%E7%B4%A0overflow1.png)
![父元素overflow2](https://gitee.com/huqian025/my-images/raw/master/css/css%E5%9F%BA%E7%A1%80/%E7%88%B6%E5%85%83%E7%B4%A0overflow2.png)

##### 3.4、方法三：:after伪元素法（重点，代表网站：百度、淘宝、网易等）

:after方式是额外标签法的升级版

* 使用：给父元素添加如下属性
```css
.clearflix:after {
    content: '';
    display: block;
    height: 0;
    clear: both;
    visibility: hidden;
}
/* 兼容IE6、7 */
.clearfix {
    *zoom: 1;
}
```
* 原理：通过css在子元素末尾添加一个块级空标签，并设置`clear: both;`属性
* 优点：没有增加标签，结构更简单
* 缺点：照顾低版本浏览器

![after伪元素法1](https://gitee.com/huqian025/my-images/raw/master/css/css%E5%9F%BA%E7%A1%80/after%E4%BC%AA%E5%85%83%E7%B4%A0%E6%B3%951.png)
![after伪元素法2](https://gitee.com/huqian025/my-images/raw/master/css/css%E5%9F%BA%E7%A1%80/after%E4%BC%AA%E5%85%83%E7%B4%A0%E6%B3%952.png)

##### 3.5、方法四：双伪元素（重点，代表网站：小米、腾讯等）

* 使用：给父元素添加如下属性
```css
.clearfix:before, .clearfix:after {
    content: "";
    display: table;
}
.clearfix:after {
    clear: both;
}
/* 兼容IE6、7 */
.clearfix {
    *zoom: 1;
}
```
* 原理：通过css在子元素的起始和末尾都添加一个空标签，并设置`clear: both;`属性
* 优点：代码更简介
* 缺点：照顾低版本浏览器

![双伪元素1](https://gitee.com/huqian025/my-images/raw/master/css/css%E5%9F%BA%E7%A1%80/%E5%8F%8C%E4%BC%AA%E5%85%83%E7%B4%A01.png)
![双伪元素2](https://gitee.com/huqian025/my-images/raw/master/css/css%E5%9F%BA%E7%A1%80/%E5%8F%8C%E4%BC%AA%E5%85%83%E7%B4%A02.png)

#### 4、IE6兼容性问题

##### 4.1、IE6不支持height小于12px的盒子

* 问题：设置一个height为5px、width为200px，IE 8和IE 6的显示效果如下：

  ![ie兼容1](https://gitee.com/huqian025/my-images/raw/master/css/css%E5%9F%BA%E7%A1%80/ie%E5%85%BC%E5%AE%B91.png)

* 解决方法：将盒子的字号大小设置为小于盒子的高，比如：盒子高为5px，就把_font-size（给css属性前加上下划线，就是IE6的专有属性）设置为0px（0px<5px）
```css
div {
    height: 5px;
    _font-size: 0px;
}
```

##### 4.2、IE6不支持overflow: hidden;清除浮动

* 问题：`overflow: hidden;`让溢出盒子border的内容隐藏，这个功能IE6是支持的，不兼容的是用它来清除浮动
* 解决方法：追加一条`_zoom: 1;`属性，`_zoom: 1;`能触发浏览器的hasLayout机制（这个机制只有IE6有，不用深究）。
```css
div {
    overflow: hidden;
    _zoom: 1;
}
```

##### 4.3、IE6的双倍margin的bug

* 问题：当出现连续浮动的元素，携带与浮动方向相同的margin时，队首的元素会出现双倍margin。

    ![ie兼容3](https://gitee.com/huqian025/my-images/raw/master/css/css%E5%9F%BA%E7%A1%80/ie%E5%85%BC%E5%AE%B93.png)

* 解决方法：
    * 1）使浮动方向与margin方向相反（推荐，作为习惯）
    * 2）单独给队首元素设置一半的margin
```css
/* 方法一 */
div {
    float: left;
    margin-right: 40px
}

/* 方法二 */
div {
    _margin-left: 20px;
}
```

##### 4.4、IE6 3px的bug

* 问题：儿子右浮动，且设置margin-right为10px，结果变为13px。

  ![ie兼容4](https://gitee.com/huqian025/my-images/raw/master/css/css%E5%9F%BA%E7%A1%80/ie%E5%85%BC%E5%AE%B94.png)

* 解决方法：出现3px的bug说明代码不标准，在描述父子间的距离时，使用padding，而不是margin

#### 5、margin相关问题

##### 5.1、margin塌陷/margin重叠

* 标准文档流中，垂直方向的margin不叠加，取较大的值作为margin（水平方向的margin可以叠加，即水平方向没有塌陷现象）。如下图所示：

  ![margin塌陷](https://gitee.com/huqian025/my-images/raw/master/css/css%E5%9F%BA%E7%A1%80/margin%E5%A1%8C%E9%99%B7.png)

* 如果不在标准文档流中，如盒子都浮动了，那两个盒子之间就没有塌陷现象。

##### 5.2、盒子居中`margin: 0 auto;`

* 上下的margin为0，左右的margin都尽可能的大，于是就居中了
* 注意：
    * 1）只有标准流的盒子才能使用`margin: 0 auto;`居中。当盒子浮动、绝对定位、固定定位都不能使用
    * 2）使用`margin: 0 auto;`的盒子，必须有明确的width
    * 3）`margin: 0 auto;`是让盒子居中，不是让盒子中的文本居中。文本居中要使用`text-align: center;`

##### 5.3、描述父子之间距离时，善用父亲的padding，而不是儿子的margin

* margin这个属性，本质上描述的是兄弟和兄弟之间的距离；最好不要用这个marign表达父子之间的距离。

* 问题：通过儿子的margin-top属性让其与父亲保持一定的上边距（父亲为设置border属性），却让父亲也有了margin-top属性。如下例所示：

  ![子元素设置margin](https://gitee.com/huqian025/my-images/raw/master/css/css%E5%9F%BA%E7%A1%80/%E5%AD%90%E5%85%83%E7%B4%A0%E8%AE%BE%E7%BD%AEmargin.png) ![父元素设置border子元素再设置margin](https://gitee.com/huqian025/my-images/raw/master/css/css%E5%9F%BA%E7%A1%80/%E7%88%B6%E5%85%83%E7%B4%A0%E8%AE%BE%E7%BD%AEborder%E5%AD%90%E5%85%83%E7%B4%A0%E5%86%8D%E8%AE%BE%E7%BD%AEmargin.png)

#### 6、浮动元素布局注意点

##### 6.1、浮动和标准流的父盒子搭配

* 先用标准流的父元素排列上下位置，之后内部的子元素采用浮动排列左右位置

![浮动和标准流父盒子](https://gitee.com/huqian025/my-images/raw/master/css/css%E5%9F%BA%E7%A1%80/%E6%B5%AE%E5%8A%A8%E5%92%8C%E6%A0%87%E5%87%86%E6%B5%81%E7%88%B6%E7%9B%92%E5%AD%90.png)

##### 6.2、一个元素浮动了，理论上其余的兄弟元素也要浮动

* 一个盒子里面有多个子盒子，其中一个盒子浮动了，那么其他的兄弟元素也应该浮动，以防止引起问题
* 浮动的盒子只会影响浮动盒子后面的标准流，不会影响前面的标准流

![一个盒子浮动](https://gitee.com/huqian025/my-images/raw/master/css/css%E5%9F%BA%E7%A1%80/%E4%B8%80%E4%B8%AA%E7%9B%92%E5%AD%90%E6%B5%AE%E5%8A%A8.png)
![多个盒子浮动](https://gitee.com/huqian025/my-images/raw/master/css/css%E5%9F%BA%E7%A1%80/%E5%A4%9A%E4%B8%AA%E7%9B%92%E5%AD%90%E6%B5%AE%E5%8A%A8.png)

### 七、其他css属性

#### 1、元素的显示和隐藏

##### 1.1、display（设置元素是否显示，以及是否独占一行）

![display属性](https://gitee.com/huqian025/my-images/raw/master/css/css%E5%9F%BA%E7%A1%80/display%E5%B1%9E%E6%80%A7.png)

##### 1.2、 visibility（设置元素的显示和隐藏）

![visibility属性](https://gitee.com/huqian025/my-images/raw/master/css/css%E5%9F%BA%E7%A1%80/visibility%E5%B1%9E%E6%80%A7.png)

##### 1.3、区别

* display隐藏时不再占据页面的空间，后面的元素会占用其位置
* visibility隐藏时会占据页面中的空间，位置还保留在页面中，只是不显示

#### 2、轮廓属性outline

##### 2.1、简介

* 轮廓outline用于在元素周围绘制一个轮廓，位于border外围，可以突出显示元素
* 在浏览器中，当鼠标点击或使用tab键让一个表单或链接获取焦点时，该元素会有一个轮廓outline

##### 2.2、常用属性

![outline属性](https://gitee.com/huqian025/my-images/raw/master/css/css%E5%9F%BA%E7%A1%80/outline%E5%B1%9E%E6%80%A7.png)

##### 2.3、outline和border的区别

* border可以引用与所有html元素，而outline主要用于表单元素、超链接元素
* 当元素获取焦点时会自动出现outline轮廓效果，当失去焦点时会自动消失，这是浏览器默认行为
* outline不影响元素的尺寸和位置，而border会影响

#### 3、最大/最小宽高

* max-height：设置元素的最大高度
* max-width：设置元素的最大宽度
* min-height：设置元素的最小高度
* min-width：设置元素的最小宽度

#### 4、overflow属性（元素内容溢出时如何处理）

![overflow属性](https://gitee.com/huqian025/my-images/raw/master/css/css%E5%9F%BA%E7%A1%80/overflow%E5%B1%9E%E6%80%A7.png)

#### 5、cursor属性（设置光标形状）

* default：默认光标，一般为箭头
* pointer：手形，光标移动超链接上时一般显示为手形
* move：可移动
* text：文本
* wait：等待
* help：帮助
![cursor属性](https://gitee.com/huqian025/my-images/raw/master/css/css%E5%9F%BA%E7%A1%80/cursor%E5%B1%9E%E6%80%A7.png)

#### 6、阴影

##### 6.1、盒子阴影

![盒子阴影](https://gitee.com/huqian025/my-images/raw/master/css/css%E5%9F%BA%E7%A1%80/%E7%9B%92%E5%AD%90%E9%98%B4%E5%BD%B1.png)

##### 6.2、文字阴影

![文字阴影](https://gitee.com/huqian025/my-images/raw/master/css/css%E5%9F%BA%E7%A1%80/%E6%96%87%E5%AD%97%E9%98%B4%E5%BD%B1.png)

### 八、css属性书写顺序

![css属性书写顺序](https://gitee.com/huqian025/my-images/raw/master/css/css%E5%9F%BA%E7%A1%80/css%E5%B1%9E%E6%80%A7%E4%B9%A6%E5%86%99%E9%A1%BA%E5%BA%8F.png)

### 九、外边距合并

使用margin定义块元素的垂直外边距时，可能会出现外边距合并

#### 1、相邻块元素垂直外边距的合并

当上下相邻的两个块元素（兄弟元素）相遇时，如果上面的元素有下外边距margin-bottom，下面的元素有上外边距margin-top，则**它们之间的垂直间距不是margin-bottom与margin-top之和，而是取两个值中的较大者**，这种现象被称为相邻块元素垂直外边距的合并

**解决方案：**
1. 尽量只给一个盒子添加margin值
2. display: inline-block
3. float属性值不是"none"的元素
4. 绝对定位

![外边距合并](https://gitee.com/huqian025/my-images/raw/master/css/css%E5%9F%BA%E7%A1%80/%E5%A4%96%E8%BE%B9%E8%B7%9D%E5%90%88%E5%B9%B6.png)

#### 2、嵌套块元素垂直外边距的塌陷

对于两个嵌套关系（父子关系）的块元素，父元素有上外边距同时子元素也有上外边距，此时父元素会塌陷较大的外边距值

**解决方案：**
1. 可以为父元素定义上边框
2. 可以为父元素定义上内边距
3. 可以为父元素添加`overflow: hidden;`
4. 浮动、固定、绝对定位的盒子不会有塌陷问题



> 参考资料：
>
> [CSS属性：定位属性（图文详解）](https://www.cnblogs.com/qianguyihao/p/8296748.html)
>
> [CSS样式----浮动（图文详解）](https://www.cnblogs.com/qianguyihao/p/7297736.html)
>
> [pink老师前端视频](https://www.bilibili.com/video/BV14J4114768)
