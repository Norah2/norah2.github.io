---
layout: post
title: 路由重分发实验
date: 2021-08-01
description: 路由重分发实验的详解
categories: RS 
tags: RS
author: NMt
mathjax: true
---

* content
{:toc}

路由重分发实验的详解，其中包括路由引入、路由策略防环。

<div style='display: none'>
@@@@
</div>





{% raw %}
## 网络拓扑图  

![][pt_01]

需求：R5和R6能够互通。  

分析：左边运行isis协议，右边运行ospf协议，如果想要两边互通，就需要互相进行路由引入；  

路由引入打破了路由协议中的防环机制，所以路由引入很有可能会让网络拓扑形成环路。例如图中R1、R2、R3、R4就形成了一个环路（如果两边运行自己的路由协议是不会产生环路的，但是两边也不能互通；路由引入让两边可以互通，但同时产生了环路），因此需要用路由策略进行防环（除了路由策略这个方法之外还可以用路由过滤）。  

## 解法  

按照图中的网段规划设置各个路由的环回口IP、接口IP；并且按照预先设定地在相应的路由器上跑路由协议；  

完成上述的步骤之后，最主要的就是在AR2、AR3上做路由引入和路由策略：  

路由策略命名：把isis路由条目引入到ospf区域中，则把该路由策略命名为isis（左侧路由引入到右侧区域）；反之，把ospf路由条目引入到isis区域中，则命名为ospf（右侧路由引入到左侧区域）。  

路由策略防环：为什么会有环路呢？假如一开始ospf路由引入到isis中，这时候isis区域中就有从ospf学来的路由和自己本身的路由，把isis区域的路由全部引入到ospf中的时候，ospf就会学到两种来源的路由，一个是一开始isis从自己这里学到的，一个是isis区域本身的路由；而这一次ospf区域引入isis路由主要是为了学习到isis区域原生的路由，自己传过去的路由再传回来就产生了环路，所以要做路由策略来防环。  

isis区域进程号为10，所以把这个区域的路由打上10的标签；ospf区域进程号为100，所以把这个区域的路由打上100的标签。  

所以在把isis区域路由import到ospf区域的时候，第一步是把要传过去的路由先过滤一遍，如果有标签值为100的路由，说明这条路由是从ospf区域学来的，就不要再import到ospf区域中，实现方法就是先用if-match去匹配tag为100的路由，然后deny掉就实现了第一步的目的；第二步是让isis区域原生路由传入到ospf区域中，实现方法是后面再加一个permit，并且用命令`apply tag 10`来标记这些传过去的路由来源是isis 10区域的路由（标记标签值为10）。这样两步就是一边的路由策略的设置，在ospf 100中用`import-route isis 10 allow-direct route-policy isis`命令在引入路由的同时带上刚刚设置的isis路由策略；**这里在引入的时候必须带上参数allow-direct，否则直连路由不会被引入过去。**  

相对应的，ospf区域的路由也有可能会有两种来源：一种是ospf原生路由（tag 100），一种是从isis区域学习来的（tag 10）；ospf路由import到isis区域肯定是只想学习ospf原生的路由，所以需要先用if-match去匹配tag为10也就是从isis学习到的路由，然后把这样的路由deny；接下来剩下的就是ospf原生路由，permit传过去的同时把这些路由条目打上ospf的标签（`apply tag 100`）。最后在isis 10中用命令`import-route ospf 100 allow-direct route-policy ospf`完成路由的引入，同样需要带上参数allow-direct，并且在isis 10进程中需要进入ip簇视图下才可以执行这条命令，使用命令`address-family ipv4 unicast`。  

在AR2和AR3上都需要做这样的操作。  

AR2:  
```shell
Route-policy: isis
  Deny   : 10
         if-match tag 100
  Permit : 20
         apply tag 10

Route-policy: ospf
  Deny   : 10
         if-match tag 10
  Permit : 20
         apply tag 100

ospf 100 router-id 2.2.2.2
 import-route isis 10 allow-direct route-policy isis
 area 0.0.0.0
  network 2.2.2.2 0.0.0.0
  network 14.0.0.2 0.0.0.0

isis 10
 is-level level-2
 network-entity 10.0010.0000.0000.0002.00
 address-family ipv4 unicast
  import-route ospf 100 allow-direct route-policy ospf
```

AR3:  
```shell
Route-policy: isis
  Deny   : 10
         if-match tag 100
  Permit : 20
         apply tag 10

Route-policy: ospf
  Deny   : 10
         if-match tag 10
  Permit : 20
         apply tag 100

ospf 100 router-id 3.3.3.3
 import-route isis 10 allow-direct route-policy isis
 area 0.0.0.0
  network 3.3.3.3 0.0.0.0
  network 15.0.0.3 0.0.0.0

isis 10
 is-level level-2
 network-entity 10.0010.0000.0000.0003.00
 address-family ipv4 unicast
  import-route ospf 100 allow-direct route-policy ospf
```

## 验证方式  

可以用tracert命令分别在AR5、AR6上ping对方，看返回的路径；  

除此之外还有一个更为简便的方法，直接在AR1和AR4上查看路由表`display ip routing-table`，在路由表中分别查看5.5.5.5 和6.6.6.6的下一跳IP就可以看出数据流量的路径：  

![][pt_09]

![][pt_10]

AR1上查看6.6.6.6的路由表是有两条路可走，形成负载均衡；AR4上查看5.5.5.5的路由表也是有两条路可走，形成负载均衡。（AR1和AR5直连，AR4和AR6直连，所以只能在AR1上看6.6.6.6路由，在AR4上看5.5.5.5路由）  

## 升级需求与分析  

![][pt_02]

需求：AR5到AR6的数据流量从上面的路径走，AR6到AR5的数据流量从下面的路径走。  

分析：5.5.5.5这个路由条目是在isis区域中，所以肯定是在名字为isis的路由策略上进行设置，引入到ospf中；  

6.6.6.6这个路由条目是在ospf区域中，所以肯定是在名字为ospf的路由策略上进行设置，引入到isis中；  

由于从AR5到AR6走上面的路，所以AR1上关于6.6.6.6的路由选择从上面走，也就是说在AR2上调优6.6.6.6路由条目，或者在AR3上把6.6.6.6的路由条目调差；  

相对应的，由于从AR6到AR5走下面的路，所以AR4上关于5.5.5.5的路由选择从下面走，也就是说在AR3上调优5.5.5.5路由条目，或者在AR2上把5.5.5.5的路由条目调差；  

总结来说，在isis路由策略中配置关于5.5.5.5路由条目，尽量在下面调优；在ospf路由策略中配置关于6.6.6.6路由条目，尽量在上面调优。  

关于调优可操作的参数：cost值、路由类型、优先级（不知道为什么调不了，理论上可以）、直接暴力deny；  

## 设置cost值  

可以直接在接口上设置cost值，但是这种不仅影响了5.5.5.5和6.6.6.6路由条目，还影响到了其他的路由条目，影响范围太广，所以选择用路由匹配，有两种匹配方式，一种是用acl，还有一种使用ip prefix-list路由前缀；  

cost调小调不动，所以这里就把cost改大，既然cost变大，就是把路由条目调差，所以关于5.5.5.5路由条目就在AR2上的isis路由策略中改大cost值；关于6.6.6.6路由条目在AR3上的ospf路由策略中改大cost值。  

AR2:  
```shell
Prefix-list: 5
 Permitted 4
 Denied 32
        index: 10        Permit 5.5.5.5/32
Prefix-list: 6
 Permitted 1
 Denied 6
        index: 10        Permit 6.6.6.6/32

Route-policy: isis
  Deny   : 10
         if-match tag 100
  Permit: 15
         if-match ip address prefix-list 5
         apply cost + 10
         apply tag 10
  Permit : 20
         apply tag 10

Route-policy: ospf
  Deny   : 10
         if-match tag 10
  Permit : 20
         apply tag 100

ospf 100 router-id 2.2.2.2
 import-route isis 10 allow-direct route-policy isis
 area 0.0.0.0
  network 2.2.2.2 0.0.0.0
  network 14.0.0.2 0.0.0.0

isis 10
 is-level level-2
 network-entity 10.0010.0000.0000.0002.00
 address-family ipv4 unicast
  import-route ospf 100 allow-direct route-policy ospf
```

AR3:  
```shell
Prefix-list: 5
 Permitted 4
 Denied 26
        index: 10        Permit 5.5.5.5/32
Prefix-list: 6
 Permitted 4
 Denied 18
        index: 10        Permit 6.6.6.6/32

Route-policy: isis
  Deny   : 10
         if-match tag 100
  Permit : 20
         apply tag 10

Route-policy: ospf
  Deny   : 10
         if-match tag 10
  Permit: 15
         if-match ip address prefix-list 6
         apply cost + 10
         apply tag 100
  Permit : 20
         apply tag 100

ospf 100 router-id 3.3.3.3
 import-route isis 10 allow-direct route-policy isis
 area 0.0.0.0
  network 3.3.3.3 0.0.0.0
  network 15.0.0.3 0.0.0.0

isis 10
 is-level level-2
 network-entity 10.0010.0000.0000.0003.00
 address-family ipv4 unicast
  import-route ospf 100 allow-direct route-policy ospf
```

实现效果:  

![][pt_03]  

![][pt_04]  

## 设置路由类型  

可以看到5.5.5.5路由的类型是`O_ASE2`，6.6.6.6路由的类型是`IS_L2`；  

外部路由类型`O_ASE1`优先于`O_ASE2`；  

ISIS中Level2优先于Level1；  

所以，在AR2上的ospf路由策略中更改6.6.6.6路由条目的类型为`O_ASE1`；同时在AR2上的isis路由策略中更改5.5.5.5路由条目的类型为Level-1；  

AR2:  
```shell
Prefix-list: 5
 Permitted 4
 Denied 32
        index: 10        Permit 5.5.5.5/32
Prefix-list: 6
 Permitted 1
 Denied 6
        index: 10        Permit 6.6.6.6/32

Route-policy: isis
  Deny   : 10
         if-match tag 100
  Permit : 20
         apply tag 10

Route-policy: ospf
  Deny   : 10
         if-match tag 10
  Permit : 20
         apply tag 100

ospf 100 router-id 2.2.2.2
 import-route isis 10 allow-direct route-policy isis
 area 0.0.0.0
  network 2.2.2.2 0.0.0.0
  network 14.0.0.2 0.0.0.0

isis 10
 is-level level-2
 network-entity 10.0010.0000.0000.0002.00
 address-family ipv4 unicast
  import-route ospf 100 allow-direct route-policy ospf
```

AR3:  
```shell
Prefix-list: 5
 Permitted 4
 Denied 26
        index: 10        Permit 5.5.5.5/32
Prefix-list: 6
 Permitted 4
 Denied 18
        index: 10        Permit 6.6.6.6/32

Route-policy: isis
  Deny   : 10
         if-match tag 100
  Permit: 15
         if-match ip address prefix-list 5
         apply cost-type type-1
         apply tag 10
  Permit : 20
         apply tag 10

Route-policy: ospf
  Deny   : 10
         if-match tag 10
  Permit: 15
         if-match ip address prefix-list 6
         apply isis level-1
         apply tag 100
  Permit : 20
         apply tag 100

ospf 100 router-id 3.3.3.3
 import-route isis 10 allow-direct route-policy isis
 area 0.0.0.0
  network 3.3.3.3 0.0.0.0
  network 15.0.0.3 0.0.0.0

isis 10
 is-level level-2
 network-entity 10.0010.0000.0000.0003.00
 address-family ipv4 unicast
  import-route ospf 100 allow-direct route-policy ospf
```

实现效果：

![][pt_05]  

![][pt_06]

## 暴力deny  

前面的两种方式都是在精细路由上进行设置；  

这个方法直接把AR2上关于5.5.5.5路由条目deny掉，把AR3上关于6.6.6.6路由条目deny掉；这样的做法没有留备用线路，一旦上面或者下面的设备或者线路出现问题，AR5和AR6之间就断了。  

AR2:  
```shell
Prefix-list: 5
 Permitted 4
 Denied 32
        index: 10        Permit 5.5.5.5/32
Prefix-list: 6
 Permitted 1
 Denied 6
        index: 10        Permit 6.6.6.6/32

Route-policy: isis
  Deny   : 10
         if-match tag 100
  Deny: 15
         if-match ip address prefix-list 5
  Permit : 20
         apply tag 10

Route-policy: ospf
  Deny   : 10
         if-match tag 10
  Permit : 20
         apply tag 100

ospf 100 router-id 2.2.2.2
 import-route isis 10 allow-direct route-policy isis
 area 0.0.0.0
  network 2.2.2.2 0.0.0.0
  network 14.0.0.2 0.0.0.0

isis 10
 is-level level-2
 network-entity 10.0010.0000.0000.0002.00
 address-family ipv4 unicast
  import-route ospf 100 allow-direct route-policy ospf
```

AR3:  
```shell
Prefix-list: 5
 Permitted 4
 Denied 26
        index: 10        Permit 5.5.5.5/32
Prefix-list: 6
 Permitted 4
 Denied 18
        index: 10        Permit 6.6.6.6/32

Route-policy: isis
  Deny   : 10
         if-match tag 100
  Permit : 20
         apply tag 10

Route-policy: ospf
  Deny   : 10
         if-match tag 10
  Deny: 15
         if-match ip address prefix-list 6
  Permit : 20
         apply tag 100

ospf 100 router-id 3.3.3.3
 import-route isis 10 allow-direct route-policy isis
 area 0.0.0.0
  network 3.3.3.3 0.0.0.0
  network 15.0.0.3 0.0.0.0

isis 10
 is-level level-2
 network-entity 10.0010.0000.0000.0003.00
 address-family ipv4 unicast
  import-route ospf 100 allow-direct route-policy ospf
```

实现效果：  

![][pt_07]  

![][pt_08]  

## 策略路由  

使用pbr强制规定数据流量的下一跳或者出接口等信息，而不是去查相关的路由表；  

根据拓扑图和需求可以看到，只需要在AR1和AR4上强制规定数据流量的下一跳就可以实现需求；  

使用命令`policy-based-route 5 permit node 10`来配置策略路由；  

配置高级acl来匹配数据流量；  

AR1上的策略路由中用if-match匹配acl 3000中设定的数据流量后，设置这些流量的下一跳为12.0.0.2（从上面走）；  

AR4上的策略路由中用if-match匹配acl 3000中设定的数据流量后，设置这些流量的下一跳为15.0.0.3（从下面走）；  

在数据流量的入接口方向上使用命令`ip policy-based-route 5`使策略路由生效。  

AR1:  
```shell
acl advanced 3000
  rule 0 permit ip source 5.5.5.5 0 destination 6.6.6.6 0

Policy name: 5
  node 10 permit:
    if-match acl 3000
    apply next-hop 12.0.0.2
  node 20 permit:

[AR1-GigabitEthernet0/2][AR1-GigabitEthernet0/2]ip policy-based-route 5
```

AR4: 
```shell
acl advanced 3000
  rule 0 permit ip source 6.6.6.6 0 destination 5.5.5.5 0

Policy name: 5
  node 10 permit:
    if-match acl 3000
    apply next-hop 15.0.0.3
  node 20 permit:

[AR4-GigabitEthernet0/2]ip policy-based-route 5
```

查看设置的策略路由命令：`dis ip policy-based-route`;  

AR2和AR3上正常引入路由和防环：  

AR2:  
```shell
Route-policy: isis
  Deny   : 10
         if-match tag 100
  Permit : 20
         apply tag 10

Route-policy: ospf
  Deny   : 10
         if-match tag 10
  Permit : 20
         apply tag 100

ospf 100 router-id 2.2.2.2
 import-route isis 10 allow-direct route-policy isis
 area 0.0.0.0
  network 2.2.2.2 0.0.0.0
  network 14.0.0.2 0.0.0.0

isis 10
 is-level level-2
 network-entity 10.0010.0000.0000.0002.00
 address-family ipv4 unicast
  import-route ospf 100 allow-direct route-policy ospf
```

AR3:  
```shell
Route-policy: isis
  Deny   : 10
         if-match tag 100
  Permit : 20
         apply tag 10

Route-policy: ospf
  Deny   : 10
         if-match tag 10
  Permit : 20
         apply tag 100

ospf 100 router-id 3.3.3.3
 import-route isis 10 allow-direct route-policy isis
 area 0.0.0.0
  network 3.3.3.3 0.0.0.0
  network 15.0.0.3 0.0.0.0

isis 10
 is-level level-2
 network-entity 10.0010.0000.0000.0003.00
 address-family ipv4 unicast
  import-route ospf 100 allow-direct route-policy ospf
```


转载请注明：[南梦婷的博客](https://norah2.github.io) » [点击阅读原文](https://norah2.github.io/2021/08/01/RoutePolicy/) 

<!--本文用到的链接-->

[pt_01]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/59_RoutePolicy/01.png
[pt_02]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/59_RoutePolicy/02.png
[pt_03]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/59_RoutePolicy/03.png
[pt_04]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/59_RoutePolicy/04.png
[pt_05]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/59_RoutePolicy/05.png
[pt_06]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/59_RoutePolicy/06.png
[pt_07]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/59_RoutePolicy/07.png
[pt_08]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/59_RoutePolicy/08.png
[pt_09]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/59_RoutePolicy/09.png
[pt_10]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/59_RoutePolicy/10.png

{% endraw %}
