---
layout: post
title: GitNote软件——强力推荐  
date: 2020-04-03
description: 软件介绍  
categories: Install
tags: Install GitNote
author: NMt
mathjax: true
---

* content 
{:toc}

记笔记软件介绍——GitNote

<div style='display: none'>
@@@@
</div>





{% raw %}
前面的都是文字铺垫，可以直接跳过~~  

相信大家电脑上总会有一些用于记笔记的软件，我之前因为一些机缘巧合用过 Wiz 为知笔记，没有仔细用过，用过几次，这个软件界面做的确实不错，单从代码块和团队合作分享来看，就觉得很棒。  

由于没有长时间用过，所以也不盲目吹捧贬低，唯一一点让我没有选择用它的原因就是它需要付费，但是新用户会有一段时间的免费试用。为什么说付费就让我不用了呢，是因为笔记软件这个东西只是偶尔用用，而且里面的功能也没有好到让我心甘情愿去付钱，所以果断舍弃了。  

接下来，我经人推荐一直在用 evernote 印象笔记这款软件，界面比较简洁，虽然有些功能没有或者不够完善会导致一些不方便，但是都能克服。这款笔记不是很好的地方就是每月同步的笔记有流量限制，似乎是每月60M，一开始想想，谁写字能写60M；直到网课充斥在我的生活里，我才发现60M是多么的捉襟见肘，所以一冲动差点买了会员。印象笔记虽然功能比较齐全，但是功能都有点弱，比如代码块，只能支持无格式粘贴，没有任何的颜色区别，粘贴之后的代码简直都要不认识了；比如自带的截屏功能，截屏之后会自动创建一个笔记放截屏图片，而不会在粘贴板里支持随处粘贴；比如搜索，完全不支持正则表达式，也有可能是我不会用，但是我是真的认真找了，没有发现任何支持正则的痕迹；比如快捷键，这个是很反人类的，常用的快捷键在这个软件里面全部都不一样了，用起来很不方便……呃，就先吐槽到这，现实的一点是，虽然有很多小瑕疵，但是我仍旧用了两年，这也十分硬核说明这个软件很不错。  

直到昨天印象笔记的一个调查问卷，让我了解到还有很多其他的软件。在好奇的驱使下，我随手去百度了一下，这下好了，我知道了 GitNote 的存在，在看过B站的一个没有人类感情的视频介绍之后，GitNote成功俘获了我的心。铺垫这么长，后面就开始介绍了：

   
   
   
## 一、GitNote软件的安装以及配置  

### 下载  

百度网盘下载地址：[https://pan.baidu.com/s/18X3Hl0ZXcVk7FItRosHuaA](https://pan.baidu.com/s/18X3Hl0ZXcVk7FItRosHuaA)  
官网地址：[https://gitnoteapp.com/](https://gitnoteapp.com/)  

Windows安装和普通软件一样，双击即可，Linux下的需要更改文件的属性，增加可执行权限即可。  

### GitHub仓库连接  

这个软件会将你的笔记和远端的 GitHub 仓库连接在一起，也就是说，不管你的更新了什么笔记，都会在 GitHub 上存储一份，只要 GitHub 不倒，不管你在哪里，只要能够上网以及记得 GitHub 账号就可以查看你的笔记。  

因此在使用 GitHub 之前需要创建一个 GitHub 的账号，并且在账号下创建一个仓库存储笔记。如果之前没有使用过 GitHub 或者 了解过 GitHub 的人在一开始会很不习惯。我由于专业的关系，对 GitHub 比较熟，因此上手起来完全无压力，不过在我第一次创建 GitHub 账号之后立马放弃它了将近一年时间，服务器响应慢以及全英文完全阻止了我的脚步，直至后面才慢慢习惯。  

1.首先你需要有一个你自己的 GitHub 账号。  

2.在 GitHub 账号上创建一个私人仓库：  

在右上角的 “+” 的下拉框中点击 new repository ：  
![][pt_01]  
  
在新出先的页面中填写 repository 的名字，属性设置为 private 私人（即不公开），勾上初始化，如果没有勾上则创建之后在repository中新建一个文件就可以手动完成 initial：  
![][pt_02]  
  
完成之后的样子是这样的：  
![][pt_03]  

3.打开软件 GitNote，点击 clone a repository：  
![][pt_04]  

如果是第一次登录的话，没有最左边的 recently opened project；而右上角可以关联你的其他社交账号，包括QQ、微信、微博，还有打赏、反馈、官网三个按钮；右下角可以选择语言，暂时只支持英文、中文简体、中文繁体三种；Add a locally existing git repository 是加载一个本地的 git repository；我们是连接 GitHub 上的远端 repository，因此选择 clone a repository。  

4.填写配置参数：  
![][pt_05]  
  
完成之后是这样的：  
![][pt_06]  

这样子基本上算是已经配置完成，页面各部件的用途：  
![][pt_07]  
   
   
   
## 二、插件介绍  

插件安装：  
![][pt_08]  
直接点击右上角绿色的 install 就可以进行安装了。  

### kityminder  

这里的 kityminder 插件能够连接 KMind 思维脑图，这和[百度脑图](https://naotu.baidu.com/)官网是一样的，画出的思维导图可以进行导出，也可以保存在 GitNote 笔记中：  
![][pt_09]  

### grapheditor  

这里的 grapheditor 插件是用于画流程图的，同样可以进行导出等一些操作：  
![][pt_10]  

### pomodoro  

这里的 pomodoro 插件则是一个番茄计时器：  
![][pt_11]  

### revealjs  

这个插件则是可以支持使用 Markdown 做演示文档，其中可以用三个短横线做分页符：`---`，使用`Note:`开头的文字是注释，在演示时不会显示出来。  

例如在md文件输入以下内容：  

![][pt_12]  

revealjs的效果如下所示：  
![][pt_13]  
![][pt_14]  
![][pt_15]  
![][pt_16]  

左下角的按钮则是导出为html文件，导出到指定文件夹，点开名为 index.html 的本地网页，就可以看到PPT：  

![][pt_17]  

右下角仍旧支持左右切换页面。  

### 图床  

GitNote中有四种图床，github、smms、imgur、local，将图片进行拖拽就可以实现上传：  

![][pt_18]  

smms是不需要设置的，安装smms的插件就可以使用，图片会上传到smms服务器；  

imgur是一个免费的图片空间，没有空间和流量的限制，默认会上传到gitnote的账号下面，不会进行任何的处理，同样可以申请自己的imgur账号，如果有自己的账号，则在插件的设置中填写自己的client id并保存即可：  

![][pt_19]  

当图片仅仅想放在本地，即可上传到本地图床local，拖拽之后就可以存储在local文件下，按照 local/年/月/日 格式进行存储；  

github 则是将图片上传到 github 上，需要进行一些配置，创建一个图片存储仓库：  

![][pt_20]  

接着点击头像，在下拉框中点击 Settings：  

![][pt_21]  

点击最下面的 Developer Settings：  

![][pt_22]  

点击左侧的 Personal access tokens，然后点击右边的 generate new token：  

![][pt_25]  

接着会出现一个确认GitHub密码的页面：  

![][pt_23]  

确认密码之后，就这可隐形设置token，其中需要为token遍写一个名字，还需要设置权限：  

![][pt_24]  

设置完成之后点击 Generate token 即可生成。

将生成的一串token码，填入GitNote中github插件的设置中：  

![][pt_26]  

这样就已经配置完成，你上传到github图床上的图片都会出现在你刚刚设置的images仓库中。  

   
   
   
## 三、其他功能  

### 发布  

我们还可以将我们的笔记发布在网络上供其他人访问，我们直接点击发布按钮，弹出的框点击确认：  

![][pt_27]  

这样就发布完成，我们可以在账号中进行查看或者删除操作：  

![][pt_28]  

接着我们就可以在浏览器上通过连接进行访问查看了：  

![][pt_29]  

这篇笔记进行发布之后会上传到 GitNote 服务器上，生成一个链接进行访问，但 GitNote 只保留24小时，24小时候服务器自动删除，同时任何时间都可以手动删除。  

### 收藏  

首先从[谷歌插件商店](https://chrome.google.com/webstore/detail/gitnote-%E6%94%B6%E8%97%8F/penfjgfngaanjogjdpdnibpeijnpbglc)中进行插件安装：  

![][pt_30]  

在 chrome 中点击插件，选择需要保存的内容，点击提交即可将网页中的内容保存成笔记：  

![][pt_31]  

------
写在最后：  

请允许我盲目吹捧以下这个软件，真的自从认识这个软件之后各种无脑吹，甚至于写了这个巨详细的教程，官方出的教程视频我0.75的速度看了三遍，一步一步跟下来的，记录出了我认为比较重要的东西，当然自然是原视频最为详细。这么好用的软件，为什么这么少人用！！没天理！！希望我用过之后不要让我打脸！！


参考文献：  

blibli上的简介链接地址：https://www.bilibili.com/video/av43903167/  

转载请注明：[南梦婷的博客](https://norah2.github.io) » [点击阅读原文](https://norah2.github.io/2020/04/03/gitnote_intro/)   

<!--以下是本文用到的链接-->  

[pt_01]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/48_gitnote_intro/01.png
[pt_02]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/48_gitnote_intro/02.png
[pt_03]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/48_gitnote_intro/03.png
[pt_04]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/48_gitnote_intro/04.png
[pt_05]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/48_gitnote_intro/05.png
[pt_06]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/48_gitnote_intro/06.png
[pt_07]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/48_gitnote_intro/07.png
[pt_08]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/48_gitnote_intro/08.png
[pt_09]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/48_gitnote_intro/09.png
[pt_10]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/48_gitnote_intro/10.png
[pt_11]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/48_gitnote_intro/11.png
[pt_12]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/48_gitnote_intro/12.png
[pt_13]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/48_gitnote_intro/13.png
[pt_14]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/48_gitnote_intro/14.png
[pt_15]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/48_gitnote_intro/15.png
[pt_16]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/48_gitnote_intro/16.png
[pt_17]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/48_gitnote_intro/17.png
[pt_18]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/48_gitnote_intro/18.png
[pt_19]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/48_gitnote_intro/19.png
[pt_20]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/48_gitnote_intro/20.png
[pt_21]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/48_gitnote_intro/21.png
[pt_22]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/48_gitnote_intro/22.png
[pt_23]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/48_gitnote_intro/23.png
[pt_24]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/48_gitnote_intro/24.png
[pt_25]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/48_gitnote_intro/25.png
[pt_26]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/48_gitnote_intro/26.png
[pt_27]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/48_gitnote_intro/27.png
[pt_28]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/48_gitnote_intro/28.png
[pt_29]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/48_gitnote_intro/29.png
[pt_30]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/48_gitnote_intro/30.png
[pt_1.]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/48_gitnote_intro/31.png)

{% endraw %}
