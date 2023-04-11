---
layout: post
title: pytorch环境搭建
date: 2023-04-10
description: 记录了安装CUDA、CUDNN、Pytorch等组件的流程，完整搭建Pytorch环境。
categories: Python
tags: pytorch
author: NMt
mathjax: true
---

* content
{:toc}

记录了安装CUDA、CUDNN、Pytorch等组件的流程，完整搭建Pytorch环境。

<div style='display: none'>
@@@@
</div>





{% raw %}
安装Pytorch的流程：  
[安装Visual Studio](#1-);  
[安装CUDA](#2-);  
[安装CUDNN](#3-);  
[安装pytorch](#4-)。  

## 一、安装Visual Studio  

CUDA安装之前需要先安装Visual Studio，因为CUDA依赖Visual Studio的组件，否则安装过程中会出现下述情况：  

![][pt_01]  

Visual Studio在官网下载，Community的免费版本就够用了：[Visual Studio][link_01]  

安装流程如下：  

首先安装安装程序：  

![][pt_02]  

在选择组件的时候我只选择了Visual Studio核心编辑器以及C++桌面开发这两个：  

![][pt_03]  

最后选择安装位置，我这里更改了新的安装位置：  

![][pt_04]  

最后点击安装即可。  

我一开始下载的最新的2022版本，安装完成之后还是会出现检测不到Visual Studio的情况，最后得到的结论是版本不匹配，最后下载的是2019版的Visual Studio，就可以正常安装后续的CUDA了。  

## 二、安装CUDA  

安装CUDA的时候需要查看自己电脑上GPU能支持的最高版本的CUDA，一共有两种方式。一种是打开cmd，输入命令`nvidia-smi`：  

![][pt_05]  

另一种方式是在桌面的界面，右键-NVDIA控制面板-帮助-系统信息-组件，可以看见目前GPU的驱动软件版本以及能够支持的最高的CUDA版本：  

![][pt_06]  

可以从图片中看到，我的电脑上GPU的驱动版本是457.49；能够支持的CUDA最高版本是11.1。接下来就可以去[CUDA官网][link_02]上下载对应的CUDA版本：  

![][pt_07]  

因为我的电脑上能支持的最高版本的CUDA是11.1，所以我选择的就是11.1。  

点进链接之后可以根据自己电脑的实际情况选择相关的配置，我的电脑是Windows系统、64位、Win10，并且我是需要本地安装：  

![][pt_08]  

选择完成之后，点击下载即可，下载完成后就可以安装了。  

一开始会让你选择一个临时解压路径，需要注意的是这个路径在完成安装后会自动删除，因此千万不要和后续的安装位置选成同一个文件夹，否则会出现CUDA安装完成后找不到安装文件目录，如果实在弄不清楚的话，可以直接保持默认文件路径：  

![][pt_09]  

开始安装：  

![][pt_10]  

这里需要选择自定义安装：  

![][pt_11]

这里如果当前版本高于新版本的话，就把勾去掉，其他我都是保持默认的全部勾选：  

![][pt_12]  

这里就是CUDA的安装位置，再强调一遍，和前面临时解压路径一定要不一样，否则刚安装完就全部删掉了：  

![][pt_13]  

接下来点击安装：  

![][pt_14]  

安装结束的页面，没有什么需要注意的点：  

![][pt_15]  

![][pt_16]  

安装完成之后需要检测CUDA是否安装成功，在cmd中输入命令`nvcc -V`，如果能够显示CUDA的版本信息就表明CUDA安装成功：  

![][pt_17]  


## 三、安装CUDNN  

安装完CUDA之后就可以安装CUDNN了，在[CUDNN官网][link_03]上下载即可：  

![][pt_18]  

下载CUDNN的话需要先注册一个账号，流程很简单，按照提示完成即可，这里就不展示了。下载完成之后是一个`.zip`后缀的文件，解压缩之后把文件的名称改成cudnn，然后将该文件整体剪切到CUDA的安装路径下：  

![][pt_19]  

接下来配置环境变量，一个是CUDA安装路径下的`./extras/CUPTI/lib64`，还有一个是CUDNN的bin路径：  

![][pt_21]  

到这里基本上就算安装成功了，后面安装pytorch就好！  

## 四、安装pytorch  

进入[pytorch官网][link_04]，根据自己的情况选择相应的选项：  

![][pt_20]  

选择完成之后复制command里面的代码，然后粘贴到cmd中就可以进行安装了！由于这里显示的只有11.7和11.8两个版本，我能够支持的是11.1，所以我需要找之前的版本，这里我也把链接贴出来：[pytorch-previous-versions][link_05]，这个链接可以在官网上找到的。  

我的command代码是：`pip3 install torch==1.8.2 torchvision==0.9.2 torchaudio==0.8.2 --extra-index-url https://download.pytorch.org/whl/lts/1.8/cu111`，下面就是正在安装的状态：  

![][pt_22]  

最后输入：`print(torch.cuda.is_available())`，如果返回值为True，则说明安装成功，如果是False则说明安装失败。  

![][pt_23]  

转载请注明：[南梦婷的博客](https://norah2.github.io) » [点击阅读原文](https://norah2.github.io/2023/04/10/pytorch_install/) 

<!--本文用到的链接-->

[pt_01]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/80_Pytorch_install/01.png  
[pt_02]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/80_Pytorch_install/02.png  
[pt_03]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/80_Pytorch_install/03.png  
[pt_04]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/80_Pytorch_install/04.png  
[pt_05]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/80_Pytorch_install/05.png  
[pt_06]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/80_Pytorch_install/06.png  
[pt_07]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/80_Pytorch_install/07.png  
[pt_08]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/80_Pytorch_install/08.png  
[pt_09]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/80_Pytorch_install/09.png  
[pt_10]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/80_Pytorch_install/10.png  
[pt_11]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/80_Pytorch_install/11.png  
[pt_12]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/80_Pytorch_install/12.png  
[pt_13]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/80_Pytorch_install/13.png  
[pt_14]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/80_Pytorch_install/14.png  
[pt_15]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/80_Pytorch_install/15.png  
[pt_16]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/80_Pytorch_install/16.png  
[pt_17]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/80_Pytorch_install/17.png  
[pt_18]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/80_Pytorch_install/18.png  
[pt_19]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/80_Pytorch_install/19.png  
[pt_20]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/80_Pytorch_install/20.png  
[pt_21]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/80_Pytorch_install/21.png  
[pt_22]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/80_Pytorch_install/22.png  
[pt_23]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/80_Pytorch_install/23.png  

[link_01]: https://visualstudio.microsoft.com/zh-hans/downloads/  
[link_02]: https://developer.nvidia.com/cuda-toolkit-archive  
[link_03]: https://developer.nvidia.com/rdp/cudnn-download  
[link_04]: https://pytorch.org/  
[link_05]: https://pytorch.org/get-started/previous-versions/  

{% endraw %}
