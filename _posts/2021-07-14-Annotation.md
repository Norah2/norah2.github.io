---
layout: post
title: Markdown语法中的注释问题  
date: 2021-07-14
description: 记录了几种Markdown中用来注释文字的方法  
categories: Blog
tags: Blog Markdown
author: NMt
mathjax: true
---

* content
{:toc}

记录了几种Markdown中用来注释文字的方法。  

<div style='display: none'>
@@@@
</div>




{% raw %}

### 1、用html标签  

因为Markdown内嵌html语法，所以可以通过html标签来隐藏文字。  

```html
<div style='display: none'>
哈哈我是注释，不会在浏览器中显示。
我也是注释。
</div>
```

**注意：需要在前面空一行**

### 2、用html语法  

```html
<!--哈哈我是注释，不会在浏览器中显示。-->

<!--
哈哈我是多段
注释，
不会在浏览器中显示。
-->
```

这种注释方式是多行注释，用的时候容易出现问题，并且不能嵌套，所以对于小范围的注释最好还是用单行注释，或者html标签（因为html标签可嵌套）。  

### 3、hack方法  

hack方法就是利用markdown的解析原理来实现注释的。一般有的markdown解析器不支持上面的注释方法，这个时候就可以用hack方法。

hack方法比上面2种方法稳定得多，但是语义化太差。

```markdown
[comment]: <> (哈哈我是注释，不会在浏览器中显示。)
[comment]: <> (哈哈我是注释，不会在浏览器中显示。)
[comment]: <> (哈哈我是注释，不会在浏览器中显示。)
[//]: <> (哈哈我是注释，不会在浏览器中显示。)
[//]: # (哈哈我是注释，不会在浏览器中显示。)
其中，这种方法最稳定，适用性最强：

[//]: # (哈哈我是注释，不会在浏览器中显示。)
这种方法最可爱，超级无敌萌啊：

[^_^]: # (哈哈我是注释，不会在浏览器中显示。)
```

**注意：需要在前面空一行**

---

我在用hack方法注释的时候是用在自己的博客上的，但是出现了这种问题:  

![][pt_01]

经过多次更改与尝试发现是没有在使用语法的那一行前面空一行。

转载请注明：[南梦婷的博客](https://norah2.github.io) » [点击阅读原文](https://norah2.github.io/2021/07/14/Annotation/) 

<!--本文用到的链接-->

[pt_01]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/50_Annotation/01.png

{% endraw %}
