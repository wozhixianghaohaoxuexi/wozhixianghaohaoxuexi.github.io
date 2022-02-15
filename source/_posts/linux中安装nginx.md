---
title: linux中安装nginx
date: 2022-01-06 11:38:01
categories: linux
tags: 
- linux
- nginx
---

#### 1、安装nginx依赖包
```powershell
# 安装gcc-c++编译器
yum install gcc-c++
yum install -y openssl openssl-devel
# 安装pcre包
yum install -y pcre pcre-devel
# 安装zlib包
yum install -y zlib zlib-devel
```
#### 2、下载并解压安装包
```powershell
# 安装wget命令
yum -y install wget
# 新建文件夹
mkdir /usr/local/nginx
cd /usr/local/nginx
# 下载tar包，下载地址可在官网查看 
wget http://nginx.org/download/nginx-1.20.2.tar.gz
# 解压
tar -zxvf nginx-1.20.2.tar.gz// 进入解压后目录cd nginx-1.20.2
```
#### 3、安装nginx
```powershell
# 使用nginx默认配置
./configure
# 编译安装
make
make install
# 查找安装路径
whereis nginx
# 进入sbin目录，可以看到有一个可执行文件nginx，直接./nginx执行就OK了。
cd sbin
./nginx
# 查看是否启动成功
ps -ef | grep nginx
```
#### 4、将nginx路径配置到系统环境变量
```powershell
# 编辑系统环境变量
vim /etc/profile
# 将 nginx 路径添加至环境变量
# 添加路径并使用 NGINX_HOME 接收
export NGINX_HOME=/usr/local/nginx/sbin
# 将 NGINX_HOME 添加至环境变量 PATH
export PATH=$PATH:$NGINX_HOME
# 重新加载环境变量
source /etc/profile
# 在 root 路径下使用 nginx 命令
cd /
nginx -h
```
#### 5、nginx其他命令
**未配置环境变量，需将nginx替换为路径/usr/local/nginx/sbin/nginx**
```powershell
# 查看nginx帮助信息
nginx -h
# 查看nginx的版本号
nginx -v
# 显示nginx的版本号和编译信息
nginx -V
# 启动nginx
start nginx
# 快速停止和关闭nginx
nginx -s stop
# 正常停止或关闭nginx，完成已接收的连接请求再退出
nginx -s quit
# 重新加载配置文件
nginx -s reload
# 测试nginx配置文件的正确性
nginx -t
```

