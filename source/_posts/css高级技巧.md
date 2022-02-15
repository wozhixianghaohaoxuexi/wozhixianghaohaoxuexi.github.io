---
title: css高级技巧
date: 2022-02-14 11:34:51
categories:
  - css
tags:
---

### 一、精灵图

#### 1、为什么需要精灵图

一个网页中往往会应用很多小的背景图片作为修饰，当页面中的图像过多时，服务器就会频繁地接收和发送请求图片，造成服务器请求压力过大，这将大大降低页面的加载速度。因此**为了有效地减少服务器接收和发送请求的次数，提高页面的加载速度**，出现了css精灵技术（也称CSS  Sprites）

核心原理：将网页中的一些小背景图像整合到一张大图中，这样服务器只需要一次请求就可以了

![精灵图](https://gitee.com/huqian025/my-images/raw/master/css/css高级技巧/精灵图.png)

#### 2、精灵图的使用

将精灵图作为背景图片，通过background-position对精灵图位置进行调整。类似拿盒子去裁剪大图中的某个小图片

![精灵图使用](https://gitee.com/huqian025/my-images/raw/master/css/css高级技巧/精灵图使用.png)

### 二、字体图标

#### 1、字体图标的产生

字体图标使用场景：主要用于显示网页中通用、常用的一些小图标

精灵图缺点：

1. 图片文件还是比较大的
2. 图片本身放大和缩小会失真
3. 一旦图片制作完毕想要更换非常复杂

此时有一种技术的出现很好的解决了以上问题，就是字体图标iconfont（展示的是图标，本质属于字体）

#### 2、字体图标的优点

* 轻量级：一个字体图标要比一系列的图像要小，一旦字体加载了，图标就会马上渲染出来，减少了服务器请求
* 灵活性：本质其实是文字，可以随意的改变颜色、产生阴影、透明效果、旋转等
* 兼容性：几乎支持所有浏览器

注意：字体图标不能代替精灵图技术，只是对工作中图标部分技术的提升和优化

![字体图标](https://gitee.com/huqian025/my-images/raw/master/css/css高级技巧/字体图标.png)

#### 3、字体图标下载

推荐网站：
icomoon
阿里iconfont

#### 4、字体图标使用

1. 下载字体图标
2. 将fonts文件夹（里面有四种字体文件，为了保证兼容性）放在页面根目录下
![字体图标使用1](https://gitee.com/huqian025/my-images/raw/master/css/css高级技巧/字体图标使用1.png)

3. 在css样式中全局声明字体，把这些字体文件通过css引入我们的页面中。复制style.css文件中的`@font-face`样式
![字体图标使用2](https://gitee.com/huqian025/my-images/raw/master/css/css高级技巧/字体图标使用2.png)

4. 打开demo.html文件，复制图标对应的文本
![字体图标使用3](https://gitee.com/huqian025/my-images/raw/master/css/css高级技巧/字体图标使用3.png)

5. 为复制的文本添加字体样式`font-family`（属性值为font-face引入的字体）
![字体图标使用4](https://gitee.com/huqian025/my-images/raw/master/css/css高级技巧/字体图标使用4.png)

#### 5、字体图标的追加

在图标网站中导入本地的字体图标（selection.json文件），选择需要追加的图标并重新下载，覆盖原来下载的图标文件

### 三、css三角形

#### 1、思路

* 将一个盒子高度和宽度都设置为0，并添加边框。当4个边框为不同颜色时，会出现4种不同颜色的三角形拼成的正方形

![css三角形1](https://gitee.com/huqian025/my-images/raw/master/css/css高级技巧/css三角形1.png)
![css三角形2](https://gitee.com/huqian025/my-images/raw/master/css/css高级技巧/css三角形2.png)

* 要实现三角形，可以将其余三个边框设置为透明色。三角的大小取决于边框大小

![css三角形3](https://gitee.com/huqian025/my-images/raw/master/css/css高级技巧/css三角形3.png)
![css三角形4](https://gitee.com/huqian025/my-images/raw/master/css/css高级技巧/css三角形4.png)

* 兼容性：

![css三角形兼容性](https://gitee.com/huqian025/my-images/raw/master/css/css高级技巧/css三角形兼容性.png)

* 应用：

![css三角形应用](https://gitee.com/huqian025/my-images/raw/master/css/css高级技巧/css三角形应用.png)

### 四、css用户界面样式

#### 1、鼠标样式

鼠标改为小手，其他参考css基础中cursor属性
`cursor: pointer;`

#### 2、取消表单轮廓

`outline: none;`

#### 3、防止文本域拖拽

`resize: none;`

### 五、vertical-align属性应用

#### 1、使用

* 使用场景：经常用于设置图片或者表单（行内块元素）和文字垂直对齐
* 官方解释：用于设置一个元素的垂直对齐方式，但是它只针对行内元素或者行内块元素有有效
* 语法：`vertical-align: baseline | top | middel | bottom`（默认基线对齐）

![vertical-align属性](https://gitee.com/huqian025/my-images/raw/master/css/css高级技巧/vertical-align属性.png)
![vertical-align对齐方式](https://gitee.com/huqian025/my-images/raw/master/css/css高级技巧/vertical-align对齐方式.png)

* 示例：

![vertical-align示例](https://gitee.com/huqian025/my-images/raw/master/css/css高级技巧/vertical-align示例.png)

#### 2、解决图片底部默认空白缝隙问题

bug：图片底侧会有一个空白缝隙，原因是行内块元素会和文字的基线对齐
主要解决方法有两种：

1. 给图片添加`vertical-align: middle | top | bottom`（提倡使用）
2. 把图片转化为块级元素`display: block;`

![图片底部空隙问题](https://gitee.com/huqian025/my-images/raw/master/css/css高级技巧/图片底部空隙问题.png)

### 六、溢出的文字省略号显示

#### 1、单行文字溢出省略号显示

![单行文字溢出省略号](https://gitee.com/huqian025/my-images/raw/master/css/css高级技巧/单行文字溢出省略号.png)

#### 2、多行文字溢出省略号显示

多行文本溢出显示省略号，有较大的兼容性问题，适合于webKit浏览器或移动端（移动端大部分是webkit内核）

![多行文字溢出省略号](https://gitee.com/huqian025/my-images/raw/master/css/css高级技巧/多行文字溢出省略号.png)
![多行文字溢出省略号2](https://gitee.com/huqian025/my-images/raw/master/css/css高级技巧/多行文字溢出省略号2.png)

更推荐让后台人员来做这个效果，因为后台人员可以设置显示多少字，操作更简单

### 七、常见布局技巧

#### 1、margin负值的运用

* 实现效果： 每个盒子都有边框，且边框粗细相同。鼠标经过盒子时，盒子的四个边框变色

![margin负值应用](https://gitee.com/huqian025/my-images/raw/master/css/css高级技巧/margin负值应用.png)

* 思路：
    1. 让每个盒子margin往左侧移动-1px（如果边框为1px），正好压住相邻盒子边框
    2. 鼠标经过某个盒子的时候，提高当前盒子的层级（如果没有定位，则添加相对定位（保留位置），如果有定位，则加z-index）

![margin负值应用2](https://gitee.com/huqian025/my-images/raw/master/css/css高级技巧/margin负值应用2.png)
![margin负值应用3](https://gitee.com/huqian025/my-images/raw/master/css/css高级技巧/margin负值应用3.png)

![margin负值应用4](https://gitee.com/huqian025/my-images/raw/master/css/css高级技巧/margin负值应用4.png)
![margin负值应用5](https://gitee.com/huqian025/my-images/raw/master/css/css高级技巧/margin负值应用5.png)

#### 2、文字围绕浮动元素

* 实现效果：文字环绕在图片右侧

![文字围绕浮动元素](https://gitee.com/huqian025/my-images/raw/master/css/css高级技巧/文字围绕浮动元素.png)

* 思路：为图片盒子添加左浮动，文本则会环绕在右侧

![文字围绕浮动元素2](https://gitee.com/huqian025/my-images/raw/master/css/css高级技巧/文字围绕浮动元素2.png)

#### 3、行内块的巧妙运用

* 实现效果：

![行内块妙用](https://gitee.com/huqian025/my-images/raw/master/css/css高级技巧/行内块妙用.png)

* 思路：
    1. 行内块元素默认有间距
    2. 行内块元素添加`text-align: center;`属性，里面的行内元素和行内块元素都会水平居中

![行内块妙用2](https://gitee.com/huqian025/my-images/raw/master/css/css高级技巧/行内块妙用2.png)

#### 4、css三角强化

* 实现效果：

![css三角形强化](https://gitee.com/huqian025/my-images/raw/master/css/css高级技巧/css三角形强化.png)

* 思路：
    1. 将border-bottom和border-left设置为0
    2. 将border-top大小增加
    3. 将border-top设置为透明
    4. 将三角与需要的盒子拼接
    

![css三角形强化2](https://gitee.com/huqian025/my-images/raw/master/css/css高级技巧/css三角形强化2.png)
![css三角形强化3](https://gitee.com/huqian025/my-images/raw/master/css/css高级技巧/css三角形强化3.png)

![css三角形强化4](https://gitee.com/huqian025/my-images/raw/master/css/css高级技巧/css三角形强化4.png)
![css三角形强化5](https://gitee.com/huqian025/my-images/raw/master/css/css高级技巧/css三角形强化5.png)

![css三角形强化6](https://gitee.com/huqian025/my-images/raw/master/css/css高级技巧/css三角形强化6.png)
![css三角形强化7](https://gitee.com/huqian025/my-images/raw/master/css/css高级技巧/css三角形强化7.png)

### 八、css初始化（仅供参考）

不同浏览器对有些标签的默认值不同，为了消除不同浏览器对HTML文本呈现的差异，照顾浏览器的兼容，需要对css初始化。以京东css初始化代码为例：

```css
* {
  /* 所有标签内外边距清零 */
  margin: 0;
  padding: 0;
}

em, i {
  /* em 和 i 斜体文字不倾斜 */
  font-style: normal;
}

li {
  /* 去掉 li 的小圆点 */
  list-style: none;
}

img {
  /* border: 0; 照顾低版本浏览器，图片外面包含了链接会有边框的问题 */
  border: 0;
  /* 取消图片底部有空白缝隙的问题 */
  vertical-align: middle;
}

button {
  /* 当鼠标经过 button 按钮的时候，鼠标变成小手 */
  cursor: pointer;
}

a {
  /* a 标签字体颜色为 #666 */
  color: #666;
  /* 清除 a 标签下划线 */
  text-decoration: none;
}

button, input {
  /* \5B8B\4F53 就是宋体，这样写浏览器兼容性比较好 */
  font-family: Microsoft YaHei, Heiti SC, tahoma, arial, Hiragino Sans GB, "\5B8B\4F53", sans-serif;
}

body {
  /* css3 抗锯齿，让文字显示的更加清晰 */
  -webkit-font-smoothing: antialiased;
  background-color: #fff;
  font: 12px/1.5 Microsoft YaHei, Heiti SC, tahoma, arial, Hiragino Sans GB, "\5B8B\4F53", sans-serif;
  color: #666;
}

/* 清除浮动 */
.clearfix::after {
  visibility: hidden;
  clear: both;
  display: block;
  content: ".";
  height: 0;
}
.clearfix {
  /* 
    兼容 ie，以前使用 *zoom: 1;  css3中引入 zoom属性后可以直接使用 zoom: 1;
  */
  zoom: 1;
}
```
