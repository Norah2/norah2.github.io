---
layout: post
title: np.clip()函数的参数详解  
date: 2019-8-16
description: 函数参数详解  
categories: Python
tag: Python Func
author: NMt
mathjax: true
---

* content
{:toc}

详述一下clip函数的用法，最近在看代码的时候遇到的这个函数。  

<div style='display: none'>
@@@@
</div>





{% raw %}
函数格式：  

```python
numpy.clip(a, a_min, a_max, out=None)
```

其中a为数组，a_min, a_max分别为最大值最小值。输出的数组大小和a一样，但是数组中小于a_min的值被改为a_min, 大于a_max的值被改为a_max。  

看个简单的例子就明白了，eg：  

```python
import numpy as np 

a = np.array(range(10)).reshape(2, 5)
print(a)
np.clip(a, 3, 7, out=a)
print(a)

[out]:  
[[0 1 2 3 4]
 [5 6 7 8 9]]

[[3 3 3 3 4]
 [5 6 7 7 7]]
```

转载请注明：[南梦婷的博客](https://norah2.github.io) » [点击阅读原文](https://norah2.github.io/2019/08/16/np_clip_func/)   

<!--以下是本文用到的链接-->  

{% endraw %}
