---
layout: post
title: "GitHub Pages + Jekyll 搭建博客"
date: 2019-03-02
description: GitHub Pages / Jekyll
categories: Blog
tags: Blog Jekyll GithubPages
author: NMt
mathjax: true
---

* content
{:toc}

使用Jekyll在Github Pages上搭建属于自己的博客。  

<div style='display: none'>
@@@@
</div>





{% raw %}
在一年之前我看到我仰慕的一个大牛学长拥有自己的域名，并且租服务器搭建了属于自己的网站后，就十分向往能拥有一个自己的域名和网站，这也就成为了我的一个小目标。但是由于各种因素导致这仍旧只是我手账本上的一个 flag 。

直到这学期开了学之后，在老师的强压之下，我终于提高了搭建博客在我心中的重要性，然后我花费了四天的时间进行探索和尝试，终于从别人GitHub上面fork了这个心仪的博客模板，经过我的改造，成了现在这个大家眼里看到的博客。

初次搭建博客，还请大家可以在评论区里留言，多多指教 ~~

现在细数一下我搭建博客一路踩过的坑

下面是整个的流程：

1. 注册GitHub账号  
2. 安装Git  
3. 搭建项目(这个不是搭建博客的主要流程，可跳过)  
4. 安装Jekyll  
5. 搭建博客  
6. 复制项目到本地  
7. 编辑项目中的文档  
8. 撰写博客  

## Github账号注册

账号注册没有难度，和一般的账号注册没什么太大的区别。

## 安装Git

我的账号是在半年前注册的，注册之后就安装了Git，那时还没有博客，因此现在只能略讲。

> [Git安装教程 + SSH 配置](https://norah2.github.io/2019/03/02/Git_install/)

## 学习搭建项目

（这里和搭建博客没有太大关系，算是一个基础。）

点击  your profile  就可以进入到自己的个人资料页面，接下来点击 Repositories 就能看到你所拥有的全部项目。

![][pt_01]  

点击右上角的 new 就可以新建一个仓库。

![][pt_02]  

按照图片设置好之后，点击 `create repository` 即可新建一个项目。

![][pt_03]  


## 安装 `Jekyll`

首先进入 [rubyInstall官网](https://rubyinstaller.org/downloads/)，点击下载安装包。安装一路next即可。

ruby仅仅只是给 Jekyll 搭建了一个环境，这时我们还需要借助另一个工具帮助我们安装 Jekyll，这里贴上下载的官方文档：[Download RubyGems](https://rubygems.org/pages/download)

由于我用的是 Windows 系统，所以选择了`zip`这个后缀名的安装包。

下载完成之后打开 git 命令行（cmd也可以，不过不推荐，据说容易报错，但是我自己没有报过错，所以没有发言权），在命令行中 cd 到 RubyGems 解压的目录下，执行下面的操作：

```
ruby setup.rb
```
然后只需要等着，出现下面这个就安装成功了。如果没有成功的话可以看报错提示或者百度自行安装。

![][pt_15]  

接下来用 gem 安装 Jekyll

```
gem install jekyll
```
在 git 中输入下面代码检查是否安装成功
```
jekyll -v
```
出现这个就表示已经安装成功（版本号可能会有不同）

![][pt_05]  

### 这里插入一段安装Jekyll的报错
	
安装完 Jekyll ，输入命令 `jekyll server` 想尝试一下，结果发现报错。

![][pt_09]  

百度之后说是要安装 bundle ，那就来试试吧！ 

```
bundle install
```

![][pt_10]  

安装完成之后，继续运行 `jekyll server`，然而仍旧报错……

![][pt_11]  

最终 get 到了正确的方法：

>这个错误的重点在Could not find ffi-1.9.18，使用gem list查看你安装的ffi版本，应该不是1.9.18版本。

### 解决办法：

>打开目录下的Gemfile.lock文件，找到修改对应的版本号，修改为你电脑安装的版本就可以了。

然后就开始修改 Gemfile.lock 中的内容，并且重新再来一次 `bundle install`:

![][pt_12]  

终于安装好了，然而……

![][pt_13]  

根据提示，仍旧不死心的继续尝试 ：

![][pt_14]  

终于搞定！！！


## 搭建博客

搭建博客有很多种办法，尤其在 GitHub Pages 自由度很高的网站上，整个网站任何一个地方都可以自己设置，不受其他的限制。一般常见的就是 Jekyll 工具和 hexo工具，Jekyll 工具是 github pages 推荐的一个工具。（你以为我是因为这个才选择用 Jekyll 的吗？工具自然是好用最为重要，但是在我浏览过大量其他人的博客时，被推荐的较多的是 hexo 方法，我自然首先将矛头转向 hexo， 但是由于路途艰难，最终放弃了，果然是 Jekyll 更适合我这种小白，两个字：简单！！）由于本身也是一个小白，所以下面只介绍自己用的最简单的方法。

当你已经把 Jekyll 安装好之后，在 [Jekyll Theme 官网](http://jekyllthemes.org/) 上面选择自己喜欢的主题风格。

![][pt_06]  

选择好一个主题后点进去，点击 `HomePage` ，这时进入了存储这个博客模板项目的 GitHub 仓库里。  

![][pt_07]  

接下来点击右上角的 `Fork` 按钮，这时这个项目就会被复制到自己的仓库中。从自己的账号中进入到这个仓库中，点击 `setting` ，下面的 Github Pages 里面有一个域名。可以通过这个域名看到自己博客现在的模样。

![][pt_04]  

这时博客已经初步搭建好了，接下来就是如何按照自己的意愿更改其中的细节使之完完全全成为自己的博客，并且要知道如何管理以及往上面发文章。  

![][pt_08]  

## 将项目复制到本地

这个过程对于已经学习过 Git 的读者来说，是很简单的，这里也只是简略的提及一下。

首先，在本地创建一个文件夹，作为存储项目的一个目录，并且进行初始化：
```
git init
```

其次，建立 Git 与仓库之间的连接：

```
git remote add origin git@github.com:username/name.github.io
```
**注明：这里最好不用 https 的链接，有可能会报错，我最初用的就是 https，不幸报错，最后更改成 ssh 的链接则没有问题了。**
**ssh链接的格式：git@github.com:username/name.github.io，其中username是用户名，可以基本等同于你GitHub账号的网址，name.github.io是你的仓库名（放博客模板的那个仓库）**

接着，将远程仓库的项目克隆到本地：
```
git pull origin master
```
**注明：这里的master是分支名，有的模板上面的主分支可能不是叫master，写的时候根据实际情况更改master那个地方即可，eg：gh-pages也是常用的主分支的命名**

这时，我们已经把所有的文件全部拷贝到本地了。

## 对项目其中某些文件进行更改

主要修改的就是_config.yml中的文件，这里附上官方中文文档的链接：[配置](https://www.jekyll.com.cn/docs/configuration/)

具体的就不再详述~

## 写博客

在放博客项目的文件夹里创建一个文件夹 `_post`，将写的md文件放在此目录下即可。



写到这里也差不多了，这是我的第一篇博客，文中如有不正确的地方，欢迎与我联系或是留言评论~

---
2019.7.29

时隔几个月，我的电脑由于某些原因造成不得已重装系统，因此也不得不重新安装一遍Jekyll，这次又出现了新的问题，所以我进来更新一下问题。   

按照我之前写的步骤安装完成之后，输入`jekyll server`会报错，如下图所示：  

![][pt_16]  

各种百度无果，直到我发现了这位大牛的博客：[https://blog.csdn.net/xftony/article/details/80536507](https://blog.csdn.net/xftony/article/details/80536507)  

大致意思就是说安装Jekyll之后需要安装其他插件，至于安装什么插件，这需要看博客中`_config.yml`文件中的配置，  

在该配置文件中会有这样的一句设定：`gems: [jekyll-paginate,jekyll-sitemap]`  

这句就是指定了用什么样的gem插件，知道了用什么插件之后，接下来就是安装了：

```
gem install jekyll-paginate jekyll-sitemap jekyll-feed
gem install jemoji
...
```

安装之后：

![][pt_17]  

安装之后就不会报错了!!!

![][pt_18]  

看到那位大牛老哥里面还有其他的东西，我就先“抄”下来做下记录，以防以后需要：

更新gem源：

```
gem sources --remove https://rubygems.org/
gem sources -a https://ruby.taobao.org/
gem update --system
```

等以后有机会再重装的时候再过来补充吧~

转载请注明：[南梦婷的博客](https://norah2.github.io) » [点击阅读原文](https://norah2.github.io/2019/03/02/blog_github/) 

<!--本文用到的链接-->

[pt_01]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/01_blog/01.png
[pt_02]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/01_blog/02.png
[pt_03]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/01_blog/03.png
[pt_04]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/01_blog/04.png
[pt_05]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/01_blog/05.png
[pt_06]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/01_blog/06.png
[pt_07]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/01_blog/07.png
[pt_08]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/01_blog/08.png
[pt_09]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/01_blog/09.png
[pt_10]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/01_blog/10.png
[pt_11]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/01_blog/11.png
[pt_12]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/01_blog/12.png
[pt_13]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/01_blog/13.png
[pt_14]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/01_blog/14.png
[pt_15]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/01_blog/15.png
[pt_16]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/01_blog/16.png
[pt_17]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/01_blog/17.png
[pt_18]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/01_blog/18.png


{% endraw %}