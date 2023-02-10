---
layout: post
title: cv2.imread()函数的参数详解  
date: 2019-08-15
description: 函数参数详解  
categories: Python
tag: Python Func
author: NMt
mathjax: true
---

* content
{:toc}

cv2.imread()函数的参数详解  

<div style='display: none'>
@@@@
</div>





{% raw %}

在cv2中`imread()`方法读取图像，此方法的原型是：  

```python
imaread(const string& filename, int flag=1)
```

filename指图像名称，flag指读取图像颜色类型：  

>flag=-1, 8位深度，原通道  
>flag=0, 8位深度，1通道  
>flag=1, 8位深度，3通道  
>flag=2, 原深度，1通道  
>flag=3, 原深度，3通道  
>flag=4, 8位深度，3通道  



转载请注明：[南梦婷的博客](https://norah2.github.io) » [点击阅读原文](https://norah2.github.io/2019/08/15/imread_func/)   

<!--以下是本文用到的链接-->  

{% endraw %}
