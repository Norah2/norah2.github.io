---
layout: post
title: Windows下安装selenium以及chrome driver的安装步骤  
date: 2019-05-06
description: 本文主要写关于selenium的前期安装工作
categories: Install
tags: Install Crawler ChromeDriver 
author: NMt
mathjax: true
---

* content
{:toc}

今天介绍一下关于selenium的安装方法：  

<div style='display: none'>
@@@@
</div>




{% raw %}
### 确定chrome版本  

首先需要知道chrome的版本号，不同的版本对应不同版本的driver，如果版本号没有对应上会报错。  

1. 打开chrome，找到帮助选项卡，下拉框中有一个关于chrome的选项。  

   ![][pt_01]
  
2. 打开后就可以看见自己电脑上的chrome版本号，我的chrome版本号是74.0.3729.131  

   ![][pt_02]
  
3. [打开链接][link_chromedriver], 在其中找到和自己chrome对应版本号的chromedriver。我选择的是74.0.3729.6/，点击。    

   ![][pt_03]  
  
   **注：一般一个版本的driver能够对应相近几个版本的chrome，所以选择邻近版本的chrome一般也可以，如果你没有在链接里面找到自己chrome的版本号可以自己查一下能够被哪个版本的driver兼容**  
  

4. 进入到这个下载界面，我的系统是Windows，所以选了第三个，你可以根据自己的系统进行选择。  

   ![][pt_04] 

   **注：Windows系统好像只有32的driver，反正我用着没有出bug。（我电脑是64位的）**  
  

5. 下载之后解压  

    ![][pt_05]  

6. 将解压之后的chromedriver.exe复制到路径`C:\Program Files (x86)\Google\Chrome\Application`下（我的需要管理员权限）。

   ![][pt_06]  

### 安装selenium第三方包  

打开cmd，输入`pip install selenium`或`pip3 install selenium`  

### 测试代码  

**重启你的python编辑器(spyder or pychram or IDLE ect.)**  

一定要记得重启一下！！！  

打开编辑器之后，输入以下代码：  

```python
from selenium import webdriver
driver = webdriver.Chrome()
```

如果出现以下界面即使成功：

![][pt_07]  

---
2019-08-06  

由于被重装系统，因此需要重新安装一遍selenium，我发现按照上述过程会出现以下报错：  

![][pt_08]  

上述报错的原因是由于chromedriver所在路径没有被放入环境变量中，有两种方法可以解决：

1. 将路径放入环境变量中，步骤如下：   

   1. 此电脑右击选择属性，再点击高级系统设置。  
      ![][pt_09]  
	  
   2. 点击环境变量。  
	  ![][pt_10]  
	  
   3. 找到path双击。  
	  ![][pt_11]  
	  
   4. 在末尾空白的行上双击进入到编辑模式，再将chromedriver的路径复制过来，然后点击确认即可。  
	  ![][pt_12]  
	  
   5. 接下来重启一下spyder或者其他你们使用的编译器就好了。  

2. 将chrome driver放入已经存在环境变量当中的路径当中，至于什么路径存在可以参照上面的方法，上面添加路径的地方也会显示其他存在于环境变量中的路径，选一个即可。  

第一次写文章时因为Google那个路径已经存在于环境变量中，因此没有出现报错，导致博客有误，敬请见谅！  


转载请注明：[南梦婷的博客](https://norah2.github.io) » [点击阅读原文](https://norah2.github.io/2019/05/06/selenium_install/)   

<!--以下是本文用到的链接-->  
[pt_01]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/17_selenium_install/01.png
[pt_02]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/17_selenium_install/02.png
[pt_03]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/17_selenium_install/03.png
[pt_04]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/17_selenium_install/04.png
[pt_05]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/17_selenium_install/05.png
[pt_06]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/17_selenium_install/06.png
[pt_07]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/17_selenium_install/07.png
[pt_08]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/17_selenium_install/08.png
[pt_09]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/17_selenium_install/09.png
[pt_10]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/17_selenium_install/10.png
[pt_11]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/17_selenium_install/11.png
[pt_12]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/17_selenium_install/12.png

[link_chromedriver]: http://npm.taobao.org/mirrors/chromedriver/  

{% endraw %}
