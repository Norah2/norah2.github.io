---
layout: post
title: Markdown表格居中
date: 2021-07-20
description: 用markdown语法进行表格居中
categories: Blog
tags: Blog Markdown CSS
author: NMt
mathjax: true
---

* content
{:toc}

用markdown语法进行表格居中。

<div style='display: none'>
@@@@
</div>





{% raw %}
在写博客的时候发现这个博客模板中的表格不是自动居中的，所以搜了一些方法进行表格居中，目前有效的是这样的一个：  

```css
<style>
table {
margin: auto;
}
</style>
```

把这一整段直接插入到markdown文件中就可以让这一整篇的表格居中，肯定在一些设置文件中能够一劳永逸的更改，我先研究一下再回来更。  

----

在我自己的博客项目里首先找到页面的总体配置文档，再找到相应调整格式的配置文件夹，在文件夹下找到关于table表格的格式设置，把margin这一项改为auto就可以了。(注释的是原来的配置，把margin属性的值新修改为auto)  

```css
table {
    border-top: 2px solid #777;
    border-bottom: 2px solid #777;
//    margin: 8px 0;
    margin: auto;
    border-collapse: collapse;
    thead {
        border-bottom: 1px solid #777;
        background-color: #aaa;
        color: #fff;
    }
	...
}
```

其实图片也没有默认居中，算了不调了，图片不居中看起来还行。  

记录一下我的修改表格和图片的设置在`_sass`文件下的`_reset.scss`文件中。


转载请注明：[南梦婷的博客](https://norah2.github.io) » [点击阅读原文](https://norah2.github.io/2021/07/21/TableCenter/) 

<!--本文用到的链接-->

{% endraw %}
