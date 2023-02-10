---
layout: post
title: NE-LAB-DHCP中继实验
date: 2021-12-04
description: 在HCL模拟器上做DHCP中继协议的实验。
categories: NE-LAB
tags: RS
author: NMt
mathjax: true
---

* content
{:toc}

在HCL模拟器上做DHCP中继协议的实验。

<div style='display: none'>
@@@@
</div>





{% raw %}

# 网络拓扑：

![][pt_01]

# 实验需求：

1. 按照图示配置IP地址；  
2. 配置R1为DHCP服务器，能够跨网段为192.168.2.0/24网段自动分配IP地址。要求分配DNS地址为202.103.24.68和202.103.0.117；  
3. PC3获取IP地址后，能够访问到192.168.1.1。  

# 实验步骤：

1. 配置作为DHCP服务器和DHCP中继服务器的路由器相关的IP地址；  

2. 将R1配置为DHCP服务器：  

	```shell
	[R1]dhcp enable
	[R1]dhcp server ip-pool 1
	[R1-dhcp-pool-1]network 192.168.2.0 mask 255.255.255.0
	[R1-dhcp-pool-1]gateway-list 192.168.2.1
	[R1-dhcp-pool-1]dns-list 202.103.24.68 202.103.0.117
	```

3. 在R2上开启dhcp服务，同时在连接client的接口上开启dhcp中继服务：  

	```shell
	[R2-GigabitEthernet0/1]dhcp select relay
	[R2-GigabitEthernet0/1]dhcp relay server-address 192.168.1.1
	```

4. 在R1上配置一个静态路由，让dhcp协议报文能够到达R2上：

	```shell
	[R1]ip rou 0.0.0.0 0 192.168.1.2
	```

5. 查看PC自动获取的IP地址：

   ![][pt_02]  

	在PC上测试是否能与192.168.1.1 连通：

   ![][pt_03]



转载请注明：[南梦婷的博客](https://norah2.github.io) » [点击阅读原文](https://norah2.github.io/2021/12/04/NE_LAB_DHCPRelay/) 

<!--本文用到的链接-->

[pt_01]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/70_NE_Lab_DHCPRelay/01.png
[pt_02]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/70_NE_Lab_DHCPRelay/02.png
[pt_03]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/70_NE_Lab_DHCPRelay/03.png

{% endraw %}
