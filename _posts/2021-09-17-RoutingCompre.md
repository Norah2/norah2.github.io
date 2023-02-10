---
layout: post
title: 大规模路由综合实验  
date: 2021-09-17
description: 一个融合了关于各种路由的大规模实验，里面涉及到的知识点包括OSPF、BGP、RIP等。  
categories: RS
tags: RS BGP OSPF RIP 
author: NMt
mathjax: true
---

* content
{:toc}

最近做了一个路由综合实验，记录一下相关配置。  

<div style='display: none'>
@@@@
</div>





{% raw %}
拓扑图以及需求如下图所示（注：AR10是用作测试的，不是原本拓扑中的设备）：  

![][pt_01]

需求解释以及解决方案：  

1. 某企业总公司和分公司运行 BGP 实现路由互通，另外还有办事处运行 RIPv2。总公司和分公司之间通过两条线路相连。企业内有 A 流和 B 流两种流量，如图所示。  
   介绍这个网络拓扑的大致的架构。  
2. 按照图示配置 IP 地址，除 R7 外，所有路由配置 Loopback0 口 IP 地址用于 OSPF 的 Router-id 和 IBGP 建立邻居，地址格式为  X.X.X.X/32 ，X 为设备编号  
   给设备接口配置IP，并且除了R7之外都要配置环回口，并且用环回口建立IBGP邻居。  
3. 总公司和分公司内部配置 OSPF，仅用于实现 BGP 的 TCP 可达，不允许宣告业务网段
   在AS内部配置OSPF，172.16.x.x和192.168.x.x两个业务网段不能宣告，但是在实际构建网络的时候需要宣告。实现效果：  
   ![][pt_02]  
   ![][pt_03]  
   ![][pt_04]  
   ![][pt_05]  
   ![][pt_06]  
   ![][pt_07]  
4. 办事处和总公司之间配置 RIPv2
   实现效果：  
   ![][pt_08]  
   ![][pt_09]  
5. 适当调整链路 Cost，避免产生等价路由
   实现效果：  
   ![][pt_10]
   ![][pt_11]
   ![][pt_12]
   ![][pt_13]
   ![][pt_14]
   ![][pt_15]
6. 总公司和分公司配置 BGP 实现路由互通，总公司在 AS 65001，分公司在 AS 65002，各自 AS 内部使用对等体组建立可靠的 IBGP 全连接，AS 之间使用直连接口建立 EBGP 邻居，总公司和分公司的业务网段宣告在 BGP 中
   实现效果：
   ![][pt_16]  
   ![][pt_17]  
   ![][pt_18]  
   ![][pt_19]  
7. 为了实现总公司和分公司的流量负载均衡，要求通过修改 AS_path，使 A 流数据经过 R2 和 R4，B 流数据经过 R3 和 R5   
   实现效果：
   ![][pt_20]  
   ![][pt_21]  
8. 在 R2 上配置 RIP 和 BGP 的双向引入，要求办事处的 A 流和 B 流都能与总公司互通，但办事处与分公司之间只有 A 流能够互通  
   实现效果：  
   ![][pt_22]
   ![][pt_23]
   ![][pt_24]
   ![][pt_25]
9. 不允许业务网段出现协议报文，不允许出现不相关的 RIP 协议报文
10. 随着公司业务发展，后续可能会有其他分公司通过 R2 或 R3 接入总公司；不允许分公司之间互访，所以要求总公司只能对分公司发布属于本 AS 的路由  
   实现效果：
   ![][pt_26]  
   ![][pt_27]  
   ![][pt_28]  


相关配置：  

```shell
AR1：
#
 sysname R1
#
interface GigabitEthernet0/0/0
 ip address 10.0.0.1 255.255.255.252 
 ospf cost 10
#
interface GigabitEthernet0/0/1
 ip address 10.0.0.5 255.255.255.252 
 ospf cost 5
#
interface LoopBack0
 ip address 1.1.1.1 255.255.255.255 
#
interface LoopBack1
 ip address 192.168.0.1 255.255.255.0 
#
interface LoopBack2
 ip address 172.16.0.1 255.255.255.0 
#
bgp 65001
 router-id 1.1.1.1
 group neib internal
 peer neib connect-interface LoopBack0
 peer 2.2.2.2 group neib 
 peer 3.3.3.3 group neib 
 network 172.16.0.0 255.255.255.0 
 network 192.168.0.0 
#
ospf 1 router-id 1.1.1.1 
 area 0.0.0.0 
  network 1.1.1.1 0.0.0.0 
  network 10.0.0.1 0.0.0.0 
  network 10.0.0.5 0.0.0.0 
#
return

AR2：
#
 sysname R2
#
acl number 2000  
 rule 5 deny source 172.16.1.0 0.0.0.255 
 rule 10 permit 
acl number 2001  
 rule 5 deny source 172.16.2.0 0.0.0.255 
 rule 10 permit 
#
interface GigabitEthernet0/0/0
 ip address 10.0.0.2 255.255.255.252 
 ospf cost 10
#
interface GigabitEthernet0/0/1
 ip address 10.0.0.9 255.255.255.252 
#
interface GigabitEthernet0/0/2
 ip address 10.0.0.13 255.255.255.252 
#
interface GigabitEthernet1/0/0
 ip address 10.0.0.33 255.255.255.252 
#
interface LoopBack0
 ip address 2.2.2.2 255.255.255.255 
#
bgp 65001
 router-id 2.2.2.2
 peer 10.0.0.14 as-number 65002 
 group neib internal
 peer neib connect-interface LoopBack0
 peer 1.1.1.1 group neib 
 peer 3.3.3.3 group neib 
 import-route rip 1
 peer 10.0.0.14 filter-policy 2001 export
 peer 10.0.0.14 as-path-filter 1 export 
 peer 10.0.0.14 route-policy as_path import
 peer neib next-hop-local 
#
ospf 1 router-id 2.2.2.2 
 area 0.0.0.0 
  network 2.2.2.2 0.0.0.0 
  network 10.0.0.2 0.0.0.0 
  network 10.0.0.9 0.0.0.0 
#
rip 1
 undo summary
 version 2
 network 10.0.0.0
 silent-interface GigabitEthernet0/0/0
 silent-interface GigabitEthernet0/0/1
 silent-interface GigabitEthernet0/0/2
 filter-policy 2000 export bgp
 import-route bgp permit-ibgp
#
route-policy as_path permit node 10 
 if-match ip-prefix as_path 
 apply as-path 100 additive
#
route-policy as_path permit node 20 
#
ip ip-prefix as_path index 10 permit 172.16.1.0 24
#
ip as-path-filter 1 permit ^$
#
return

AR3：
#
 sysname R3
#
acl number 2001  
 rule 5 deny source 172.16.2.0 0.0.0.255 
 rule 10 permit 
#
interface GigabitEthernet0/0/0
 ip address 10.0.0.6 255.255.255.252 
 ospf cost 5
#
interface GigabitEthernet0/0/1
 ip address 10.0.0.10 255.255.255.252 
#
interface GigabitEthernet0/0/2
 ip address 10.0.0.17 255.255.255.252 
#
interface LoopBack0
 ip address 3.3.3.3 255.255.255.255 
#
bgp 65001
 router-id 3.3.3.3
 peer 10.0.0.18 as-number 65002 
 group neib internal
 peer neib connect-interface LoopBack0
 peer 1.1.1.1 group neib 
 peer 2.2.2.2 group neib 
 peer 10.0.0.18 filter-policy 2001 export
 peer 10.0.0.18 as-path-filter 1 export 
 peer 10.0.0.18 route-policy as_path import
 peer neib next-hop-local
#
ospf 1 router-id 3.3.3.3 
 area 0.0.0.0 
  network 3.3.3.3 0.0.0.0 
  network 10.0.0.6 0.0.0.0 
  network 10.0.0.10 0.0.0.0 
#
route-policy as_path permit node 10 
 if-match ip-prefix as_path 
 apply as-path 100 additive
#
route-policy as_path permit node 20 
#
ip ip-prefix as_path index 10 permit 192.168.1.0 24
#
ip as-path-filter 1 permit ^$
#
return


AR4：
#
 sysname R4
#
interface GigabitEthernet0/0/0
 ip address 10.0.0.14 255.255.255.252 
#
interface GigabitEthernet0/0/1
 ip address 10.0.0.21 255.255.255.252 
#
interface GigabitEthernet0/0/2
 ip address 10.0.0.25 255.255.255.252 
 ospf cost 5
#
interface LoopBack0
 ip address 4.4.4.4 255.255.255.255 
#
bgp 65002
 router-id 4.4.4.4
 peer 10.0.0.13 as-number 65001 
 group neib internal
 peer neib connect-interface LoopBack0
 peer 5.5.5.5 group neib 
 peer 6.6.6.6 group neib 
 peer 10.0.0.13 route-policy as_path import
 peer neib next-hop-local 
#
ospf 1 router-id 4.4.4.4 
 area 0.0.0.0 
  network 4.4.4.4 0.0.0.0 
  network 10.0.0.21 0.0.0.0 
  network 10.0.0.25 0.0.0.0 
#
route-policy as_path permit node 10 
 if-match ip-prefix as_path 
 apply as-path 100 additive
#
route-policy as_path permit node 20 
#
ip ip-prefix as_path index 10 permit 172.16.0.0 24
#
return

AR5：
#
 sysname R5
#
interface GigabitEthernet0/0/0
 ip address 10.0.0.18 255.255.255.252 
#
interface GigabitEthernet0/0/1
 ip address 10.0.0.22 255.255.255.252 
#
interface GigabitEthernet0/0/2
 ip address 10.0.0.29 255.255.255.252 
 ospf cost 10
#
interface LoopBack0
 ip address 5.5.5.5 255.255.255.255 
#
bgp 65002
 router-id 5.5.5.5
 peer 10.0.0.17 as-number 65001 
 group neib internal
 peer neib connect-interface LoopBack0
 peer 4.4.4.4 group neib 
 peer 6.6.6.6 group neib 
 peer 10.0.0.17 route-policy as_path import
 peer neib next-hop-local 
#
ospf 1 router-id 5.5.5.5 
 area 0.0.0.0 
  network 5.5.5.5 0.0.0.0 
  network 10.0.0.22 0.0.0.0 
  network 10.0.0.29 0.0.0.0 
#
route-policy as_path permit node 10 
 if-match ip-prefix as_path 
 apply as-path 100 additive
#
route-policy as_path permit node 20 
#
ip ip-prefix as_path index 10 permit 192.168.0.0 24
#
return


AR6：
#
 sysname R6
#
interface GigabitEthernet0/0/0
 ip address 10.0.0.26 255.255.255.252 
 ospf cost 5
#
interface GigabitEthernet0/0/1
 ip address 10.0.0.30 255.255.255.252 
 ospf cost 10
#
interface LoopBack0
 ip address 6.6.6.6 255.255.255.255 
#
interface LoopBack1
 ip address 192.168.1.1 255.255.255.0 
#
interface LoopBack2
 ip address 172.16.1.1 255.255.255.0 
#
bgp 65002
 router-id 6.6.6.6
 group neib internal
 peer neib connect-interface LoopBack0
 peer 4.4.4.4 group neib 
 peer 5.5.5.5 group neib
 network 172.16.1.0 255.255.255.0 
 network 192.168.1.0 
#
ospf 1 router-id 6.6.6.6 
 area 0.0.0.0 
  network 6.6.6.6 0.0.0.0 
  network 10.0.0.26 0.0.0.0 
  network 10.0.0.30 0.0.0.0 
#
return


AR7：
#
 sysname R7
#
interface GigabitEthernet0/0/0
 ip address 10.0.0.34 255.255.255.252 
#
interface LoopBack0
 ip address 192.168.2.1 255.255.255.0 
#
interface LoopBack1
 ip address 172.16.2.1 255.255.255.0 
#
rip 1
 undo summary
 version 2
 network 10.0.0.0
 network 192.168.2.0
 network 172.16.0.0
#
return
```

转载请注明：[南梦婷的博客](https://norah2.github.io) » [点击阅读原文](https://norah2.github.io/2021/09/17/RoutingCompre/) 

<!--本文用到的链接-->


[pt_07]:https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/62_RoutingCompre/07.png
[pt_08]:https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/62_RoutingCompre/08.png
[pt_09]:https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/62_RoutingCompre/09.png
[pt_10]:https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/62_RoutingCompre/10.png
[pt_11]:https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/62_RoutingCompre/11.png
[pt_12]:https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/62_RoutingCompre/12.png
[pt_13]:https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/62_RoutingCompre/13.png
[pt_14]:https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/62_RoutingCompre/14.png
[pt_15]:https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/62_RoutingCompre/15.png
[pt_16]:https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/62_RoutingCompre/16.png
[pt_17]:https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/62_RoutingCompre/17.png
[pt_18]:https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/62_RoutingCompre/18.png
[pt_19]:https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/62_RoutingCompre/19.png
[pt_20]:https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/62_RoutingCompre/20.png
[pt_21]:https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/62_RoutingCompre/21.png
[pt_22]:https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/62_RoutingCompre/22.png
[pt_23]:https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/62_RoutingCompre/23.png
[pt_24]:https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/62_RoutingCompre/24.png
[pt_25]:https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/62_RoutingCompre/25.png
[pt_26]:https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/62_RoutingCompre/26.png
[pt_27]:https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/62_RoutingCompre/27.png
[pt_28]:https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/62_RoutingCompre/28.png
[pt_01]:https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/62_RoutingCompre/01.png
[pt_02]:https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/62_RoutingCompre/02.png
[pt_03]:https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/62_RoutingCompre/03.png
[pt_04]:https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/62_RoutingCompre/04.png
[pt_05]:https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/62_RoutingCompre/05.png
[pt_06]:https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/62_RoutingCompre/06.png


{% endraw %}
