---
layout: post
title: coo_matrix()函数的参数详解  
date: 2019-08-16
description: 函数参数详解  
categories: Python
tags: Python Func
author: NMt
mathjax: true
---

* content
{:toc}

coo_matrix()函数的参数详解

<div style='display: none'>
@@@@
</div>





{% raw %}

`coo_matrix()`方法是scipy库中sparse模块中的函数。  

`coo_matrix((coo_data, (coo_rows, coo_cols)), shape=shape)` 其中coo_cols参数不能大于参数shape的列。  

row序列是行索引，col序列是列索引，相对应位置的row、col能组成一个坐标，data中则是与row、col相同位置的值，相同位置的值会相加，最后得到的矩阵是在row，col位置上存着data的值，其他位置为0。  

而`.roscr()`函数则是一个记录稀疏矩阵的函数，能够更节约内存的存储稀疏矩阵。  

下面的例子可以看出`coo_matrix()`方法与`.toscr()`方法的效果：  

```python
from numpy import array
from scipy.sparse import coo_matrix
row  = array([0, 0, 1, 3, 1, 0, 0])
col  = array([0, 2, 1, 3, 1, 0, 0])
data = array([1, 1, 1, 1, 1, 1, 1])
A = coo_matrix((data, (row, col)), shape=(4, 4)).tocsr()
A.toarray()

[out]:  
array([[3, 0, 1, 0],
       [0, 2, 0, 0],
       [0, 0, 0, 0],
       [0, 0, 0, 1]], dtype=int32)
```


转载请注明：[南梦婷的博客](https://norah2.github.io) » [点击阅读原文](https://norah2.github.io/2019/08/16/coo_matrix_func/)   

<!--以下是本文用到的链接-->  

{% endraw %}
