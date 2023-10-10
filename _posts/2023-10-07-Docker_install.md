---
layout: post
title: Docker提示wsl版本低
date: 2023-10-07
description: 
categories: Install
tags: Docker
author: NMt
mathjax: true
---

* content
{:toc}

安装软件[Docker Desktop][link_01]的时候出现了报错，造成Docker软件无法正常打开，显示的是WSL的版本太低，因此需要自己另外想办法将WSL升级一下，先尝试了一下在Power Shell中使用命令升级，然后发现会报错，因此找了教程，这里记录一下成功升级的方法。  

<div style='display: none'>
@@@@
</div>





{% raw %}

[Docker Desktop：https://www.docker.com/][link_01]直接在官网下载即可。  
[在线学习资源][link_02]  

## 准备  

1. 打开任务管理器，查看CPU下的虚拟化是否启用，若没有启用则需要开启。  
	
	![][pt_01]  

2. 打开“启用或关闭Windows功能”，检查“适用于Linux 的Windows子系统”是否勾选，若没有则需要勾选。  
	
	![][pt_02]  

## wsl2  

**提示：PowerShell需要以管理员的身份运行。**  

* 选择需要安装的版本  

```shell
wsl --list --online
```

![][pt_03]  

* 安装对应版本  

```shell
wsl --install -d Ubuntu-20.04

# wsl2升级包
# https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi
```

![][pt_04]  

## wsl更新到wsl2  

* 获取WSL2 Linux内核更新包并运行  

```shell
https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi
```

* 将WSL 2 设置为默认版本  

```shell
wsl --set-default-version 2
```

* 查看目前的WSL版本  

```shell
PS C:\WINDOWS\system32> wsl -l -v
  NAME            STATE           VERSION
* Ubuntu-20.04    Running         1
```

* 执行更新  

```shell
PS C:\WINDOWS\system32> wsl --set-version Ubuntu-20.04 2
正在进行转换，这可能需要几分钟时间...
有关与 WSL 2 的主要区别的信息，请访问 https://aka.ms/wsl2
转换完成。

# 重新查看目前的WSL版本
PS C:\WINDOWS\system32> wsl -l -v
  NAME            STATE           VERSION
* Ubuntu-20.04    Stopped         2
```

![][pt_05]  

## wsl2迁移  

```shell
# 终止正在运行的wsl
wsl --shutdown

# 检查当前wsl是否在运行
wsl -l -v


# 将需要迁移的Linux，进行导出
wsl --export Ubuntu-20.04 D:\Ubuntu.tar
# 导出完成之后，就需要将原有的分发进行卸载
wsl --unregister Ubuntu-20.04
# 然后将导出的文件放到需要保存的地方，进行导入即可
wsl --import Ubuntu-20.04 D:\Ubuntu_2004 D:\Ubuntu.tar --version 2

# 上述操作完成之后能够顺利打开WSL Ubuntu，但是显示以root身份登录
# 进行用户配置<username>是你前面注册的用户名（如果你的WSL Ubuntu的名称是Ubuntu-20.04，那么对应的可执行文件名为ubuntu2004.exe）
ubuntu.exe config --default-user <username>
ubuntu2004.exe config --default-user zkc
# 看下图进入对应目录,找到ubuntu2004.exe，在此目录进入cmd执行上面的命令
```

![][pt_07]  

## Docker-Desktop储存路径更改  

提示：现在的新版没有办法更改安装位置，按照下面的方法在安装的时候会报错，这里只是记录一下，如果使用旧版安装要记住这个操作是在安装之前进行  

```shell
# Docker Desktop的默认存储路径
C:\Users${用户文件}\AppData\Local\Docker

# 建立软连接：（第一个参数是原本的Docker-Desktop要安装的位置，第二个参数是你想要更改的位置）
mklink /j "C:\Program Files\Docker" "D:\Program Files\Docker"
```

## Docker-Desktop储存路径更改  

```shell
# Docker Desktop的默认存储路径
C:\Users${用户文件}\AppData\Local\Docker
```

```shell
PS C:\Users\Lenovo> wsl -l -v
  NAME                   STATE           VERSION
* Ubuntu-20.04           Running         2
  docker-desktop         Running         2
  docker-desktop-data    Running         2
PS C:\Users\Lenovo> wsl -l --all -v
  NAME                   STATE           VERSION
* Ubuntu-20.04           Running         2
  docker-desktop         Running         2
  docker-desktop-data    Running         2
  
# docker-desktop：保存的是程序
# docker-desktop-data: 保存的镜像
```

* 将Docker-Desktop退出，确保状态是关闭状态  

```shell
wsl --shutdown

# 结果
PS C:\Users\Lenovo> wsl -l -v
  NAME                   STATE           VERSION
* Ubuntu-20.04           Stopped         2
  docker-desktop         Stopped         2
  docker-desktop-data    Stopped         2
```

* 备份  

```shell
wsl --export docker-desktop E:\wsl2\docker\docker-desktop.tar
wsl --export docker-desktop-data E:\wsl2\docker\docker-desktop-data.tar
```

* 注销  

```shell
wsl --unregister docker-desktop
wsl --unregister docker-desktop-data
```

* 导入  

```shell
wsl --import docker-desktop D:\wsl2\docker\docker-desktop D:\wsl2\docker\docker-desktop.tar --version 2
wsl --import docker-desktop-data D:\wsl2\docker\docker-desktop-data D:\wsl2\docker\docker-desktop-data.tar --version 2
```

![][pt_06]  


## win系统安装docker 需要注意本机端口是否被使用，如果使用映射的时候就没法用了  

```shell
# 排查是否有程序占用了9091端口
netstat -ano | findstr 9091

# 通过cmd命令查看哪些端口被禁用TCP协议
netsh interface ipv4 show excludedportrange protocol=tcp
```


## cmd中wsl命令  

```shell
列出分发：wsl -l
运行指定分发：wsl -d <分发>
更改新分发的默认安装版本：wsl --set-default-version <Version>
将分发设置为默认值：wsl -s <分发>
更改指定分发的版本：wsl --set-version <分发> <版本>
立即终止所有运行的分发及 WSL2：wsl --shutdown
终止指定的分发（相当于关机）：wsl -t <分发>
注销分发并删除根文件系统：wsl --unregister <分发>
将指定的 tar 文件作为新分发导入：wsl --import <Distro> <InstallLocation> <FileName>
将分发导出到 tar 文件：wsl --export <Distro[分发]> <FileName[文件名，包含文件全路径]>
```


转载请注明：[南梦婷的博客](https://norah2.github.io) » [点击阅读原文](https://norah2.github.io/2023/10/07/docker_install/) 

<!--本文用到的链接-->

[pt_01]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/81_Docker_install/01.png  
[pt_02]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/81_Docker_install/02.png  
[pt_03]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/81_Docker_install/03.png  
[pt_04]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/81_Docker_install/04.png  
[pt_05]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/81_Docker_install/05.png  
[pt_06]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/81_Docker_install/06.png  
[pt_07]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/81_Docker_install/07.png  
[link_01]: https://www.docker.com/  
[link_02]: https://github.com/RumbleDB/bigdata-exercises/tree/master/Big_Data_For_Engineers  

{% endraw %}
