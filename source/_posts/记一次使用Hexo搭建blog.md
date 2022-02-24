---
title: 记一次使用Hexo + GitHub Pages搭建blog
date: 2022-01-26 14:53:51
categories:
  - 其他
tags:
  - hexo
---

前几天在掘金摸鱼看到首页推荐了一个Hexo+GitHub Pages自己搭建博客的文章，于是开始自己摸索去搭建的试一下。按照[官网的步骤](https://hexo.io/zh-cn/docs/)初始化了一个Hexo项目，并根据官方的GitHub Pages部署文档尝试着部署项目。

---

本地项目搭建完成，github pages搭建完成，ssh配置完成后：

#### 1. 修改_config.yml配置文件

在项目根目录下的_config.yml配置文件最后改为
```yaml
deploy:
  type: git
  repository: 远程仓库地址
  branch: master
```

#### 2. 安装hexo-deployer-git插件

安装部署插件hexo-deployer-git，必须安装不然会报错，[详情可以看官方这个issues](https://github.com/hexojs/hexo/issues/1040)
```powershell
npm install hexo-deployer-git --save
```

#### 3. 部署

执行部署上传命令即可访问线上博客项目，g 是 generate 缩写，d 是 deploy 缩写。
```powershell
# 生成静态文件
hexo g
# 启动本地服务
hexo server
# 部署网站
hexo d
# 文件生成后立即部署网站
hexo g -d
# 清除缓存，建议每次部署前先清除缓存
hexo clean
```

#### 4. 新建文章

```powershell
# 新建一篇文章
hexo new [layout] <title>
# 例如
hexo new "post title with whitespace"
```

如果没有设置 layout 的话，默认使用_config.yml 中的 default_layout 参数代替。如果标题包含空格的话，需要使用引号括起来。**title必须设置**。

| 参数          | 描述                                        |
| ------------- | ------------------------------------------- |
| -p, --path    | 自定义新文章的路径                          |
| -r, --replace | 如果存在同名文章，将其替换                  |
| -s, --slug    | 文章的Slug，作为新文章的文件名和发布后的URL |

```powershell
# 创建一个 source/about/me.md 文件，且标题为 About me
hexo new page --path about/me "About me"
```

#### 5. 分类

**分类具有顺序性和层次性，例如Foo, Bar 不等于 Bar, Foo**
1. 子分类
```yaml 
categories:
    - Diary
    - Life
```
分类 Life 会成为 Diary 的子分类，而不是并列分类

2. 并列分类
```yaml
categories:
    - [Diary]
    - [Life]
```
Diary 和 Life 为并列分类

3. 并列+子分类
```yaml
categories:
    - [Diary, PlayStation]
    - [Diary, Games]
    - [Life]
```
PlayStation 和 Games 分别都是父分类 Diary 的子分类，同时 Life 是一个没有子分类的分类

#### 6. 标签

**标签没有顺序和层次**
```yaml
tags:
    - PS3
    - Games
```
PS3 和 Games 是两个独立的标签，没有层级和顺序

#### 结语

官方部署文档如下：

![官方部署文档](https://gitee.com/huqian025/my-images/raw/master/其他/记一次使用Hexo搭建blog/hexo官方部署文档.png)

再使用Travis CI时，官方提示需要添加一个plan才能使用，但添加plan**需要stripe信用卡信息**，直接劝退。

![Travis CI添加plan](https://gitee.com/huqian025/my-images/raw/master/其他/记一次使用Hexo搭建blog/Travis_CI添加plan.png)
![Travis CI添加plan-中文](https://gitee.com/huqian025/my-images/raw/master/其他/记一次使用Hexo搭建blog/Travis_CI添加plan-中文.png)

最后参照了一下[大佬的文章](https://segmentfault.com/a/1190000017986794#item-9)，本来都想记录分享一下搭建过程的，但是大佬写的真的太细了。。
