---
layout: post
title: Gitee图床改换成阿里云OSS图床
date: 2023-01-11
description: 基于阿里云OSS进行图床创建。
categories: Blog
tags: Blog ImageBed OSS
author: NMt
mathjax: true
---

* content
{:toc}

前段时间比较忙，本来想要写点blog，但是因为发现文章中的图片没有办法显示了，所以就先放下了。现在忙完了就开始研究研究博客出了啥问题。

<div style='display: none'>
@@@@
</div>





{% raw %}
博客图片无法显示其实很明显是图片存放这一块出现了问题，所以我首先去检查了一下Gitee（上次重整博客的时候用Gitee作为图床使用的），排除出来是因为Gitee自己本身关掉了作为图床的功能。因此不得不换一个图床，上次在找合适的图床的时候已经尝试过很多种，各有各的特色，但尝试过的平台真的不太适合我这种blog的框架，会让整体blog框架变得特别乱，百度之后看见有人在介绍阿里云的OSS，其实一开始对于这种平台我有点不太放心，主要是在收费上。最后决定还是尝试一段时间，接下来就是我部署的步骤，中间虽然出了一些问题，但是总体来说还是比较顺利的（所有出现的问题都被我解决了）。

主要大概有三个部分需要设置：第一个部分是阿里云上平台的操作；第二个部分是关于PicGo（上传图片的工具）关于OSS图床的相关参数配置，第三个部分是blog内容上的修改。主要是阿里云上平台的操作，其次是关于PicGo
中关于图床的参数配置，blog里面没什么太多要操作的，只是更改一下图片的链接。这里还是全部都记录一下，虽然原理简单，但是操作的时候会涉及到一些小细节。
  
  
  
# OSS相关的操作  

## 购买阿里云OSS资源包  

* 需要去阿里云官网上购买OSS资源包：  

![][pt_01]  

可以根据自己的需求选择套餐，我买的是比较便宜的，想先试试。  

## 在阿里云账号下创建用户 

（这里的用户和阿里云账号不是一个东西，简单分开理解）

* 首先在阿里云账号下点击访问控制：  
![][pt_02]  

* 其次在左边的身份管理下的用户中创建用户：  
![][pt_03]  

* 接着在创建用户的时候填相关的信息：  
![][pt_04]  

显示名称可以单纯解释一下这个用户创建的目的是什么，可以填自己想填的内容，我这里填的比较简单。  

**注：需要勾选OpenAPI调用访问。**  

* 创建用户之后保存相关信息  
![][pt_05]  

点击下载csv文件就可以下载相关的表格，表格中存储了用户信息，可以保存下载，需要用的时候复制粘贴就好了。其中AccessKey ID和AccessKey Secret一会儿会用到。   

* 给用户授权  
![][pt_06]  

找到刚刚创建完成的用户，勾选之后点击添加权限，选择右边的AliyunOSSFullAccess的权限策略就可以了。  

## 创建Bucket  

Bucket我认为和大数据的存储结构有关系，如果想要详细了解的话，可以去找一找大数据的存储结构之类的文章看看。  

* 开通对象存储服务OSS  

要创建Bucket就需要去OSS工作台，进去的时候发现没有开通这个功能。点击阿里云之后找到对象存储OSS，显示没有开通之后点击开通即可。  

![][pt_08]  

![][pt_07]  

显示开通成功即完成，点击管理控制台就可以创建Bucket。  

![][pt_09]  

* 创建Bucket  

点击Bucket列表之后，在点击创建Bucket：  

![][pt_10]  

在创建Bucket的时候，可以选择是否开通相关的功能，如果有需求的话可以去研究一下，我这里没有什么特别的要求，只需要显示博客图片即可，所以只需要在读写权限中开启**公共读**。  

![][pt_11]  

这里是一点关于Bucket的介绍与解释：  

![][pt_12]  

# PicGo的配置  

* 看图片就可以理解了：  

![][pt_13]  

# blog中的更改  

将原来的图片链接的前缀更改为阿里云的前缀即可。  

https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/图片名称

https://gitee.com/nora2nan/blog-image/raw/master/图片名称

我原来的Gitee图床的前缀是下面这一条，将前面相同的部分改为现在阿里云上的图片链接的前缀即可。直接在编辑项目中用替换功能即可。  

---

中间配置的过程中，遇到了一个问题，将本地修改之后的blog文件push到github之后，github仓库中的文件没有变化，但是本地显示已经上传，这个问题排错了一会儿，发现是github上的公钥失效了，重新填上公钥即可，一般来说公钥的存放位置在C盘的用户下的.ssh目录下，该目录可能被隐藏，所以需要点击一下显示隐藏目录。  

在更改blog中图片路径的时候，我在想是否能够将这个前缀设置为一个变量，后续更改的时候只需要更改一个位置，而不是像现在这样一个一个替换，虽然可以用自带工具选择全部替换，但是仍旧有些麻烦。由于没有详细学过关于SCSS相关的内容，所以不太清楚，百度之后也没有找到确切的答案，后面有了解答之后再进行补充。  

转载请注明：[南梦婷的博客](https://norah2.github.io) » [点击阅读原文](https://norah2.github.io/2023/01/11/oss_ImageBed/) 

<!--本文用到的链接-->

[pt_01]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/71_OSS_ImageBed/01.png
[pt_02]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/71_OSS_ImageBed/02.png
[pt_03]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/71_OSS_ImageBed/03.png
[pt_04]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/71_OSS_ImageBed/04.png
[pt_05]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/71_OSS_ImageBed/05.png
[pt_06]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/71_OSS_ImageBed/06.png
[pt_07]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/71_OSS_ImageBed/07.png
[pt_08]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/71_OSS_ImageBed/08.png
[pt_09]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/71_OSS_ImageBed/09.png
[pt_10]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/71_OSS_ImageBed/10.png
[pt_11]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/71_OSS_ImageBed/11.png
[pt_12]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/71_OSS_ImageBed/12.png
[pt_13]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/71_OSS_ImageBed/13.png

{% endraw %}
