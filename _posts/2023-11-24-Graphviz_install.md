---
layout: post
title: 可视化数据库结构
date: 2023-11-24
description: 使用`Django-extensions`以及Graphviz工具生成数据库结构图。  
categories: Pyhton
tags: Install
author: NMt
mathjax: true
---

* content
{:toc}

使用`Django-extensions`以及Graphviz工具生成数据库结构图。  

<div style='display: none'>
@@@@
</div>





{% raw %}
最近在做一个实时聊天系统，里面涉及到了数据库的内容，因为数据库内容学的不够扎实，所以对于数据库部分的理解比较有限，就借助了第三方工具的使用。这篇文章主要记载了一下如何生成数据库结构图。  

### 1. 安装`Django-extensions`:  

```bash
pip install django-extensions
```

**注：django-extensions的版本要和Django的版本互相兼容。**  

### 2. 更改配置文件  

在 Django 项目的 settings.py 中添加 `django_extensions` 到 `INSTALLED_APPS`:  

```python
INSTALLED_APPS = [
    # ... 其他已安装的应用 ...
    'django_extensions',
]
```

### 3. 安装 Graphviz:  

`django-extensions` 使用 Graphviz 来生成图形。需要在系统中安装 Graphviz。这可以通过包管理器或从 [Graphviz 官网][link_01] 下载安装。  

安装的时候需要将Graphviz软件的bin目录加入到环境变量中。(添加到Path路径下即可)  

如果安装好了，在cmd下输入命令：`dot -V` 会显示版本信息：  

![][pt_01]  

### 4. 安装依赖包：  

```bash
pip install pydotplus
pip install pygraphviz
```

### 5. 生成图形:  

在 Django 项目目录下运行以下命令：

```bash
python manage.py graph_models -a -o my_project_model_graph.png
```

这将为项目中的所有应用生成一个模型关系图，并将其保存为 `my_project_model_graph.png`。

![][pt_02]  

### 报错汇总  

* 缺少 `pydotplus` 以及`pygraphviz` 两个依赖包：  

	```bash
	CommandError: Neither pygraphviz nor pydotplus could be found to generate the image. To generate text output, use the --json or --dot options.
	```

	解决方案：  

	```bash
	pip install pydotplus
	pip install pygraphviz
	```


* 安装过程中缺少了 `graphviz/cgraph.h` 文件:  

	```bash
	  note: This error originates from a subprocess, and is likely not a problem with pip.
	  ERROR: Failed building wheel for pygraphviz
	  Running setup.py clean for pygraphviz
	Failed to build pygraphviz
	ERROR: Could not build wheels for pygraphviz, which is required to install pyproject.toml-based projects
	```

	这通常意味着 Graphviz 库没有被正确安装或者没有被添加到系统路径中。并且需要注意的是，若是正确加入环境变量之后仍旧出现这种问题，说明设置并没有生效，可以重启命令行工具。  

*  `Django-extensions` 的版本与 Django 版本不兼容  

	```bash
	AttributeError: module 'django.db.models.deletion' has no attribute 'RESTRICT'
	```

	解决方案：需要检查 Django 和 `Django-extensions` 的版本，并且通过查看官方文档来确定可以兼容的版本。  

	```bash
	python -m django --version
	pip show django-extensions
	```


[django-extensions官方文档][link_02]: https://pypi.org/project/django-extensions/#history  
[Django官方文档][link_03]: https://www.djangoproject.com/download/  
[channels官方文档][link_04]: https://channels.readthedocs.io/en/latest/index.html  


转载请注明：[南梦婷的博客](https://norah2.github.io) » [点击阅读原文](https://norah2.github.io/2023/11/24/Graphviz_install/) 

<!--本文用到的链接-->

[pt_01]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/82_Graphviz_install/01.png  
[pt_02]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/82_Graphviz_install/02.png  

[link_01]: https://graphviz.org/download/  
[link_02]: https://pypi.org/project/django-extensions/#history  
[link_03]: https://www.djangoproject.com/download/  
[link_04]: https://channels.readthedocs.io/en/latest/index.html  

{% endraw %}
