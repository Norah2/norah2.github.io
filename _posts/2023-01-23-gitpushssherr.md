---
layout: post
title: git push出现ssh端口错误的解决方案  
date: 2023-01-23
description: git push 出现问题，更改ssh端口解决上传错误。  
categories: Blog
tags: Blog
author: NMt
mathjax: true
---

* content
{:toc}

使用`git push`命令推送项目到github仓库的时候发现上传不了，尝试了一些方式，最终顺利上传了，因此记录一下。  

<div style='display: none'>
@@@@
</div>





{% raw %}
前几天使用`git push`上传的时候出现下述问题：  

![][pt_01]  

百度搜索之后总共有两种解决方案：一、将ssh方式改为https方式，即关联远端仓库的时候换一种方式，这种方式的配置在`~/.git/config`的文件中更改。二、将ssh的端口改掉，即在`~/.ssh/config`的文件中更改。  

这两种方式在我的电脑只有第二种能够成功，第一种还是会出现错误。这里我简单记录一下：  

## 第一种：将ssh方式更改为https方式。  

* 找到`~/.git/config`，可以使用命令：`git config --local -e`，出现下述内容：  

```
[core]
	repositoryformatversion = 0
	filemode = false
	bare = false
	logallrefupdates = true
	symlinks = false
	ignorecase = true
[remote "origin"]
	url = git@github.com:Norah2/norah2.github.io.git
	fetch = +refs/heads/*:refs/remotes/origin/*
[branch "master"]
	remote = origin
	merge = refs/heads/master
[user]
	name = nora
	email = norah2nan@gmail.com
```

* 将其中的`url = git@github.com:Norah2/norah2.github.io.git`改为`url = https://github.com/Norah2/norah2.github.io.git`，其实就是将原来的`git@`更改为`https://`，再将冒号`:`改为斜杠`/`。如果实在不会改的话，可以登录到github上，进入到远端的仓库中，点击code之后选择https，直接复制上面的链接即可。  

更改完成之后保存退出，我再次尝试上传的时候仍旧出现了问题，所以我尝试了第二种方式。（第一种没有成功可能是因为没有刷新配置，后面使用第二种方式成功了，所以没有继续往下测试）  

## 第二种：更改ssh的端口。  

* 新建一个`~/.git/config`文件：`vim ~/.git/config`。  

* 在里面新增内容：  

```
Host github.com
User norah2nan@gmail.com
Hostname ssh.github.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/id_rsa
Port 443
```

在第二行的User后面填上自己的邮箱，填好之后保存退出。  

* 需要重新使用命令`git config user.name xxx`以及`git config user.email xxx` 设置一下，为了刷新配置用的。  

![][pt_02]  

如果没有这一步的话`git push`仍旧会出问题：  

![][pt_03]  

* 可以使用命令`ssh -T git@github.com`检测ssh是否正常：  

![][pt_04]  

上述操作完成之后，再次`git push`上传才恢复正常：  

![][pt_05]  



转载请注明：[南梦婷的博客](https://norah2.github.io) » [点击阅读原文](https://norah2.github.io/2023/01/23/gitpushssherr/) 

<!--本文用到的链接-->

[pt_01]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/72_gitpushssherr/01.png
[pt_02]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/72_gitpushssherr/02.png
[pt_03]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/72_gitpushssherr/03.png
[pt_04]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/72_gitpushssherr/04.png
[pt_05]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/72_gitpushssherr/05.png


{% endraw %}
