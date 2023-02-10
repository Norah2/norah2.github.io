---
layout: post
title: IDEA安装教程  
date: 2019-08-05
description: IDEA安装教程以及简单设置内核。  
categories: Install
tags: Install Python IDEA 
author: NMt
mathjax: true
---

* content
{:toc}

IDEA安装教程以及简单设置内核。  

<div style='display: none'>
@@@@
</div>




{% raw %}
在安装IDEA之前需安装jdk，安装教程可以参照[jdk安装教程][link_01]  
  
1. 右击以管理员身份运行并点击next。  
  
   ![][pt_01]) 

2. 选择安装路径，点击next。  
  
   ![][pt_02])  

3. 如图勾选，点击next。  
  
   ![][pt_03])  

4. 给新文件夹起一个名字，我起的是JetBrains，也可起其他名字，点击install进行安装。  
  
   ![][pt_04])  

5. 等待安装完成之后右击点击“打开文件所在的位置”。  
  
   ![][pt_05])  

6. 找到“idea.exe.vmoptions”，右击选择打开方式，选择记事本打开。  
  
   ![][pt_06])  

7. 将以下代码复制到文件当中。  
  
   ```python
-server
-Xms128m
-Xmx512m
-XX:ReservedCodeCacheSize=240m
-XX:+UseConcMarkSweepGC
-XX:SoftRefLRUPolicyMSPerMB=50
-ea
-Dsun.io.useCanonCaches=false
-Djava.net.preferIPv4Stack=true
-Djdk.http.auth.tunneling.disabledSchemes=""
-XX:+HeapDumpOnOutOfMemoryError
-XX:-OmitStackTraceInFastThrow
-javaagent:F:\IDEA\bin\JetbrainsCrack.jar
   ```
  
   ![][pt_07])  
  
   **注：红圈中的路径改为自己的安装路径。**  
  
8. 找到“idea64.exe.vmoptions”，右击选择打开方式，选择记事本打开。  
  
   ![][pt_08])  
  
9. 将以下代码复制到文件中：  
  
   ```
-Xms128m
-Xmx750m
-XX:ReservedCodeCacheSize=240m
-XX:+UseConcMarkSweepGC
-XX:SoftRefLRUPolicyMSPerMB=50
-ea
-Dsun.io.useCanonCaches=false
-Djava.net.preferIPv4Stack=true
-Djdk.http.auth.tunneling.disabledSchemes=""
-XX:+HeapDumpOnOutOfMemoryError
-XX:-OmitStackTraceInFastThrow
-javaagent:F:\IDEA\bin\JetbrainsCrack.jar
   ```
  
   ![][pt_09])  
  
   **注：红圈中的路径改为自己的安装路径。**  
  
10. 双击图标，点击OK。  
   ![][pt_10])  
   
11. 点击“skip Remaining and Sets Defaults”。  
   ![][pt_11])  
   
12. 选择Activation code，将代码复制进去，点击OK。  
   ![][pt_12])  
   
13. 点击“Create New Project”。  
   ![][pt_13])  
   
14. 按图所示，点击Java，在New中选择一开始安装的jre1.8，点击next。  
   ![][pt_14])  
   
15. 点击next。  
   ![][pt_15])  
  
16. 第一行是项目名称，第二行是项目路径，可自行更改，点击Finish。  
   ![][pt_16])  
  
17. 点击“Show tips on startup”，再点击Close即可。  
   ![][pt_17])  
  
18. 将文件resources_cn_IntelliJIDEA_2018.1_r2复制到安装路径下的lib文件夹中。  
   ![][pt_18]  
  
转载请注明：[南梦婷的博客](https://norah2.github.io) » [点击阅读原文](https://norah2.github.io/2019/08/05/IDEA_install/)   

<!--以下是本文用到的链接-->  

[pt_01]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/26_IDEA_install/01.png
[pt_02]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/26_IDEA_install/02.png
[pt_03]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/26_IDEA_install/03.png
[pt_04]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/26_IDEA_install/04.png
[pt_05]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/26_IDEA_install/05.png
[pt_06]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/26_IDEA_install/06.png
[pt_07]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/26_IDEA_install/07.png
[pt_08]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/26_IDEA_install/08.png
[pt_09]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/26_IDEA_install/09.png
[pt_10]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/26_IDEA_install/10.png
[pt_11]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/26_IDEA_install/11.png
[pt_12]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/26_IDEA_install/12.png
[pt_13]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/26_IDEA_install/13.png
[pt_14]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/26_IDEA_install/14.png
[pt_15]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/26_IDEA_install/15.png
[pt_16]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/26_IDEA_install/16.png
[pt_17]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/26_IDEA_install/17.png
[pt_18]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/26_IDEA_install/18.png

[link_01]: https://norah2.github.io/2019/05/17/jdk_install/

{% endraw %}
