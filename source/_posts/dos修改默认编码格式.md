---
title: dos修改默认编码格式
date: 2022-01-06 11:42:48
categories: 其他
tags:
- dos
---

#### 1、临时修改
* `chcp` 查看当前编码，936（简体中文），65001（UTF-8）
* `chcp 65001` 将当前窗口的编码改为65001

#### 2、永久修改
##### 2.1、powershell
1. 打开注册表，win+R => regedit
2. 进入HKEY_CURRENT_USER\Console\%SystemRoot%_System32_WindowsPowerShell_v1.0_powershell.exe，将CodePage的十进制值修改为65001。如果没有CodePage选项，右键新建DWORD(32位)，修改十进制值为65001
3. 重新打开powershell，输入chcp显示65001，说明修改成功

##### 2.2、cmd
1. 打开注册表，win+R => regedit
2. 进入HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Command Processor，添加一个叫autorun的字符串值，值是chcp 65001
3. 重新打开cmd，输入chcp显示65001，说明修改成功
