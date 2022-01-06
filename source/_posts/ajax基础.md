---
title: ajax基础
date: 2022/1/6 11:14:25
tags: ajax
categories: ajax
---

#### 0、HTTP协议

* HTTP 超文本传输协议，协议详细规定了浏览器与万维网服务器之间互相通信的规则。
* 请求报文：
![请求报文](https://gitee.com/huqian025/my-images/raw/master/ajax/ajax%E5%9F%BA%E7%A1%80/%E8%AF%B7%E6%B1%82%E6%8A%A5%E6%96%87.png)

* 响应报文：
![响应报文](https://gitee.com/huqian025/my-images/raw/master/ajax/ajax%E5%9F%BA%E7%A1%80/%E5%93%8D%E5%BA%94%E6%8A%A5%E6%96%87.png)

#### 1、Ajax简介

* Ajax全称为Asynchronous JavaScript And XML，就是异步的JS和XML
* 通过Ajax可以在浏览器中向服务器发送异步请求，最大的优势：**无刷新获取数据**
* Ajax不是新的编程语言，而是一种将现有的标准组合在一起使用的新方式

#### 2、XML简介

* XML可扩展标记语言，被设计用来传输和存储数据
* XML和HTML类似，不同的是HTML中都是预定义标签，而XML中没有预定义标签，全部都是自定义标签，用来表示一些数据

```xml
<!-- 例如：有一个学生数据name="孙悟空"; age=18; gender="男" -->
<!-- 用XML表示： -->
<student>
    <name>孙悟空</name>
    <age>18</age>
    <gender>男</gender>
</student>
```

* 现在已经被JSON取代了
```json
// 用JSON表示：
{
    "name": "孙悟空",
    "age": 18,
    "gender": "男"
}
```

#### 3、Ajax特点

##### 3.1、Ajax优点

* 可以在无需刷新页面与服务器进行通信
* 允许根据用户事件来更新部分页面内容

##### 3.2、Ajax缺点

* 没有浏览历史，不能回退
* 存在跨域问题
* SEO优化不友好

#### 4、发送ajax请求

##### 4.1、原生ajax

```javascript
// 获取button元素
const btn = document.getElementsByTagName('button')[0]
const box = document.getElementsByClassName('box')[0]
// 绑定事件
btn.onclick = function (){
    // 1. 创建对象
    const xhr = new XMLHttpRequest()
    // 2. 初始化 设置请求类型和url
    xhr.open('GET', 'http://127.0.0.1:8000/server')
    // 3. 发送
    xhr.send()
    // 4. 事件绑定 处理服务端返回的结果
    // on when 当...时候
    // readystate 是xhr对象中的属性，表示状态0,1,2,3,4（分别对应未初始化，open方法调用完毕，send方法调用完毕，服务端返回部分结果，服务端返回所有结果）
    // change 改变
    xhr.onreadystatechange = function(){
      // 判断服务端返回所有结果
      if(xhr.readyState === 4){
        // 判断相应状态码，2开头都是成功
        if(xhr.status >= 200 && xhr.status < 300){
          // 处理结果
          console.log(xhr.status) // 响应状态码
          console.log(xhr.statusText) // 响应状态字符串
          console.log(xhr.getAllResponseHeaders()) // 所有响应头
          console.log(xhr.response) // 响应体
          box.innerHTML = xhr.response
        }
      }
    }
}
```

##### 4.2、jquery发送ajax请求

```javascript
$('button').eq(0).click(function(){
    $.get('http://127.0.0.1:8000/jquery-server', { a: 100, b: 200}, function(data){
      console.log(data)
    }, 'json')
  })
  $('button').eq(1).click(function(){
    $.post('http://127.0.0.1:8000/jquery-server', { a: 100, b: 200}, function(data){
      console.log(data)
    })
  })
  $('button').eq(2).click(function(){
    $.ajax({
      // url
      url: 'http://127.0.0.1:8000/jquery-server',
      // 参数
      data: { a: 100, b: 200},
      // 请求类型
      type: 'GET',
      // 响应体结果
      dataType: 'json',
      // 成功回调
      success: function(data){
        console.log(data)
      },
      // 超时时间
      timeout: 2000,
      // 失败回调
      error: function(){
        console.log('出错了')
      }
    })
  })
```

##### 4.3、axios发送ajax请求

```javascript
const btns = document.querySelectorAll('button')
  // 配置baseURL
  axios.defaults.baseURL = 'http://127.0.0.1:8000'
  btns[0].onclick = function () {
    // GET请求
    axios.get('axios-server', {
      // url参数
      params: {
        id: 100,
        vip: 7
      },
      // 请求头信息
      headers: {
        name: 'abc',
        age: 18
      }
    }).then(res => {
      console.log(res)
    })
  }
  btns[1].onclick = function () {
    // POST请求
    axios.post('axios-server', 
      {
        // 请求体
        username: 'admin',
        password: 'admin'
      }, 
      {
        // url参数
        params: {
          id: 100,
          vip: 7
        },
        // 请求头信息
        headers: {
          name: 'abc',
          age: 18
        }
      }
    ).then(res => {
      console.log(res)
    })
  }
  btns[2].onclick = function(){
    axios({
      // 请求类型
      method: 'POST',
      // url
      url: 'axios-server',
      // url参数
      params: {
        id: 100,
        vip: 7
      },
      // 请求头信息
      headers: {
        name: 'abc',
        age: 18
      },
      // 请求体
      data: {
        username: 'admin',
        password: 'admin'
      }
    }).then(res => {
      console.log(res)
    })
  }
```

##### 4.4、fetch发送ajax请求

```javascript
const btn = document.querySelector('button')
  btn.onclick = function(){
    fetch('http://127.0.0.1:8000/fetch-server', {
      // 请求类型
      method: 'POST',
      // 请求头
      headers: {
        name: 'abc'
      },
      // 请求体
      body: 'username=admin&password=admin'
    }).then(res => {
      // console.log(res)
      return res.json()
    }).then(res => {
      console.log(res)
    })
  }
```

#### 5、跨域

##### 5.1、同源策略

* 1. 同源策略是浏览器的一种安全策略
* 2. 同源：协议、域名、端口号必须完全相同
* 3. 违背同源策略就是跨域

##### 5.2、解决跨域-jsonp

* 1. jsonp是什么：jsonp是一个非官方的跨域解决方案，凭借程序员的聪明才智开发出来，只支持get请求
* 2. jsonp怎么工作的：在网页中有一些标签天生具有跨域能力，比如img、link、iframe和script。jsonp就是利用script标签的跨域能力来发送请求的
* 3. jsonp的使用：
    * 1）动态创建一个script标签`var script = document.createElement('script')`
    * 2）设置script的src属性，设置回调函数`script.src = 'http://127.0.0.1:8000/check-username'`
    * 3）将script标签插入到文档中`document.body.appendChild(script)`

##### 5.3、解决跨域-设置CORS响应头

* 1. CORS是什么：跨域资源共享，CORS是官方的跨域解决方案，它的特点是不需要在客户端做任何特殊的操作，完全在服务器中进行处理，支持post和get请求。跨域资源共享标准新增了一组HTTP首部字段，允许服务器声明哪些源站通过浏览器访问哪些资源
* 2. CORS是怎么工作的：CORS是通过设置一个响应头来告诉浏览器，该请求允许跨域，浏览器收到该响应以后就会对响应放行
* 3. CORS的使用：
    * 在服务端设置请求的响应头`response.setHeader('Access-Control-Allow-Origin', '*')`

![解决跨域](https://gitee.com/huqian025/my-images/raw/master/ajax/ajax%E5%9F%BA%E7%A1%80/%E8%A7%A3%E5%86%B3%E8%B7%A8%E5%9F%9F.png)
