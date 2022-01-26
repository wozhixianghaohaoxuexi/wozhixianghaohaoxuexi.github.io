---
title: 使用vscode构建uniapp项目
date: 2022-01-26 15:10:05
categories:
  - uniapp
tags:
  - vscode
  - uniapp
---

#### 1. 全局安装vue-cli
```powershell
npm install -g @vue/cli
```

#### 2. 通过vue-cli创建uniapp项目
```powershell
# my-project为项目名称
vue create -p dcloudio/uni-preset-vue my-project
```
项目创建过程中，会提示选择模板，选择默认模板即可

#### 3. 安装语法提示
1. vscode插件商店中搜索并安装vue语法提示插件vetur
2. 安装uniapp框架提供的组件语法提示

```powershell
# 安装uniapp框架提供的组件语法提示
npm i @dcloudio/uni-helper-json
```

#### 4. 导入hbuildx自带的代码块

从 github 下载 [uni-app 代码块](https://github.com/zhetengbiji/uniapp-snippets-vscode)，放到项目目录下的 .vscode 目录即可拥有和 HBuilderX 一样的代码块

#### 5. 运行发布项目
```powershell
# 运行项目
npm run dev:%PLATFORM%

# npm run serve指令实际执行的是npm run dev:h5，即将项目运行至浏览器
npm run serve

# 发布项目
npm run build:%PLATFORM%
```

%PLATFORM% 可取值如下：
| 值         | 平台         |
| ---------- | ------------ |
| h5         | H5           |
| mp-alipay  | 支付宝小程序 |
| mp-baidu   | 百度小程序   |
| mp-weixin  | 微信小程序   |
| mp-toutiao | 头条小程序   |
| mp-qq      | qq小程序     |

[参考文档](https://ask.dcloud.net.cn/article/36286)
