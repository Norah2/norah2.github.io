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


转载请注明：[南梦婷的博客](https://norah2.github.io) » [点击阅读原文](https://norah2.github.io/2021/07/20/TableCenter/) 

<!--本文用到的链接-->

{% endraw %}
