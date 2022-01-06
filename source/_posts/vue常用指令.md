---
title: vue常用指令
date: 2022-01-06 11:39:15
categories: vue
tags:
- vue
---

### 一、v-text

* 与 Mustache比较相似：都用于将数据显示在界面中（没有Mustache灵活）

![v-text](https://gitee.com/huqian025/my-images/raw/master/vue/vueDirectives+computed/v-text.png)

### 二、v-html

* 会将string的html解析出来并进行渲染

![v-html](https://gitee.com/huqian025/my-images/raw/master/vue/vueDirectives+computed/v-html.png)

### 三、v-show

* 用法与v-if相似，也用于决定一个元素是否渲染

#### 1、v-if和v-show对比

* v-if：当条件为false时，包含v-if指令的元素，根本就不会存在于dom中
* v-show：当条件为false时，v-show只是给我们的元素添加一个行内样式 display：none

#### 2、如何选择

* 需要在显示与隐藏之间频繁切换时，使用v-show
* 只有一次切换时，使用v-if（根据服务器传入数据决定是否渲染，大量使用）

### 四、v-if

* 与js中if类似
* Vue的条件指令可以根据表达式的值在DOM中渲染或销毁元素或组件
* 原理：v-if后面的条件为false时，对应的元素以及其子元素不会渲染，根本没有不会有对应的标签出现在DOM中

### 五、 v-else

* 与js中else类似
* Vue的条件指令可以根据表达式的值在DOM中渲染或销毁元素或组件

### 六、v-else-if

* 与js中else if类似
* Vue的条件指令可以根据表达式的值在DOM中渲染或销毁元素或组件

### 七、v-for

#### 1、遍历数组

##### 1.1、不含索引

* `<li v-for="item in names">{{ item }}</li>`

##### 1.2、含索引

* `<li v-for="(item, index) in names">{{ index+1 }}.{{ item }}</li>`

#### 2、遍历对象

##### 2.1、获取value

* `<li v-for="item in info">{{ item }}</li>`

##### 2.2、获取value和key

* `<li v-for="(value, key) in info">{{ value }}-{{ key }}</li>`

##### 2.3、获取value、key和index

* `<li v-for="(value, key, index) in info">{{ value }}-{{ key }}-{{ index }}</li>`

#### 3、v-for绑定key和不绑定key的区别

* 主要在于虚拟dom的复用，绑定key可以更好的复用
* 不推荐使用index作为key（在最后插入输入，重新渲染最后一条，使用index没有问题。但在中间插入时，后面数据的index都会发生改变，都会重新进行渲染，与不绑定key效果一样）
* 有相同父元素的子元素必须有独特的key，重复的key会造成渲染错误
* 官方示例：`<li v-for="item in items" :key="item.id">{{ item.text }}</li>`

![v-for](https://gitee.com/huqian025/my-images/raw/master/vue/vueDirectives+computed/v-for.png)

### 八、v-on

* 绑定事件监听器
* 语法糖 @

#### 1、v-on参数

##### 1.1、情况一（方法不需参数）

* 调用方法时可以加()，也可以不加()

##### 1.2、情况二（方法需要一个参数）

* 调用方法时不加()：该形参被赋值为原生事件event
* 调用方法时加()：()中传入形参对应的实参。如果不传参数，形参会被赋值为undefined

##### 1.3、情况三（方法需要其他参数和event）

* 调用方法时不加()：第一个形参被赋值为event，其他参数为undefined
* 调用方法时加()：()中传入形参对应的实参。如果传入参数个数少于方法需要的参数个数（包含不传），其他形参会被赋值为undefined。可以通过$event传入event事件

#### 2、修饰符

##### 2.1、.stop

* 调用event.stopPropagation()
* 停止冒泡

![v-on.stop](https://gitee.com/huqian025/my-images/raw/master/vue/vueDirectives+computed/v-on.stop.png)

##### 2.2、.prevent

* 调用event.prevenDefault
* 阻止默认行为（如点击按钮表单自动提交）

![v-on.prevent](https://gitee.com/huqian025/my-images/raw/master/vue/vueDirectives+computed/v-on.prevent.png)

##### 2.3、.{keyCode | keyAlias}

* 只当事件是从特定键触发时才触发回调

![v-on.keyCode](https://gitee.com/huqian025/my-images/raw/master/vue/vueDirectives+computed/v-on.keyCode.png)

##### 2.4、.once

* 只触发一次回调

![v-on.once](https://gitee.com/huqian025/my-images/raw/master/vue/vueDirectives+computed/v-on.once.png)

##### 2.5、.native

* 监听组件根元素的原生事件，不限于click事件（例如：调用组件时，直接使用@click无法监听到点击事件）
    * `<cpn @click="btnClick"></cpn> // 无法监听click`
    * `<cpn @click.native="btnClick"></cpn> // 可以监听click`

### 九、v-bind

* 动态绑定数据
* 语法糖 :

#### 1、绑定基本属性

![v-bind-base](https://gitee.com/huqian025/my-images/raw/master/vue/vueDirectives+computed/v-bind-base.png)

#### 2、绑定class

##### 2.1、对象语法

![v-bind-classObj](https://gitee.com/huqian025/my-images/raw/master/vue/vueDirectives+computed/v-bind-classObj.png)

##### 2.2、数组语法（了解）

![v-bind-classArr](https://gitee.com/huqian025/my-images/raw/master/vue/vueDirectives+computed/v-bind-classArr.png)

#### 3、绑定style

##### 3.1、对象语法

![v-bind-styleObj](https://gitee.com/huqian025/my-images/raw/master/vue/vueDirectives+computed/v-bind-styleObj.png)

##### 3.2、数组语法（了解）

![v-bind-styleArr](https://gitee.com/huqian025/my-images/raw/master/vue/vueDirectives+computed/v-bind-styleArr.png)

### 十、v-model

#### 1、修饰符

##### 1.1、lazy

* 默认情况下，v-model默认是在input事件中同步输入框的数据的，也就是说，一旦有数据发生改变对应的data中的数据就会自动发生改变
*  lazy修饰符可以让数据在失去焦点或者回车时才会更新
`<input type="text" v-model.lazy="message">`

##### 1.2、number

* 默认情况下，在输入框中无论我们输入的是字母还是数字，都会被当做字符串类型进行处理，但如果我们希望处理的是数字类型，那么最好直接将内容当做数字处理
* number修饰符可以让输入框中输入的内容自动转成数字类型
`<input type="number" v-model.number="age"><h2>{{typeof age}}</h2>`

##### 1.3、trim

* 如果输入的内容首尾有很多空格，通常我们希望将其去除，trim修饰符可以过滤内容左右两边的空格

#### 2、v-model原理

```html
<input type="text" v-model="message">
<!-- 等同于下面 -->
<input type="text" v-bind:value="message" v-on:input="message = $event.target.value">
<h2>{{ message }}</h2>
```

#### 3、v-model结合radio类型

![v-model结合radio](https://gitee.com/huqian025/my-images/raw/master/vue/vueDirectives+computed/v-model结合radio.png)

#### 4、v-model结合checkbox类型

![v-model结合checkbox](https://gitee.com/huqian025/my-images/raw/master/vue/vueDirectives+computed/v-model结合checkbox.png)

#### 5、v-model结合select类型

![v-model结合select1](https://gitee.com/huqian025/my-images/raw/master/vue/vueDirectives+computed/v-model结合select1.png)
![v-model结合select2](https://gitee.com/huqian025/my-images/raw/master/vue/vueDirectives+computed/v-model结合select2.png)

### 十一、v-slot

#### 1、插槽类型

##### 1.1、匿名插槽（隐含name=“default”）

```html
<div>
    <slot>匿名插槽默认内容</slot>
</div>
```

##### 1.2、具名插槽

```html
<div>
    <slot name="top"></slot>
    <slot name="center"></slot>
    <slot name="bottom"></slot>
</div>
```

##### 1.3、作用域插槽（父组件替换插槽的标签，但是内容由子组件来提供）

```html
<!-- 子组件中定义插槽，name为top（插槽名），data（属性名自己取）为items（子组件中的数据） -->
<div>
    <slot name="top" :data="items"></slot>
</div>

<!-- 父组件调用子组件，子组件名以cpn为例，slotProps（名称自定义） .data（调用子组件中绑定的data自定义属性）-->
<cpn slot="top" slot-scope="slotProps">{{ slotProps.data }}</cpn>
```

* 作用域插槽示例：子组件中包括一组数据，比如：pLanguages: ['JavaScript', 'Python', 'Swift', 'Go', 'C++']，需要在多个界面进行展示（某些界面是以水平方向一一展示的，某些界面是以列表形式展示的）

![v-slot1](https://gitee.com/huqian025/my-images/raw/master/vue/vueDirectives+computed/v-slot1.png)
![v-slot2](https://gitee.com/huqian025/my-images/raw/master/vue/vueDirectives+computed/v-slot2.png)

#### 2、v-slot

* 在vue2.6及已上版本，slot 和slot-scope已经开始废弃， 有了新的替代: v-slot，v-slot只能用在template 上，和组件标签上
* 语法糖 #

```html
<!-- 子组件中定义插槽方式不变 -->
<div>
    <slot name="top" :data="items"></slot>
</div>

<!-- 父组件调用子组件，使用v-slot代替slot和slot-scope -->
<!-- #top为v-slot:top的简写 -->
<cpn #top="slotProps">{{ slotProps.data }}</cpn>
```

### 十二、v-pre（不需要表达式）

* v-pre用于跳过这个元素和它子元素的编译过程，用于显示原本的Mustache语法

![v-pre](https://gitee.com/huqian025/my-images/raw/master/vue/vueDirectives+computed/v-pre.png)

### 十三、 v-cloak（不需要表达式）

* 这个指令保持在元素上直到关联实例结束编译（vue解析完成后，元素上的v-cloak属性消失）

![v-cloak](https://gitee.com/huqian025/my-images/raw/master/vue/vueDirectives+computed/v-cloak.png)

### 十四、v-once（不需要表达式）

* 元素和组件只渲染一次，不会随着数据的改变而改变

![v-once](https://gitee.com/huqian025/my-images/raw/master/vue/vueDirectives+computed/v-once.png)
