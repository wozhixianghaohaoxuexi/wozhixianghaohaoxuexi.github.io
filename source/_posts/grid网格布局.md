---
title: grid网格布局
date: 2022-02-14 11:25:12
categories:
  - css
tags:
---

#### 一、容器属性

##### 1. display：是否开启grid布局

```css
/*
    grid：指定容器采用grid布局，容器元素默认为块级元素
    inline-grid：指定容器采用grid布局，容器元素设为行内元素
*/
.container {
    display: grid | inline-grid;
}
```

##### 2. grid-template-columns属性、grid-template-rows属性

* gird-template-columns属性用来定义每一列的列宽
* grid-template-rows属性用来定义每一行的行高

```css
.container {
    display: grid;
    /* 使用绝对单位，定义三列，每列列宽为100px */
    grid-template-columns: 100px 100px 100px;
    /* 使用百分比，定义三列，每列列宽为33.33% */
    grid-template-columns: 33.33% 33.33% 33.33%;
}
```

**repeact()**：简化重复的值。该函数接受两个参数，第一个是重复的次数，第二个是要重复的值

```css
.container {
    display: grid;
    /* 定义三列，每列列宽为100px */
    grid-template-columns: repeat(3, 100px);
    /* 定义六列，第一、四列列宽为100px，第二、五列列宽为30px，第三、六列列宽为70px */
    grid-template-columns: repeat(2, 100px 30px 70px);
}
```

**auto-fill关键字**：自动填充，让一行/列中尽可能容纳多的单元格

```css
.container {
    display: grid;
    /* 每列列宽为100px，自动填充直到容器不能放置更多的列，列数视容器宽度而定 */
    grid-template-columns: repeat(auto-fill, 100px);
}
```

**fr关键字**：表示多行/列之间的比例关系

```css
.container {
    display: grid;
    /* 定义两列，列宽相同 */
    grid-template-columns: 1fr 1fr;
    /* 定义三列，第一列列宽为150px，第三列列宽为第二列列宽的两倍 */
    grid-template-columns: 150px 1fr 2fr;
}
```

**minmax()**：产生一个长度范围，表示长度在范围之中。该函数接受两个参数，分别为最小值和最大值

```css
.container {
    /* 定义三列，第一列和第二列列宽相等，第三列列宽不小于100px，不大于1fr */
    grid-template-columns: 1fr 1fr minmax(100px, 1fr);
}
```

**auto关键字**：由浏览器决定长度

```css
.container {
    /* 定义三列，第一列和第三列列宽为100px，第二列列宽由浏览器决定，基本等于该列单元格的最大宽度 */
    grid-template-columns: 100px auto 100px;
}
```

##### 3. grid-row-gap属性、grid-column-gap属性、grid-gap属性

* grid-row-gap 属性设置行与行的间隔（行间距）
* grid-column-gap 属性设置列与列的间隔（列间距）
* grid-gap 属性是 grid-column-gap 和 grid-row-gap 的合并简写形式

```css
.container {
    /* 行间距为20px */
    grid-row-gap: 20px;
    /* 列间距为20px */
    grid-column-gap: 20px;
    /* 行间距为20px，列间距为20px */
    grid-gap: 20px 20px;
    /* 如果省略第二个值，浏览器认为第二个值等于第一个值 */
    grid-gap: 20px;
}
```

> 根据最新标准，以上三个属性名的 grid- 前缀已经删除，grid-column-gap 写成 column-gap，grid-row-gap 写成 row-gap，grid-gap 写成 gap

##### 4. grid-template-areas属性

* grid-template-areas属性用于定义一个区域，一个区域由单个或多个单元格组成

```css
.container {
    display: grid;
    grid-gap: 10px;
    grid-template-columns: repeat(3, 120px);
    /* 划分六个单元格，小数点代表空的单元格，也就是没有用到该单元格 */
    grid-template-areas: ". header header"
                                "sidebar content content";
}

/* 将类 .header .sidebar .content 所在的元素放在上面 grid-template-areas 中定义的 header sidebar content 区域中 */
.header {
    grid-area: header;
}
.sidebar {
    grid-area: sidebar;
}
.content {
    grid-area: content;
}
```

##### 5. grid-auto-flow属性

* grid-auto-flow 属性指定容器子元素的排列方式。默认顺序是“先行后列”，即先填满第一行，再开始放入第二行

```css
/*
    row：先行后列
    row dense：先行后列，尽可能紧密排布，尽量不出现空格
    column：先列后行
    column dense：先列后行，尽可能紧密排布，尽量不出现空格
*/
.container {
    grid-autp-flow: row | row dense | column | column dense;
}
```

##### 6. justify-items属性、align-items属性、place-items属性

* justify-items 属性设置单元格内容的水平位置
* align-items 属性设置单元格内容的垂直位置
* place-items 属性是 align-items 属性和 justify-items 属性的合并简写形式。如果省略第二个值，浏览器会认为两个属性值相同

```css
/*
    start：对齐单元格的起始边缘
    end：对齐单元格的结束边缘
    center：单元格内部居中
    stretch：拉伸，占满单元格的整个宽度（默认值）
*/
.container {
    justify-items: start | end | center | stretch;
    align-items: start | end | center | stretch;
    place-items: <align-items> <justify-items>
}
```

##### 7. justify-content属性、align-content属性、place-content属性

* justify-content 属性是整个内容区域在容器里面的水平位置
* align-content 属性是整个内容区域在容器里面的垂直位置
* place-content 属性是 align-content 属性和 justify-content 属性的合并简写形式。如果省略第二个值，浏览器会认为两个属性值相同

```css
/*
    start：对齐容器的起始边框
    end：对齐容器的结束边框
    center：容器内部居中
    stretch：项目大小没有指定时，拉伸占据整个网格容器
    space-around：每个项目两侧的间隔相等，项目之间的间隔比项目与容器边框的间隔大一倍
    space-between：项目与项目的间隔相等，项目与容器边框之间没有间隔
    space-evenly：项目与项目的间隔相等，项目与容器边框之间也是同样长度的间隔
*/
.container {
    justify-content: start | end | center | stretch | space-around | space-between | space-evenly;
    align-content: start | end | center | stretch | space-around | space-between | space-evenly;
    place-content: <align-content> <justify-content>
}
```

##### 8. grid-auto-columns属性、grid-auto-rows属性

* grid-auto-columns 属性和 grid-auto-rows 属性用来设置浏览器自动创建的多余网格的列宽和行高。它们的写法与 grid-template-columns 和 grid-template-rows 完全相同。如果不指定这两个属性，浏览器会根据单元格内容的大小，决定新增网格的列宽和行高

##### 9. grid-template属性、grid属性（简写形式，不建议使用）

* grid-template 属性是 grid-template-columns、grid-template-rows 和 grid-template-areas 三个属性的合并简写形式
* grid 属性是 grid-template-rows、grid-template-columns、grid-template-areas、grid-auto-rows、grid-auto-columns、grid-auto-flow 六个属性的合并简写形式

#### 二、项目属性

##### 1. grid-column-start属性、grid-column-end属性、grid-row-start属性、grid-row-end属性

* grid-column-start 属性表示项目左边框所在的垂直网格线
* grid-column-end 属性表示项目右边框所在的垂直网格线
* grid-row-start 属性表示项目上边框所在的水平网格线
* grid-row-end 属性表示项目下边框所在的水平网格线

```css
.item {
    grid-column-start: 1;
    grid-column-end: 2;
    grid-row-start: 1;
    grid-row-end: 2;
}
```

##### 2. grid-column属性、grid-row属性

* grid-column 属性是 grid-column-start 和 grid-column-end 的合并简写形式
* grid-row 属性是 grid-row-start 和 grid-row-end 的合并简写形式

```css
.item{
    grid-column: <grid-column-start> / <grid-column-end>;
    grid-row: <grid-row-start> / <grid-row-end>;
    
    /* 例如 */
    grid-column: 1 / 2;
    grid-row: 1 / 2;
}
```

##### 3. grid-area属性

* grid-area 属性指定项目放在哪个区域（在容器属性 grid-template-areas 中提及）
* grid-area 属性还可以用作 grid-column-start、grid-column-end、grid-row-start、grid-row-end 的合并简写形式，直接指定项目的位置

```css
.item {
    grid-area: <grid-row-start> / <grid-column-start> / <grid-row-end> / <grid-column-end>;
    
    /* 例如 */
    /* header 为 grid-template-areas 中定义的区域 */
    grid-area: header;
    /* 简写形式，直接指定项目位置 */
    grid-area: 1 / 1 / 2 / 2;
}
```

##### 4. justify-self属性、align-self属性、place-self属性

* justify-self 属性设置单元格内容的水平位置。跟 justify-items 属性的用法完全一致，但只作用于单个项目
* align-self 属性设置单元格内容的垂直位置。跟 align-items 属性的用法完全一致，但之作用于单个项目
* place-self 属性是 align-self 属性和 justify-self 属性的合并简写形式。如果忽略第二个值，浏览器会认为两个属性值相等

```css
/*
    start：对齐单元格的起始边缘
    end：对齐单元格的结束边缘
    center：单元格内容居中
    stretch：拉伸，占满单元格的整个宽度（默认值）
*/
.item {
    justify-self: start | end | center | stretch;
    align-self: start | end | center | stretch;
    place-self: <align-self> / <justify-self>;
}
```


> 参考文档：
> [阮一峰老师的grid布局教程](https://www.ruanyifeng.com/blog/2019/03/grid-layout-tutorial.html)
> [掘金grid布局文章](https://juejin.cn/post/6854573220306255880#heading-7)
