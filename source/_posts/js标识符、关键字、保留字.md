---
title: js标识符、关键字、保留字
date: 2022-01-06 11:35:50
categories: js
tags:
- js
- 标识符
---

#### 1、标识符
标识符：指开发人员为变量、属性、函数、参数取的名字。

**标识符可以包含字母、数字、下划线、$符号，但是不能以数字开头，并区分大小写，标识符不能是关键字或保留字。**

#### 2、关键字
关键字：指JS本身已经使用了的字，不能再用他们充当变量名、方法名。
包括： break、case、catch、class、const、continue、debugger、default、delete、do、else、export、extends、finally、for、function、if、import、in、instanceof、new、return、super、switch、this、throw、try、typeof、var、void、while、with、yield

#### 3、保留字
保留字：**实际就是预留的“关键字”，意思是现在虽然还不是关键字，但是未来可能会成为关键字**，同样不能使用他们当变量名或方法名。
包括：enum、abstract、boolean、byte、char、double、final、float、goto、int、long、native、short、synchronized、transient、volatile
只在严格模式中被当成保留关键字：implements、interface、let、package、private、protected、public、static
只在模块代码中被当成保留关键字：await

**另外，直接量null、true和false同样不能被当成标识使用**

#### 4、标识符为关键字报错信息
![标识符为关键字报错信息](https://gitee.com/huqian025/my-images/raw/master/js/js%E6%A0%87%E8%AF%86%E7%AC%A6/%E5%85%B3%E9%94%AE%E5%AD%97%E6%8A%A5%E9%94%99.png)

#### 5、标识符为保留字报错信息
![标识符为保留字报错信息](https://gitee.com/huqian025/my-images/raw/master/js/js%E6%A0%87%E8%AF%86%E7%AC%A6/%E4%BF%9D%E7%95%99%E5%AD%97%E6%8A%A5%E9%94%99.png)

[关键字、保留字参考文档](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Lexical_grammar)

