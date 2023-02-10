---
layout: post
title: 在IDEA中配置python环境  
date: 2019-08-28
description: 在IDEA中配置python环境  
categories: Windows
tags: Python IDEA Windows
author: NMt
mathjax: true
---

* content
{:toc}

在IDEA中配置python环境  

<div style='display: none'>
@@@@
</div>





{% raw %}
最近在帮老师写一个大数据案例分析，想着项目还是用IDEA比较好，所以重新打开IDEA；大家知道的我电脑之前抽过一次风，上次安装软件的时候仅仅只是安装了一下，并没有完全配置好，所以现在只能重新开始配置python环境了。  

下面就是在IDEA中配置python环境的详细步骤：  

1. 在"file"中找到"setting"点击：  

   ![][pt_01]  

2. 单击"Plugins"，再单击"Install JetBrains plugin..."：

   ![][pt_02]  

3. 在搜索框中输入"Python", 找到python插件再点击"Install": 

   ![][pt_03]  

4. 等待安装完成之后会出现下述对话框，点击"重启":  

   ![][pt_04]  

5. 重启之后在整个界面右上角找到图中标记的位置，该位置是"项目结构"的快捷方式，单击：  

   ![][pt_05]  

6. 单击后会出现下述对话框，在左侧找到"Project"，单击之后点击"新建"，在下拉框中找到Python单击：  

   ![][pt_06]  

7. 在新弹出的对话框中的左侧找到"Virtualenv Environment"单击，右侧点击"Existing environment"，在"Interpreter"后面点击"..."（浏览），在新弹出的对话框中找到"python.exe"并选择，后面就一路点确认：  

   ![][pt_07]  
  
   ![][pt_08]  

8. 返回到最开始的对话框后在左侧点击"SDKs"，这时候会发现除了Java的内核外还多了一个python的内核，选择并点击确定：  

   ![][pt_09]  

9. 重新打开"files"选项卡，在移动到"New"处，可以发现多了一个"Python File"的选项，点击：  

   ![][pt_10]  

10. 现在就可以在已经存在的项目中新建一个py文件，并且将项目改成python环境。  

   ![][pt_11]  

**注：我是在已经存在的一个项目中，更改项目的运行环境。也可以先安装python插件，加载python的内核，然后新建一个项目，在新建项目的时候选择python即可，关键步骤都是一样的。**

---

信心满满的我，忙活了一晚上就配置好了一个环境。后面需要抓紧时间了！！！

转载请注明：[南梦婷的博客](https://norah2.github.io) » [点击阅读原文](https://norah2.github.io/2019/08/28/SymPy_Introduce/)   

<!--以下是本文用到的链接-->  

[pt_01]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/32_idea_python/01.png
[pt_02]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/32_idea_python/02.png
[pt_03]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/32_idea_python/03.png
[pt_04]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/32_idea_python/04.png
[pt_05]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/32_idea_python/05.png
[pt_06]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/32_idea_python/06.png
[pt_07]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/32_idea_python/07.png
[pt_08]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/32_idea_python/08.png
[pt_09]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/32_idea_python/09.png
[pt_10]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/32_idea_python/10.png
[pt_11]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/32_idea_python/11.png

{% endraw %}
