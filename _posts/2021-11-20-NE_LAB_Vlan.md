---
layout: post
title: NE-LAB-Vlan实验
date: 2021-11-20
description: 在HCL模拟器上做Vlan二层广播域隔离的实验。
categories: NE-LAB
tags: RS
author: NMt
mathjax: true
---

* content
{:toc}

在HCL模拟器上做Vlan二层广播域隔离的实验。

<div style='display: none'>
@@@@
</div>





{% raw %}
# 网络拓扑：  

![][pt_01]  


# 实验需求：  

1.配置IP地址；  
2.SW1和SW2上分别创建vlan10和vlan20，要求PC3和PC5属于vlan10，PC4和PV6属于vlan20；  
3.SW1和SW2相连的接口配置为trunk类型，允许vlan10和vlan20通过；  
4.测试效果，同一vlan的PC可以互通，不同vlan的PC无法互通。  


# 实验步骤：

1. 为PC配置相关的IP地址；  

2. 在两个交换机上创建相关的vlan并把对应的端口加入：  

	```shell
	[SW1]vlan 10
	[SW1-vlan10]port g1/0/2
	[SW1-vlan10]vlan 20
	[SW1-vlan20]port g1/0/3

	[SW2]vlan 10
	[SW2-vlan10]port g1/0/2
	[SW2-vlan10]vlan 20
	[SW2-vlan20]port g1/0/3
	```

3. 配置两个交换机之间的端口类型为trunk，并允许vlan10、vlan20的数据流量通过：  

	```shell
	[SW1-vlan20]int g1/0/1
	[SW1-GigabitEthernet1/0/1]port link-type trunk
	[SW1-GigabitEthernet1/0/1]port trunk permit vlan 10 20

	[SW2-vlan20]int g1/0/1
	[SW2-GigabitEthernet1/0/1]port link-type trunk
	[SW2-GigabitEthernet1/0/1]port trunk permit vlan 10 20
	```

4. 测试功能：  

	![][pt_02]  

	![][pt_03]  


转载请注明：[南梦婷的博客](https://norah2.github.io) » [点击阅读原文](https://norah2.github.io/2021/11/20/NE_LAB_Vlan/) 

<!--本文用到的链接-->

[pt_01]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/67_NE_LAB_Vlan/01.png
[pt_02]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/67_NE_LAB_Vlan/02.png
[pt_03]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/67_NE_LAB_Vlan/03.png

{% endraw %}
