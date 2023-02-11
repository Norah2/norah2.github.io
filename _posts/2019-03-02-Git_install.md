---
layout: post
title: Git 安装教程
date: 2019-03-02
categories: Install
tags: Git Install Blog
author: NMt
mathjax: true
---

* content
{:toc}

Git + SSH配置流程  

<div style='display: none'>
@@@@
</div>





{% raw %}
由于篇幅问题，这里又新写了一篇博客用作补充的，这里专门讲如何安装 Git ，外加配置 SSH。
## Git安装教程

由于我现在只在win10的操作系统上用过这个，就先只写windows上安装Git的详细过程。

1、先进入Git官网下载对应版本的Git安装包，跳过agree协议和修改安装路径的步骤。

2、选择安装组件，推荐全选

![][pt_01]  

在我自己安装的时候底下会多出一个复选框，可能和我找到的这个教程版本不相同的原因，那个我没有勾。

![][pt_02]  

3、菜单文件夹 -- 默认

![][pt_03]

4、修改系统的环境变量--建议选择上面两个。我选择的第一个

![][pt_04] 


5、SSH的证书的选择--我选的第一个

![][pt_05] 


6、配置行尾结束符--我选的第三个

![][pt_06] 


说实话，我不太懂行尾结束符有什么用，但是我就这么配置了

![][pt_07] 


7、配置终端仿真

![][pt_08] 


大多数其他Cygwin/MSYS终端一样，MinTTY也是基于pseudo终端("pty")设备的。但是MinTTY并不能完全替代windows的命令提示符。windows上自带简单的文本输出的原生态的命令提示符通常可以很好的工作，但交互性更好的诸如MinTTY这样的应用程序却可能出现故障——虽然通常都有应对方案。这就是为什么MinTTY不能完全替代windows自带的命令提示符。

8、其他的配置--默认

![][pt_09] 


9、使用测试界面

![][pt_10] 


10、使用过程问题解决汇总--我没有出现这种问题

![][pt_11] 

-------
安装教程结束



## 配置SSH

1. 首先设置一下git的邮箱和用户名  
	输入以下两行命令：
```
git config --global user.email "your email"
git config --global user.name "your name"
```
**注：引号里面的请填写自己的邮箱和用户名**

2. 创建 SSH key  
   
   删除.ssh文件下的 known_hosts
   git 输入命令：
```
$ ssh-keygen -t rsa -C "your@email.com"（请填你设置的邮箱地址）
```

   接着出现：  
   
	```
	Generating public/private rsa key pair.

	Enter file in which to save the key (/Users/your_user_directory/.ssh/id_rsa):
	```
   
   请直接按下回车
   然后系统会自动在.ssh文件夹下生成两个文件，id_rsa和id_rsa.pub，用记事本打开id_rsa.pub
   windows 在 `C:\Users\username\.ssh\`  路径下有 `id_rsa` 和 `id_rsa.pub` 两个文件，`id_rsa.pub`文件中存储着 SSH 公钥。`id_rsa` 存储着私钥。

   以上两个步骤整体流程如下图：
![][pt_14]  

3. 登陆Github，打开“Accounting setting”，“SSH Keys”页面：
   ![][pt_12] 

   点“Add Key”，你就应该看到已经添加的Key：
   ![][pt_13] 


------
由于 Git 是在很久之前配置的，因此图片大部分不是自己的，用的别人的，但是过程是自己的。如有错误，欢迎与我联系并指正~

转载请注明：[南梦婷的博客](https://norah2.github.io) » [点击阅读原文](https://norah2.github.io/2019/03/02/Git_install/) 


<!--以下是本文用到的链接-->

[pt_01]: {{site.image_src_prefix}}/02_Git_install/01.png
[pt_02]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/02_Git_install/02.png
[pt_03]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/02_Git_install/03.png
[pt_04]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/02_Git_install/04.png
[pt_05]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/02_Git_install/05.png
[pt_06]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/02_Git_install/06.png
[pt_07]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/02_Git_install/07.png
[pt_08]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/02_Git_install/08.png
[pt_09]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/02_Git_install/09.png
[pt_10]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/02_Git_install/10.png
[pt_11]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/02_Git_install/11.png
[pt_12]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/02_Git_install/12.png
[pt_13]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/02_Git_install/13.png
[pt_14]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/02_Git_install/14.jpg


{% endraw %}