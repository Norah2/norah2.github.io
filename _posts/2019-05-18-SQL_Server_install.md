---
layout: post
title: SQL Server 安装教程  
date: 2019-05-18
description: sql server 的安装教程
categories: Install 
tags: Install SQLServer DataBase
author: NMt
mathjax: true
---

* content
{:toc}

这两天安装了个数据库，就顺便写个教程，数据库是SQL server。安装环境是Windows server 10。    

<div style='display: none'>
@@@@
</div>




{% raw %}
如果需要安装sql server全部功能则需要先安装jdk，jdk的安装教程请移步我的另一篇文章：[jdk安装教程](https://norah2.github.io/2019/05/17/jdk_install/)  

在安装jdk的基础之上开始安装我们的数据库~  

1. 先将iso后缀的文件进行解压，打开解压后的文件夹点击setup：  
    ![][pt_01]  
2. 点击左边的安装选项卡，再点击“全新SQL Server独立安装或向现有安装添加功能”   
    ![][pt_02]  
3. 填写产品密钥，点击下一步：  
    ![][pt_03]  
4. 点击接受许可条款，点击下一步：
    ![][pt_04]  
5. 点击下一步：
    ![][pt_05]  
6. 点击下一步：
    ![][pt_06]  
7. 点击下一步：  
    ![][pt_07]  
8. 等待，点击下一步:  
    ![][pt_08]  
9. 点击下一步：  
    ![][pt_09]  
10. 点击全选，安装路径可以改成自己喜欢的，我直接改到了D盘，改好点击下一步：  
    ![][pt_10]  
	
    但是我这一步出现了报错，PolyBase的检查不通过，点击查看原因如下： 
	![][pt_11]  
	
    百度之后说是需要安装JRE 7，只能是7，装8也不行，下载地址：[http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html)  
	![][pt_12]  
	![][pt_13]  
	我安装之后还是不行，可能是没装上吧，于是我就暴力解决，直接不安装这个功能，在上一步中取消勾选“针对外部数据的PolyBase查询服务”：  
	![][pt_14]  
11. 点击下一步：  
    ![][pt_15]  	
12. 点击下一步：
    ![][pt_16]  	
13. 点击“添加当前用户”，再点击下一步：  
    ![][pt_17]   	
14. 点击“添加当前用户”，再点击下一步：  
    ![][pt_18]  	
15. 勾选第一个和第三个：  
    ![][pt_19]  	
16. 点击“添加当前用户”，再点击下一步：  
    ![][pt_20]  	
17. 更改工作目录进和结果目录，可选择自己喜欢的文件夹。  
    ![][pt_21]  	
18. 点击“接受”，下一步:  
    ![][pt_22]  	
19. 出现以下界面，等待即可：  
    ![][pt_23]  	
20. 安装完成之后点击确定：  
    ![][pt_24]  	
21. 从开始界面点击“Reporting Services配置管理器”  
    ![][pt_25]  	
22. 点击连接：  
    ![][pt_26]  	
23. 这里的状态应该是启动状态，如果不是，则手动点击启动：  
    ![][pt_27]  	
24. 在开始里找到“SQL Server 2016 Data Quality Server”, 点击：  
    ![][pt_28]  	
25. 出现“是否要继续？[是/否]”，打一个“是”即可：  
    ![][pt_29]  	
26. 接下来它会让你输出密码，密码长度至少8个字符，大小写字母、特殊字符、数字至少出现3种。确认一遍密码即可：  
    ![][pt_30]  	
26. 出现点击任意键继续，就随意按一下键盘就好了。  
    ![][pt_31]  	
27. 从开始点击“SQL Server 2016 Data Quality Cli...”  
    ![][pt_33]  	
28. 点击下拉箭头，选择“LOCAL”, 点击连接:  
	![][pt_35]  
    ![][pt_32]  
29. 出现下述界面就是安装成功了：  
    ![][pt_34]   


转载请注明：[南梦婷的博客](https://norah2.github.io) » [点击阅读原文](https://norah2.github.io/2019/05/18/SQL_Server_install/)   

<!--以下是本文用到的链接-->  

[pt_01]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/20_SQL_Server_install/01.png
[pt_02]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/20_SQL_Server_install/02.png
[pt_03]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/20_SQL_Server_install/03.png
[pt_04]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/20_SQL_Server_install/04.png
[pt_05]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/20_SQL_Server_install/05.png
[pt_06]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/20_SQL_Server_install/06.png
[pt_07]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/20_SQL_Server_install/07.png
[pt_08]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/20_SQL_Server_install/08.png
[pt_09]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/20_SQL_Server_install/09.png
[pt_10]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/20_SQL_Server_install/10.png
[pt_11]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/20_SQL_Server_install/11.png
[pt_12]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/20_SQL_Server_install/12.png
[pt_13]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/20_SQL_Server_install/13.png
[pt_14]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/20_SQL_Server_install/14.png
[pt_15]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/20_SQL_Server_install/15.png
[pt_16]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/20_SQL_Server_install/16.png
[pt_17]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/20_SQL_Server_install/17.png
[pt_18]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/20_SQL_Server_install/18.png
[pt_19]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/20_SQL_Server_install/19.png
[pt_20]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/20_SQL_Server_install/20.png
[pt_21]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/20_SQL_Server_install/21.png
[pt_22]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/20_SQL_Server_install/22.png
[pt_23]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/20_SQL_Server_install/23.png
[pt_24]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/20_SQL_Server_install/24.png
[pt_25]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/20_SQL_Server_install/25.png
[pt_26]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/20_SQL_Server_install/26.png
[pt_27]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/20_SQL_Server_install/27.png
[pt_28]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/20_SQL_Server_install/28.png
[pt_29]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/20_SQL_Server_install/29.png
[pt_30]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/20_SQL_Server_install/30.png
[pt_31]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/20_SQL_Server_install/31.png
[pt_32]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/20_SQL_Server_install/32.png
[pt_33]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/20_SQL_Server_install/33.png
[pt_34]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/20_SQL_Server_install/34.png
[pt_35]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/20_SQL_Server_install/35.png

{% endraw %}
