---
layout: post
title: 相似性和相异性的度量
date: 2023-03-01
description: 相似性和相异性等多种度量指标的定义及概念。
categories: ML
tags: ML
author: NMt
mathjax: true
---

* content
{:toc}

介绍了相似性度量以及相异性度量的相关概念。其中包含简单匹配系数、Jaccard系数、余弦相似度、Tanimoto系数、皮尔森相关系数等。

<div style='display: none'>
@@@@
</div>





{% raw %}
# 相似度和相异度简介  

**相似度（Similarity）：**  

* 两个数据对象相似程度的数值度量；  
* 对象越相似，值越高；  
* 通常在[0, 1]区间取值。  

有时候相似度的取值范围可能在[-1, 1]之间，这时正负号包含了一定信息，这种情况下可以保留其符号，而非强行转换到[0, 1]之间。  

**相异度（Dissimilarity）：**  

* 两个对象不同（相异）程度的数值度量；  
* 对象越相似，值越低；  
* 通常，最小相异度为0；  
* 上界不确定。  

对象越类似，他们的相异度就越低。距离常常用来表示特定类型的相异度。  

相异度可以在[0, 1]中取值，但也常常在$[0, \infty]$中取值。而将相异度的值映射到[0, 1]时往往会损失一些信息，甚至尺度会发生一些变化，这个过程往往需要结合算法以及具体场景来斟酌判断。  

**邻近度（Proximity）**：相似度和相异度的统称。   

相似度和相异度之间是可以相互转换的。可以用d=1-s来从相似度数据中得到相异度，这时相异度越接近1，代表数据间距离越远；相异度越接近0，代表数据间距离越近。还可以直接对相似度取负，一般来说，单调减函数都可以用来完成相似度和相异度间的转换。  

# 数据对象之间的相异度  

距离常常用于度量数据对象的相异度。下面介绍一些常用的距离公式。  

**欧几里得距离 ( Euclidean distance) d**如下公式定义：  

$$
d(x, y)=\sqrt{\sum_{k=1}^{n}(x_k-y_k)^2}
$$

其中n是维度，而${x_k}$ 和 ${y_k}$分别是x和y的第k个属性（分量）：  

![][pt_01]  

**闵可夫斯基距离 ( Minkowski distance )**:  

$$
d(x, y)=(\sum_{k=1}^{n}{|x_k-y_k|}^r)^{1/r}
$$

Minkowski距离是欧氏距离的推广，其中r是参数：  

* r=1时成为街区距离（或曼哈顿距离，$L_1$范数）；  
* r=2时为Euclidean距离（或$L_2$范数）；  
* r$\to\infty$时成为切比雪夫距离（或$L_{max}$范数，$L_{\infty}$ 范数）：

$$
d(x, y)=\lim_{r\to\infty}(\sum_{k=1}^n{|x_k-y_k|}^r)^{1/r}=\max_k(|x_k-y_k|)
$$

**Mahalanobis距离（马氏距离）** 可以看作是欧氏距离的一种修正，修正了欧式距离中各个维度尺度不一致且相关的问题。**某些属性之间相关就使用Mahalanobis距离。**计算Mahalanobis距离的代价较大，但对于属性相关的对象来说是值得的，如果属性相对来说不相关，只是具有不同的值域，则只需要对变量进行标准化就足够了。  

Mahalanobis距离公式定义如下：  

$$
mahalanobis(x, y)=(x-y)^{T}\Sigma^{-1}{(x-y)}
$$

* 其中，$\Sigma^{-1}$是数据协方差矩阵的逆；  
* 协方差矩阵$\Sigma$是这样的矩阵，它的第i行第j列元素是第i个和第j个属性的协方差。  

距离具有一些常见性质。如果$d(x, y)$是两个点x和y之间的距离，则如下性质成立：  

1. **非负性**  
   * 对于所有x和y，$d(x, y)\geq{0}$;  
   * 仅当x=y时，$d(x, y)=0$;  
2. **对称性**  
   * 对于所有x和y，$d(x, y)=d(y, x)$;  
3. **三角不等式**  
   * 对于是所有x，y和z，$d(x, z)\leq{d(x, y)+d(y, z)}$  

满足以上三个性质的测度成为**度量 ( metric )**。

**非度量的相异度：集合差**  基于集合论中定义的两个集合差的概念举例。设有两个集合A和B，A-B是不在B中的 A 中元素的集合。例如，如果 A={1, 2, 3, 4}, 而 B={2, 3, 4},则A-B={1}，而 $B-A=\emptyset$，即空集。我们可以将两个集合A和B之间的距离定义为 d(A, B)=size(A - B)，其中 size 是一个函数，它返回集合元素的个数。该距离测度是大于或等于零的整数值，但不满足非负性的第二部分，也不满足对称性，同时还不满足三角不等式。然而，如果将相异度修改为 d(A, B)= size(A - B) + size(B-A)，则这些性质都可以成立。  

**非度量的相异度：时间**  这里给出一个更常见的例子，其中相异性测度并非度量，但依然是有用的。定义时间之间的距离测度如下:  

$$
d(t_1, t_2)= \left\{\begin{array}{ll}
t_2-t_1 & \textrm{if $t_1 \leq{t_2}$}\\
24+(t_2-t_1) & \textrm{if $t_1\geq{t_2}$}\\
\end{array} \right.
$$

例如，d(1PM,2PM)=1小时，而d(2PM,1PM)= 23 小时。这种定义是有意义的，例如，在回答如下问题时就体现了这种定义的意义:“如果一个事件在每天下午 1 点发生，现在是下午 2点，那么我们还需要等待多长时间才能等到该事件再度发生?”  

# 数据对象之间的相似度  

对于相似度，三角不等式（或类似的性质）通常不成立，但是对称性和非负性通常成立。如通$s(x, y)$时数据点x和y之间的相似度，则相似度具有如下典型性质：  

1. 仅当x=y时，$s(x, y)=1 (0 \leq{s}\leq{1})$  
2. 对于所有x和y，$s(x, y)=s(y, x)$ (对称性)  

对于相似度，没有与三角不等式对应的一般性质。然而，有时可以将相似度简单的变换成一种度量距离。  

考虑一个实验，实验中要求人们对屏幕上快速闪过的一小组字符进行分类。该实验的**混淆矩阵 (confusion matrix)**记录每个字符被分类为自己的次数和被分类为另一个字符的次数。例如，假定“0”出现了 200 次，它被分类为“0”160 次，而被分类为“o”40次。类似地，“o”出现 200 次并且分类为“o”170 次，但是分类为“0”只有 30 次。如果取这些计数作为两个字符之间相似性的度量，则得到一种相似性度量，但这种相似性度量不是对称的。在这种情况下，通过选取 $s'(x, y)=s'(y,x)=(s(x, y)+ s(y,x))/2$，相似性度量可以转换成对称的，其中s'是新的相似性度量。  

# 邻近性度量  

## 二元数据的相似性度量  

两个仅包含二元属性的对象之间的相似性度量也称为**相似系数 ( similarity coefficient )**, 并且通常在0和1之间取值，1表示两个对象完全相似，0表明对象完全不相似。  

设x和y是两个对象，都由n个二元属性组成，这样的两个对象（即两个二元向量）的比较可生成以下四个量（频率）：  

* $f_{00}$：x取0并且y取0的属性个数；  
* $f_{01}$：x取0并且y取1的属性个数；  
* $f_{10}$：x取1并且y取0的属性个数；  
* $f_{11}$：x取1并且y取1的属性个数。  

**简单匹配系数 ( Simple Matching Coefficient, SMC )**: 一种常用的相似性系数是简单匹配系数，定义如下：  

$$
SMC=\frac{值匹配的属性个数}{属性个数}=\frac{f_{11}+f_{00}}{f_{01}+f_{10}+f_{11}+f_{00}}
$$

简单匹配系数适合作为仅包含二元属性（属性仅有两个取值，例如0和1）的数据之间的相似性度量。

**Jaccard系数 (Jaccard Coefficient)** 用来处理仅包含非对称的二元属性的对象，由如下等式定义：  

$$
J=\frac{匹配的个数}{不涉及0-0匹配的属性个数}=\frac{f_{11}}{f_{01}+f_{10}+f_{11}}
$$

![][pt_02]  


## 余弦相似度  

设x和y时两个向量，则：

$$
cos(x, y)=\frac{x·y}{\|x\|\|y\|}
$$

**"·"表示向量点积：**  

$$
x·y=\sum_{k=1}^{n}x_ky_k
$$

$\|x\|$是向量x的长度

$$
\|x\|=\sqrt{\sum_{k=1}^n{x_k^2}}=\sqrt{x·x}
$$

$$
cos(x, y)=\frac{x·y}{\|x\|\|y\|}=x'·y'
$$

其中，$x'=x/\|x\|, y'=y/\|y\|$是长度为1的向量。  

![][pt_03]  

例：  

![][pt_04]  

## 广义Jaccard系数  

广义Jaccard系数可以用于文档数据，并在二元属性情况下归约为Jaccard系数。广义Jaccard系数又称为Tanimoto系数，该系数用EJ表示，由下式定义：  

$$
EJ(x, y)=\frac{x·y}{{\|x\|}^2+{\|y\|}^2-x·y}
$$

## 相关性  

两个具有二元变量或连续变量的数据对象之间的相关性是对象属性之间线性联系的度量。两个数据对象x和y之间的**皮尔森相关 (Pearson's correlation)** 系数由下式定义：

$$
corr(x, y)=\frac{covariance(x, y)}{standard\_deviation(x)×standard\_deviation(y)}=\frac{S_{xy}}{S_xS_y}
$$

其中，统计学记号和定义：

协方差:   

$$
covariance(x, y)=S_{xy}=\frac{1}{n-1}\sum_{k=1}^{n}{(x_k-\bar{x})(y_k-\bar{y})}
$$

标准差:  

$$
standard\_deviation(x)=S_x=\sqrt{\frac{1}{n-1}\sum_{k=1}^{n}{(x_k-\bar{x})^2}}
$$
$$
standard\_deviation(y)=S_y=\sqrt{\frac{1}{n-1}\sum_{k=1}^{n}{(y_k-\bar{y})^2}}
$$

均值:  

$$
\bar{x}=\frac{1}{n}\sum_{k=1}^{n}{x_k}\\
\\
\bar{y}=\frac{1}{n}\sum_{k=1}^{n}{y_k}
$$

$-1 \leq corr(x, y) \leq 1$， $corr(x, y)=0$ 不相关，$corr(x, y)=1(-1)$ 正（负）相关。  

相关系数即将x、y坐标向量各自平移到原点后的夹角余弦。这即解释了为何文档间求距离使用夹角余弦——因为这一物理量表征了文档去均值化后的随机向量间相关系数。  

### 相关性的可视化  

![][pt_05]  

# 相似度/距离计算方法总结  

* 闵可夫斯基距离Minkowski/欧氏距离：

$$
dist(X, Y)=(\sum_{i=1}^{n}{|x_i-y_i|}^p)^{\frac{1}{p}}
$$

* 杰卡德相似系数 (Jaccard)：

$$
J(A, B)=\frac{|A\cap{B}|}{|A\cup{B}|}
$$

* 余弦相似度 (cosine similarity)：

$$
cos(\theta)=\frac{a^Tb}{|a|·|b|}
$$

* Pearson相似系数：
$$
\rho_{XY}=\frac{cov(X, Y)}{\sigma_X \sigma_Y}=\frac{E[(X-\mu_X) (Y-\mu_Y)]}{\sigma_X \sigma_Y}=\frac{\sum_{i=1}^{n}{(X_i-\mu_X)(Y_i-\mu_Y)}}{\sqrt{\sum_{i=1}^{n}{(X_i-\mu_X)^2}}\sqrt{\sum_{i=1}^{n}{(Y_i-\mu_Y)^2}}}
$$

* 相对熵(K-L距离)：

$$
D(p\|q)=\sum_{x}{p(x)log{\frac{p(x)}{q(x)}}}=E_{p(x)}log{\frac{p(x)}{q(x)}}
$$

* Hellinger距离：

$$
D_{\alpha}(p\|q)=\frac{2}{1-\alpha^2}(1-\int{p(x)^{\frac{1+\alpha}{2}}q(x)^{\frac{1-\alpha}{2}}dx})
$$


转载请注明：[南梦婷的博客](https://norah2.github.io) » [点击阅读原文](https://norah2.github.io/2021/03/01/Similarity/) 

<!--本文用到的链接-->

[pt_01]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/78_Similarity/01.png  
[pt_02]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/78_Similarity/02.png  
[pt_03]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/78_Similarity/03.png  
[pt_04]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/78_Similarity/04.png  
[pt_05]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/78_Similarity/05.png  

{% endraw %}
