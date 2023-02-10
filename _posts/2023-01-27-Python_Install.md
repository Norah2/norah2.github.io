---
layout: post
title: Python安装详细流程与个人见解  
date: 2023-01-27
description: 使用pyenv工具管理python版本，使用virtualenv以及virtualenvwrapper工具管理python的环境，同时安装常用的python编译器。  
categories: Install
tags: Install Python
author: NMt
mathjax: true
---

* content
{:toc}

使用pyenv、virtualenv、virtualenvwrapper、VSCode、Jupyter notebook、Spyder等工具实现对python版本管理、环境管理以及常用编译器解释器设置。  

<div style='display: none'>
@@@@
</div>





{% raw %}

# 关于Python的思考流程和一些废话（可以略过）  

最近想要再次开始学习关于python之类的东西，但是之前电脑因为硬盘坏了所以重装了系统，导致之前用的python环境都没有了，因此现在需要重新安装。  

用了这么久的pyhton，很明显可以知道很多项目会依赖不同的python版本，要么是代码语法格式略有差异，要么是依赖的第三方库兼容的python版本不同，这就会经常出现在上一个项目运行的好好的包，在下一个项目中就不能兼容了，一个包的版本经常一会儿升高一会儿降低，一会儿又要更换一个版本的python运行。  

其实这种情况解决的方式也很简单，就是为项目创建一个干净的只属于它的python环境，但这又涉及到环境管理的问题了，还有一个python版本共存的问题。  

我最开始安装python一般都是安装python官方软件或者anaconda等集成平台，然后将安装路径添加到环境变量中即可。按照我目前的理解来看，添加环境变量能够让我们在cmd中对python进行一些操作（eg：pip）或者直接在cmd中打开python的console。  

后来被老师推荐了winpython这个软件，它是一个免安装的python，内部集成了spyder、jupyter notebook、console等等一些编辑器，还自带一个python command prompt，这样winpython就能自成一个环境，对python的所有操作都可以在这一个文件夹内部搞定。安装第三方库使用python command prompt即可，打开jupyter notebook也是用python command prompt，安装的第三方库也不会牵涉到其他文件夹里。这样对python操作起来就会更加安全，也很方便，但是winpython推广范围可能有些小，因此资源找起来有点麻烦。各个环境内部很多内容都是重复的，有点浪费空间，但是这个不是重点，后面自己做的其实也存在这个问题。目前电脑上只有一个python3.8版本的winpython，其他版本的安装之后没有办法用，可见也容易出错，出错之后还比较难找资源和解决方案。  

接着我又接触到了其他的一些编译器，例如我后来用的IEDA，这个用来写项目非常方便，代码命令联想填充、格式、纠错功能都做的非常人性化，但是在启动的时候比较慢，更加适合台式机，并且是长期不会关机的情况。我在做历时比较长比较完整的项目的时候用的都是IDEA，比如我的毕设就是用的IDEA，还有一些结业论文都是用的这个软件。IDEA原本是用来写JAVA的，但是可以配置python的解释器，这样就可以完美的兼容python了（小提一下，它不支持C）。我在IDEA配置的python解释器都是winpython的核。  

结合这些我使用的经历，想到找一些工具组成一个大的python框架，能够很好的管理python版本、环境、项目、编译器等这四者之间的关系，尽可能的集合他们的优点，让自己能更方便的使用和管理python。  

首先我是根据多年的项目经验和用过多种编译器的经验来思考python相关的东西，这里是我思考的相关的内容：  

![][pt_01]  

在python里我将其分为三个部分：python、环境、编辑器。python会存在很多种版本，能够方便的实现python版本的共存和切换；环境其实就是python核以及各种第三方库集成的一个文件夹，环境一般都是和项目关联的，一个项目只能存在于一个环境中，一个环境可以支持多个项目；编辑器单纯是提供一个舒适的写代码平台，但是需要注意的是编辑器要能够方便的切换项目运行的环境，因为环境最终都是落实到项目代码上。  

# 安装pyenv，管理python  

pyenv是一个python版本的管理工具，可以实现多个版本python共存，以及不同版本之间的灵活切换。  

1、下载路径：https://github.com/pyenv-win/pyenv-win#readme  

2、解压到不带中文的目录下，更改文件名字为.pyenv  

![][pt_02]  

3、配置环境变量  

此电脑右击，选择属性，在关于界面中点击高级系统设置选项，即可进入环境变量窗口。或者直接搜索“编辑系统环境变量”也可以直接进入环境变量窗口。  

先新建一个系统变量，变量名设置为“PYENV”，变量值设置为刚刚pyenv解压目录的pyenv-win文件夹，即`“C:\Users\Nora\Desktop\.pyenv\pyenv-win”`：  

![][pt_03]  

双击系统变量的Path，在下面空白行中分别填入“%PYENV%\bin”、“%PYENV%\shims”：  

![][pt_04]  

4、检测是否安装成功，在cmd中输入`pyenv`，若出现下面这种情况即安装成功：  

![][pt_05]  

5、安装python，使用命令`pyenv install [版本号]`：  

![][pt_06]  

6、命令说明  

```
commands    -------------列出所有可用的pyenv命令
duplicate    -------------创建一个重复的python环境
local    --------------设置或显示特定于本地应用程序的Python版本
global    --------------设置或显示全局Python版本
shell    --------------设置或显示特定于shell的Python版本
install      --------------Python构建安装Python版本
uninstall    -------------卸载特定的Python版本
update      -------------更新缓存的版本数据库
rehash      -------------重新安装pyenv垫片（安装可执行文件后运行此操作）
vname       -------------显示当前的Python版本
version      -------------显示当前Python版本及其来源
version-name ----------------显示当前的Python版本
versions    -----------------列出pyenv可用的所有Python版本
exec        -----------------通过首先准备路径来运行可执行文件，以便选定的Python
which       -------------- 显示可执行文件的完整路径
whence     ---------------------列出包含给定可执行文件的所有Python版本
```

7、常用命令  

```
pyenv versions：列出当前系统中所有安装的python。
pyenv version：显示出当前使用的python。
pyenv global <python_version>：设置使用哪一个python。
pyenv install <python_version>：安装特定版本的python。
pyenv uninstall <python_version>：移除特定版本的python。
pyenv install -l：查看可安装的python。
pyenv local 3.7.4：为当前目录设置python 版本
pyenv local --unset ：取消当前目录设置的python 版本
pyenv shell pypy-2.2.1：指定当前shell使用的Python
pyenv shell --unset：当不再需要的时候，用--set来清除
```

# 安装virtualenv，管理环境  

virtualenv用于管理某个版本python下的多个环境。由此可见，在每个不同版本的python下都需要安装一遍virtualenv：  

`pyenv global 3.8.8`  

`pyenv local 3.8.8`（写到后面我思考了一下，认为在安装virtualenv的时候不需要设置local）  

`pip install virtualenv`  

这些命令都是在cmd下运行的，第一条命令是将系统默认的全局python版本改为3.8.8，第二条命令针对的是当前目录文件夹，这样就可以保证当前目录文件夹不会受到全局python版本的影响，且local的优先级高于global的优先级，如果想要当前文件夹跟随全局python版本的话就可以不设置locla参数，如果想要当前文件夹保持某个版本的python不变，就可以设置local参数。然后在3.8.8版本下安装virtualenv，如果还有其他版本的python，也需要这样做一遍，先切换python版本，然后使用pip工具安装。  

1、创建环境  

`virtualenv -p [制定 python 的版本路径] [python 虚拟空间的名称]`  

![][pt_07]  

2、激活环境（必须进入环境的Scripts目录下使用activate命令，名称可以省略）  

`activate [环境名称]`  

3、退出环境  

`deactivate`  

![][pt_08]  

# 安装virtualenvwrapper，简化管理环境操作  

1、在全局环境下输入命令：`pip install virtualenvwrapper-win` (注意这里后面必须要带-win)  

2、设置环境变量，变量名：`WORKON_HOME`; 变量值：`C:\Users\Nora\Desktop\envs`(该路径是创建环境的默认存放位置，如果没有设置的话，则是在用户下的env)  

![][pt_09]  

列出所有虚拟环境：`lsvirtualenv` or `workon` or `lsvirtualenv -b`  
激活虚拟环境：`workon venv`  
进入当前虚拟环境目录：`cdvirtualenv`  
进入当前虚拟环境的site-packages目录：`cdsitepackages`  
列出site-packages目录的所有软件包：`lssitepackages`  
退出虚拟环境：`deactivate`  
删除虚拟环境：`rmvitualenv venv`  
复制虚拟环境：`cpvirtualenv env1 env3` (我自己电脑上测试了没有这个命令，可能只有linux下支持吧)  
创建独立运行环境-命名：`virtualenv --no-site-packages --python=python3 venv`  #得到独立第三方包的环境，并且指定解释器是python3   


我按照上述方式设置之后，出现了一个问题：命令`workon [虚拟环境名称]`无法进入虚拟环境，搜索出来的解决方法大概是这么几种：  
一、不要用powershell，在cmd下可以正常进入；  
二、WORKON_HOME环境变量值设置的目录存在问题；  
三、更改过虚拟环境的默认路径，需要迁移；  
四、安装virtualenvwrapper第三方库的时候，需要注意如果你是在windows系统下，输入的安装命令应该是`pip install virtualenvwrapper-win`，有些人忘记加`-win`了；  
五、安装virtualenvwrapper第三方库的时候需要在全局环境下，而不是在某个环境下安装。  


我自己在安装的时候上述的问题全都排查过了，仍然没有办法使用workon命令进入虚拟环境，只能用virtual库下的`activate`命令激活。这个问题还有待解决……  

# 配置VSCode  

1、采用工作区设置默认解释器的方式（推荐）  

点击左侧的运行与调试，然后点击“create a launch.json file”，接着点击第一个Python File，即可创建一个`.\.vscode\launch.json`文件：  

![][pt_10]  

在`launch.json`同级目录下新建一个`settings.json`文件，里面设置两个变量：  

![][pt_11]  

`python.defaultInterpreterPath`变量后面写python所在的目录（某个环境的python.exe位置），后面再加一行`"jupyter.debugJustMyCode": true`即可。  

这样就为该项目指定了某个python环境。  

2、给VSCode设置解释器  

按照图中的方式选择虚拟环境：  

![][pt_12]

这两种方式，第二种方式的优先级会更高，当设置了第二种方式之后，第一种方式会失效。  

想继续使用settings文件为当前项目设置解释器的话，按住ctrl+shift+P，输入清除工作区解释器设置：  

![][pt_13]  

# 安装jupyter notebook  

在某个python版本下，输入`pip install jupyter`；或者`pip install jupyter -i https://pypi.tuna.tsinghua.edu.cn/simple/ --trusted-host pypi.tuna.tsinghua.edu.cn`  

![][pt_14]  


我在安装的时候报错了，升级了pip之后安装就没有问题了，升级pip输入`python -m pip install --upgrade pip`：  

![][pt_15]  

接下来测试是否能正常运行，进入某个python环境，然后输入`jupyter notebook`：  

![][pt_16]  

![][pt_17]  

运行正常，安装成功！

# 安装spyder  

下载软件：https://github.com/spyder-ide/spyder/  

先点击右边的Releases，然后点击.exe文件下载：  

![][pt_18]  

![][pt_19]  

我后来选了个4.2.5版本的下载，据说会比较稳定一点。  

下载非常非常非常慢！！！  

下载完成之后双击安装，一路next就可以了，这里就不贴图片了。  

进入spyder之后，可以按照下图中的任一方式设置解释器，一共三种：一、直接点扳手图标；二、Tools(选项卡)->Preferences；三、点击右下角的custom->change default environment in Preferences。还可以快捷键：CTRL+SHIFT+ALT+P  

![][pt_20]  

![][pt_21]  

![][pt_22]  

接下来会出现这样的问题：

![][pt_23]  

只需要进入对应的环境下，输入命令：`pip install spyder-kernels==1.10.0`  

![][pt_24]  

重新连接kernel之后就会发现它好了：  

![][pt_25]  

另外一种安装方法（我自己安装的时候，在3.6.4版本下出现了问题，缺少依赖包，但是pip的时候显示的又是正常的，没有弄成功过，但是细细研究一下的话应该可以解决；在3.8.8版本下能够成功）：  

在全局的python下，输入命令：`pip install spyder` ：  

![][pt_26]  

还有一种方式是下载whl文件到本地，然后再pip，我没有尝试过，但是和上面一种大同小异。  

# 换源的配置  

1、临时更换源  

在安装第三方库的时候想用 -i 参数指定国内的镜像源，然后报错了：  

![][pt_27]  

解决的方案是增加--trusted-host xxx：  

`pip install 安装包 -i https://pypi.tuna.tsinghua.edu.cn/simple/ --trusted-host pypi.tuna.tsinghua.edu.cn`  

`pip install 安装包 -i https://pypi.douban.com/simple/ --trusted-host pypi.douban.com`  

……以此类推  

2、永久使用  

`pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple/`  

![][pt_28]  

其他源：  

清华大学 ：https://pypi.tuna.tsinghua.edu.cn/simple/  
阿里云：http://mirrors.aliyun.com/pypi/simple/  
中国科学技术大学 ：http://pypi.mirrors.ustc.edu.cn/simple/  
华中科技大学：http://pypi.hustunique.com/  
豆瓣源：http://pypi.douban.com/simple/  
腾讯源：http://mirrors.cloud.tencent.com/pypi/simple  
华为镜像源：https://repo.huaweicloud.com/repository/pypi/simple/  

# 快速复制某个环境  

环境A（有很多基础包）=>  环境B（没有包）  

查看安装包的命令：`pip list` or `pip freeze`  

将环境A安装的包都重定向到一个txt文本文件中：`pip freeze > C:\Users\Nora\Desktop\requirements.txt`  

![][pt_29]  

进入到一个新的python环境中，然后将requirements.txt中的包安装上去：  

`pip install -r C:\Users\Nora\Desktop\requirements.txt`  

![][pt_30]  

完成之后看到这个就说明复制成功：  

![][pt_31]  

**注：mkvitualenv创建新环境不会把包继承过来：**  

![][pt_32]  

这篇文章写了好久，先这样吧。



转载请注明：[南梦婷的博客](https://norah2.github.io) » [点击阅读原文](https://norah2.github.io/2023/01/27/Python_Install/) 

<!--本文用到的链接-->

[pt_01]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/74_Python_install/01.png
[pt_02]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/74_Python_install/02.png
[pt_03]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/74_Python_install/03.png
[pt_04]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/74_Python_install/04.png
[pt_05]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/74_Python_install/05.png
[pt_06]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/74_Python_install/06.png
[pt_07]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/74_Python_install/07.png
[pt_08]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/74_Python_install/08.png
[pt_09]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/74_Python_install/09.png
[pt_10]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/74_Python_install/10.png
[pt_11]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/74_Python_install/11.png
[pt_12]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/74_Python_install/12.png
[pt_13]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/74_Python_install/13.png
[pt_14]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/74_Python_install/14.png
[pt_15]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/74_Python_install/15.png
[pt_16]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/74_Python_install/16.png
[pt_17]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/74_Python_install/17.png
[pt_18]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/74_Python_install/18.png
[pt_19]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/74_Python_install/19.png
[pt_20]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/74_Python_install/20.png
[pt_21]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/74_Python_install/21.png
[pt_22]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/74_Python_install/22.png
[pt_23]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/74_Python_install/23.png
[pt_24]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/74_Python_install/24.png
[pt_25]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/74_Python_install/25.png
[pt_26]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/74_Python_install/26.png
[pt_27]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/74_Python_install/27.png
[pt_28]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/74_Python_install/28.png
[pt_29]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/74_Python_install/29.png
[pt_30]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/74_Python_install/30.png
[pt_31]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/74_Python_install/31.png
[pt_32]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/74_Python_install/32.png


{% endraw %}
