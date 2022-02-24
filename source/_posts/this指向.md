---
title: this指向
date: 2022-02-24 16:13:35
categories:
  - js
tags:
  - this指向
---

#### 一、如何判断函数中 this 指向

1. 检查 `.` 左边是谁调用这个函数。例如 `xiaoming.getName();` getName 函数里面有this，然后 `.` 旁边是 xiaoming，那么 this 就是指向 xiaoming。这种叫做 Implicit Binding（隐式绑定）
2. 如果 `.` 左边不是一个对象，那就检查有没有用到 bind、apply、call 这三个方法，如果有，this 就指向这三个方法的第一个参数。这种叫做 explicit binding（显式绑定）
3. 如果前两个条件不满足，检查是否通过 new 关键字调用该函数，如果是，this 就指向 new 创建的对象。这种叫做 new binding（new 绑定）
4. 如果前三个条件不满足，检查 this 所在的函数是否为箭头函数，如果是，this 就指向箭头函数所绑定的对象（**箭头函数中没有this，它的this继承外部函数的作用域。箭头函数在哪个位置定义，里面的this就跟这个位置的this指向相同**）。这种叫做 lexical binding（词汇绑定）
5. 以上条件都不满足，如果不是严格模式，this 指向 window 对象，严格模式为 undefined

#### 二、bind、apply、call 三者之间的区别

* 联系：
1. 都可以改变函数的 this 指向
2. 第一个参数都是 this 要指向的对象
3. 都可以利用后续参数传参

* 区别：
1. call 和 apply 都是对函数直接调用（立即调用函数），而 bind 方法会返回一个函数，再通过()调用
2. call 和 bind 传入的参数和调用方法中的形参一一对应，而 apply 需要将一一对应的参数封装为一个数组。bind 可以向 call 一样在对象后面跟上参数，也可以在返回函数调用时传入参数

```js
function fn(name, age) {
  console.log(name, age)
  console.log(this)
}
fn('a', 18);    // a 18 window
let obj = {};
fn.call(obj, 'b', 19);  // b 19 obj
fn.apply(obj, ['c', 20]);   // c 20 obj
fn.bind(obj)('d', 21);  // d 21 obj
```
