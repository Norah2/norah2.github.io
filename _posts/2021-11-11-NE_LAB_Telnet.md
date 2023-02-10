---
layout: post
title: NE-LAB-Telnet实验
date: 2021-11-11
description: 在HCL模拟器上做telnet远程登录管理的实验。
categories: NE-LAB
tags: RS
author: NMt
mathjax: true
---

* content
{:toc}

在HCL模拟器上做telnet远程登录管理的实验。

<div style='display: none'>
@@@@
</div>





{% raw %}

# 网络拓扑：

![][pt_01] 


# 实验需求：

1. 按照图示连接到真机，并配置IP地址（真机IP地址配置到VirtualBox Host-Only Ethernet Adapter网卡）；  
2. 在R1上开启Telnet服务，创建用户nan，密码123456用于身份验证；  
3. 在真机上使用S-CRT通过Telnet登录R1。  


# 实验步骤：

1. 配置IP地址，连接真机：

	```shell
	 int g0/0
	 ip add 192.168.56.2 24
	```

2. 在R1上开启Telnet服务：

	```shell
	[AR1]telnet server enable
	```

3. R1上创建用户“nan”，密码“123456”用于Telnet身份验证:  

	```shell
	[AR1]local-user nan class manage 
	New local user added.
	[AR1-luser-manage-nan]password simple 123456
	[AR1-luser-manage-nan]service-type telnet 
	[AR1-luser-manage-nan]authorization-attribute user-role level-15
	```

4. 在vty终端视图下配置Telnet验证模式为AAA验证，权限为Level-15

	```shell
	[AR1]user-interface vty 0 4
	[AR1-line-vty0-4]authentication-mode scheme 
	[AR1-line-vty0-4]user-role level-15
	```

5. 真机上打开CRT，连接AR1：

	![][pt_02] 

	![][pt_03] 


转载请注明：[南梦婷的博客](https://norah2.github.io) » [点击阅读原文](https://norah2.github.io/2021/11/11/NE_LAB_Telnet/) 

<!--本文用到的链接-->

[pt_01]:https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/66_NE_LAB_Telnet/01.png
[pt_02]:https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/66_NE_LAB_Telnet/02.png
[pt_03]:https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/66_NE_LAB_Telnet/03.png


{% endraw %}
