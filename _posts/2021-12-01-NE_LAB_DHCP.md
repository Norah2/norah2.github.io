---
layout: post
title: NE-LAB-DHCP实验
date: 2021-12-01
description: 在HCL模拟器上做DHCP协议的实验。
categories: NE-LAB
tags: RS
author: NMt
mathjax: true
---

* content
{:toc}

在HCL模拟器上做DHCP协议的实验。

<div style='display: none'>
@@@@
</div>





{% raw %}

# 网络拓扑：  

![][pt_01]  

# 实验需求：  

1. 按照图示为R1配置IP地址；  
2. 配置R1为DHCP服务器，提供服务的地址池为192.168.1.0/24网段，网关为192.168.1.1，DNS服务器地址为202.103.24.68，202.103.0.117；  
3. 192.168.1.10-192.168.1.20为专用地址段，要求不能用于自动分配；  
4. PC3和PC4都能获取到192.168.1.0/24网段的IP地址；  
5. 用路由器模拟PC，使得该设备也能获取到IP地址和相应的网关；  

# 实验步骤：  

1. 配置IP地址过程略；  

2. 配置R1为DHCP服务器，首先需要在全局设备上开启HDCP服务功能；接着需要开启一个dhcp的ip地址池，在地址池的视图下需要设置：  

	①可以用来分配的网段；  
	②网关地址列表；  
	③dns地址列表；  
	④禁止被分配的地址（可选）：  

	```shell
	dhcp server ip-pool 1
	 gateway-list 192.168.1.1
	 network 192.168.1.0 mask 255.255.255.0
	 dns-list 202.103.24.68 202.103.0.117
	```

3. 查看效果：

   ![][pt_02]  
   
   ![][pt_03]  
   
4. 在dhcp的ip地址池中禁止分配专用的IP地址：  

	```shell
	[R1-dhcp-pool-1]forbidden-ip 192.168.1.10 192.168.1.20
	```

5. 将两个PC上的接口关闭然后重启，重新获取ip地址，查看效果：
   
   ![][pt_04]  
   
   ![][pt_05]  

6. 在R2上的接口配置为dhcp获取IP地址和网关：  

	```shell
	[R2]int g0/0
	[R2-GigabitEthernet0/0]ip add dhcp-alloc
	``` 

7. 验证R2是否获取到IP地址以及网关：
    
    ![][pt_06]  
    
    ![][pt_07]  


转载请注明：[南梦婷的博客](https://norah2.github.io) » [点击阅读原文](https://norah2.github.io/2021/12/01/NE_LAB_DHCP/) 

<!--本文用到的链接-->

[pt_02]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/69_NE_Lab_DHCP/02.png
[pt_03]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/69_NE_Lab_DHCP/03.png
[pt_04]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/69_NE_Lab_DHCP/04.png
[pt_05]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/69_NE_Lab_DHCP/05.png
[pt_06]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/69_NE_Lab_DHCP/06.png
[pt_07]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/69_NE_Lab_DHCP/07.png
[pt_01]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/69_NE_Lab_DHCP/01.png

{% endraw %}
