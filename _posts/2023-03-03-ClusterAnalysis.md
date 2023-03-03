---
layout: post
title: 聚类分析
date: 2023-03-02
description: 介绍聚类的常见算法
categories: ML
tags: ML Model
author: NMt
mathjax: true
---

* content
{:toc}

介绍了聚类分析的基本概念、K-means聚类、二分K均值聚类、层次聚类、DBSCAN聚类、簇评估等内容。在必要处对各个算法的算法步骤、公式、优缺点、简单示例、时间复杂性、空间复杂性做了描述。其中还归纳了聚类类型、簇类型、相似性度量方法、邻近性矩阵、SSR、凝聚度和分离度、轮廓系数等内容做了简述。  

<div style='display: none'>
@@@@
</div>





{% raw %}

# 聚类分析基本概念  

## 聚类分析定义  

聚类分析是一种数据分析技术，对大量未知标注的数据集，通过将具有相似数据特性的数据对象分组到一起，使得类别内的数据相似度较大而类别间的数据相似度较小，以便对这些数据对象进行更好的理解和分析。总的来说，聚类分析就是将数据划分成有意义或有用的组（簇）。  

**注：聚类分析是无监督学习。**  

## 聚类类型  

* 划分聚类（Partitional Clustering）  
* 层次聚类（Hierarchical Clustering）  
* 互斥聚类（exclusive clustering）  
* 非互斥（重叠）聚类（non-exclusive）  
* 模糊聚类（fuzzy clustering）  
* 完全聚类（complete clustering）  
* 部分聚类（partial clustering）  

### 划分聚类（Partitional Clustering）  

划分聚类简单地将数据对象集划分成不重叠的子集，使得每个数据对象恰在一个子集。

![][pt_01]  

### 层次聚类（Hierarchical Clustering）  

层次聚类是嵌套簇的集族，组织成一棵树。  

![][pt_02]  

### 互斥的、重叠的、模糊的  

* 互斥的（Exclusive）  
   * 每个对象都指派到单个簇  
* 重叠的（overlapping）或非互斥的（non-exclusive）  
   * 聚类用来反映一个对象。同时属于多个组（类）这一事实  
   * 例如：在大学里，一个人可能既是学生，又是雇员  
* 模糊聚类（Fuzzy clustering ）  
   * 每个对象以一个0（绝对不属于）和1（绝对属于）之间的隶属权值属于每个簇。换言之，簇被视为模糊集  
* 部分的（Partial）  
   * 部分聚类中数据集某些对象可能不属于明确定义的组。如：一些对象可能是离群点、噪声  
* 完全的（complete）  
   * 完全聚类将每个对象指派到一个簇  

## 簇类型  

* 明显分离的  
* 基于原型的  
* 基于图的  
* 基于密度的  
* 概念簇  

### 明显分离的（Well-Separated）  

每个点到同簇中任一点的距离比到不同簇中所有点的距离更近。  

![][pt_03]  

### 基于原型的  

* 每个对象到定义该簇的原型的距离比到其他簇的原型的距离更近。对于具有连续属性的数据，簇的原型通常是质心，即簇中所有点的平均值。当质心没有意义时，原型通常是中心点，即簇中最有代表性的点。  

* 基于中心的（ Center-Based）的簇：每个点到其簇中心的距离比到任何其他簇中心的距离更近。  

![][pt_04]  

### 基于图的  

* 如果数据用图表示，其中节点是对象，而边代表对象之间的联系。  
* 簇可以定义为连通分支（connected component）：互相连通但不与组外对象连通的对象组。  
* 基于近邻的（ Contiguity-Based）：其中两个对象是相连的，仅当它们的距离在指定的范围内。这意味着，每个对象到该簇某个对象的距离比到不同簇中任意点的距离更近。  
![][pt_05]  

### 基于密度的（Density-Based）  

簇是对象的稠密区域，被低密度的区域环绕。  

![][pt_06]  

### 概念簇（Conceptual Clusters）  

可以把簇定义为有某种共同性质的对象的集合。例如：基于中心的聚类。还有一些簇的共同性质需要更复杂的算法才能识别出来。  

![][pt_07]  

# K-Means聚类  

K-Means算法，被成为k-平均或k-均值，是一种广泛使用的聚类算法，或者成为其他聚类算法的基础。  

## K-means算法步骤  

假定输入样本为$S=x_1, x_2, \dots, x_m$, 则算法步骤为：  

1. 选择初始的k个类别中心$\mu_1, \mu_2, \dots, \mu_k$  
2. 对于每个样本$x_i$, 将其标记为距离类别中心最近的类别，即$label_i=\argmin_{1\leq{j}\leq{k}}\|x_i-\mu_{j}\|$  
3. 将每个类别中心更新为隶属该类别的所有样本的均值：$\mu_j=\frac{1}{\|c_j\|}\sum_{i\in{c}_j}{x_j}$  
4. 重复最后两步，直到类别中心的变化小于某阈值。  
5. 中止条件：迭代次数/簇中心变化率/最小平方误差MSE(Minimum Squared Error)  

## K-Means过程：  

![][pt_08]  

## K-means的公式化解释：  

* 记K个簇中心为$\mu_1, \mu_2, \dots, \mu_k$, 每个簇的样本数目为 $N_1, N_2, \dots, N_k$  
* 使用平方误差作为目标函数：$J(\mu_1, \mu_2, \dots, \mu_k)=\frac{1}{2}\sum_{j=1}^{K}\sum_{i=1}^{N_j}(x_i-\mu_j)^2$  
* 该函数为关于$\mu_1, \mu_2, \dots, \mu_k$的凸函数，其驻点为：$\frac{\partial{J}}{\partial{\mu_j}}=-2\sum_{i=1}^{N_j}{(x_i-\mu_j)}\to0\Longrightarrow\mu_j=\frac{1}{N_j}\sum_{i=1}^{N_j}{x_i}$  

## K-Means是初值敏感的  

![][pt_09]  

选择适当的初始质心是基本K均值过程的关键步骤。常见的方法是随机地选取初始质心，但是簇的质量常常很差。  

随机地选取初始质心可能很糟糕。第一张图和第二张图使用相同的数据集。两张图显示了两种选定的初始质心获得的。(对于这两个图，各次迭代的簇质心位置由“+”指出。) 第一张图中尽管所有的初始质心都在自然簇中但是仍然找到了最小 SSE 聚类。而在第二张图中，尽管初始质心的分布看上去较好，但是仅得到了一个次最优聚类，具有较高的平方误差。  

![][pt_10]  

![][pt_11]  

**随机初始化的局限** 处理选取初始质心问题的一种常用技术是: 多次运行，每次使用一组不同的随机初始质心，然后选取具有最小SSE的簇集。该策略虽然简单，但是效果可能不好，这取决于数据集和寻找的簇的个数。  

使用下图所示的数据集进行解释。该数据由两个族对组成，其中，每个簇对(上、下)中的簇更靠近，而离另一对中的簇较远。

如果对每个簇对用两个初始质心，则即使两个质心在一个簇中，质心也会自己重新分布，从而找到“真正的”簇。如果一个簇对只用一个初始质心，而另一对有三个，这将会造成两个真正的簇将合并、一个真正的簇被分裂的结果。  

![][pt_12]  

![][pt_13]  

注意: 只要两个初始质心落在簇对的任何位置，就能得到最优聚类，因为质心将自己重新分布，每个簇一个。不幸的是，随着簇的个数增加，至少一个簇对只有一个初始质心的可能性也逐步增大。在这种情况下，由于对相距较远，K 均值算法不能在对之间重新分布质心，这样就只能得到局部最优。  

随机选择初始质心存在的问题即使重复运行多次也不能克服，因此常常使用其他技术进行初始化。一种有效的方法是，取一个样本，并使用层次聚类技术对它聚类。从层次聚类中提取 K个簇，并用这些簇的质心作为初始质心。该方法通常很有效，但仅对下列情况有效:(1)样本相对较小，例如数百到千(层次聚开销较);(2)K相对于样本大小较小。  

下面的过程是另一种选择初始质心的方法。随机地选择第一个点，或取所有点的质心作为第1个点。然后，对于每个后继初始质心，选择离已经选取过的初始质心最远的点。使用这种办法我们得到初始质心的集合，确保不仅是随机的，而且是散开的。然而，这种方法可能选中离群点。而不是稠密区域(簇)中的点。此外，求离当前初始质心集最远的点开销也非常大。为了克服这些问题，通常将该方法用于点样本。由于离群点很少，它们多半不会在随机样本中出现。相比之下，除非样本非常小，否则来自稠密区域中的点很可能包含在样本中。此外，找出初始质心所需要的计算量也大幅度减少，因为样本的大小通常远小于点的个数。  

## 时间复杂性和空间复杂性  

K 均值的空间需求是适度的，因为只需要存放数据点和质心。具体地说，所需要的存储量为$O((m + K)n)$，其中 $m$ 是点数，$n$ 是属性数。K 均值的时间需求也是适度的——基本上与数据点个数线性相关。具体地说，所需要的时间为 $O(l×K×m×n)$，其中l是收所需要的选代次数。如前所述，l 通常很小，可以是有界的，因为大部分变化通常出现在前几次迭代。因此，只要簇个数 $K$ 显著小于 $m$，则K 均值的计算时间与 $m$ 线性相关，并且是有效的和简单的。  

## 二分K均值  

二分K均值算法是基本K均值算法的直接扩充，它基于一种简单想法: 为了得到K个簇将所有点的集合分裂成两个簇，从这些簇中选取一个继续分裂，如此下去，直到产生K个簇。  

**二分K均值算法：**  

1. 初始化簇表，使之包含由所有的点组成的簇；  
2. **repeat**  
3. $\quad$从簇表中取出一个簇；  
4. $\quad${对选定的簇进行多次二分“试验”}；  
5. $\quad$**for** i = 1 to 试验次数 **do**  
6. $\quad\quad$使用基本K均值，二分选定的簇；  
7. $\quad$**end for**  
8. $\quad$从二分试验中选择具有最小总SSE的两个簇；  
9. $\quad$将这两个簇添加到簇表中；  
10. **until** 簇表中包含K个簇。  

待分裂的簇有许多不同的选择方法。可以选择最大的簇，选择具有最大 SSE 的簇，或者使用一个基于大小和 SSE 的标准进行选择。不同的选择导致不同的簇。  

通常使用结果簇的质心作为基本 K 均值的初始质心，对结果簇逐步求精。这是必要的因为尽管 K 均值算法可以确保找到使 SSE 局部小的聚类但是在二分 K 值算法中，我们“局部地”使用了 K 均值算法，即二分个体簇。因此，最终的簇集并不代表使 SSE 局部最小的聚类。  

## K均值和不同的簇类型  

对于发现不同的簇类型，K均值和它的变种都具有一些局限性。具体地说，当簇具有非球形形状或具有不同尺寸或密度时，K均值很难检测到“自然的”簇。  

### 具有不同尺寸的簇  

在下图中，K均值不能发现那三个自然簇，因为其中一个簇比其他两个大得多，因此较大的簇被分开，而一个较小的簇与较大簇的一部分合并到一起。  

![][pt_14]  

### 具有不同密度的簇  

在下图中，K均值未能发现那三个自然簇，因为两个较小的簇比较大的簇稠密得多。  

![][pt_15]  

### 非球形的簇  

在下图中，K均值发现了两个簇(两个自然簇的混合体)，因为两个自然簇的形状不是球形的。  

![][pt_16]  

这三种情况的问题在于 K 均值的目标函数与我们试图发现的簇的类型不匹配，因为 K均值目标函数是最小化等尺寸和等密度的球形簇，或者明显分离的簇。  

## 优点和缺点  

**优点：**  

* 算法简单  
* 适合球形簇  
* 二分k均值等变种算法运行良好，不受初始化问题的影响  

**缺点：**  

* 不能处理非球形簇、不同尺寸和不同密度的簇  
* 对离群点、噪声敏感  

**对于非数值型数据或混合类型数据的替代方法：K-modes，K-medoids，K-prototypes**  

# 层次聚类  

* 层次聚类按数据分层建立簇，形成一棵以簇为节点的树，成为聚类图；  
* 按自底向上层次分解，则称为**凝聚的**层次聚类；  
* 按自顶向下层次分解，就称为**分裂的**层次聚类。  

![][pt_17]  

凝聚的层次聚类采用自底向上的策略，开始时把每个对象作为一个单独的簇，然后逐次对各个簇进行适当合并，直到满足某个终止条件。  

分裂的层次聚类采用自顶向下的策略，与凝聚的层次聚类相反，开始时将所有对象置于同一个簇中，然后逐次将簇分裂为更小的簇，直到满足某个终止条件。  
传统的算法利用相似性或相异性的邻近度矩阵进行凝聚的或分裂的层次聚类。  

![][pt_18]  

**基本凝聚层次聚类算法:**  

1. 计算邻近度矩阵；  
2. 让每个点作为一个cluster；  
3. **Repeat**  
4. $\quad$合并最近的两个类；  
5. $\quad$更新邻近度矩阵，以反映新的簇与原来的簇之间的邻近性；  
6. **Utile**仅剩下一个簇。  

关键的操作是两个簇的林进度计算：不同的邻近度的定义区分了各种不同的凝聚层次技术。  

## cluster间的相似性度量  
* MIN  
* MAX  
* Group Average  
* Distance Between Centroids  
* Other methods driven by an objective function: Ward's Method利用平方误差增量  

### MIN or 单链  

单链意义下两个簇的相似性定义为：这两个簇中任意两点之间距离的最短值，即由一对最近邻点决定。  

![][pt_19]  

![][pt_20]  

**MIN (单链法) 在层次聚类中的应用：**  

![][pt_21]  

**单链的优势：** 单链技术可以处理非椭圆形状的簇。  

![][pt_22]  

**单链的局限性：** 对噪音和离群点很敏感。  

![][pt_23]

### MAX or 全链  

全链意义下两个簇的相似性定义为：这两个簇中任意两点之间距离的最大值。  

![][pt_24]  

**MAX (全链法) 在层次聚类中的应用：**  

![][pt_25]  

**全链的优势：** 对噪音和离群点不敏感。  

![][pt_26]  

**全链的局限性：** 可能使大的簇破裂；偏好球形簇。  

![][pt_27]  

### 组平均  

两个簇的邻近度定义为不同的所有点对的平均逐对邻近度，是一种单链与全链的折中算法。  
$$
proximity(Cluster_i, Cluster_j)=\frac{\sum_{p_i\in{Cluster_i} \atop p_j\in{Cluster_j}}{proximity(p_i, p_j)}}{\|Cluster_i\|*\|Cluster_j\|}
$$

![][pt_28]  

**组平均法在层次聚类中的应用：**  

![][pt_29]  

**组平均的优势：** 对噪音和极端值影响小。  

**组平均的局限性：** 偏好球形簇。  

### Ward’s Method

* 两个簇的邻近度定义为两个簇合并时导致的平方误差增量  
   * 当邻近度取它们之间的平方时，ward与组平均类似  
   * 对噪音和极端值影响小  
* 偏好球型簇  

**优点：**  

* 某些应用领域需要层次结构；  
* 有些研究表明，这种算法能够产生较高质量的聚类；  
* 简单，易于理解。  

**缺点：**  

* 计算量、存储量大；  
* 对噪声、高维数据敏感；  
* 一旦一组对象被合并，不能撤销，类之间成员不能交换。

### 四种相似性度量方式的比较  

![][pt_30]  

## 时间和空间复杂性  

基本凝聚层次聚类算法使用邻近度矩阵。这需要存储 $m^2/2$个邻近度 (假定邻近度矩阵是对称的)，其中 $m$ 是数据点的个数。记录簇所需要的空间正比于簇的个数为 $m-1$，不包括单点簇因此总的空间复杂度为 $O(m^2)$。  

基本凝聚层次聚类算法的计算复杂度分析也是很明确的，即需要$O(m^2)$时间计算邻近度矩阵。之后，步骤4 和5（步骤4：合并最接近的两个簇；步骤5：更新邻近性矩阵）涉及$m-1$ 次迭代，因为开始有m个簇，而每次选代合并两个簇。如果邻近度矩阵采用线性搜索，则对于第i次迭代，步骤 4 需要$O((m-i+1)^2)$时间，这正比于当前个数的平方。步骤 5只需要$O(m-i+1)$时间，在合并两个簇后更新邻近度矩阵。(对于我们考虑的技术，簇合并只影响$O(m-i+1)$个邻近度。）不作修改，时间复杂度将为$O(m^3)$。如果某个簇到其他所有簇的距离存放在一个有序表或堆中，则查找两个最近簇的开销可能降低到$O(m-i+1)$。然而，由于维护有序表或堆的附加开销，基于基本凝聚层次聚类算法的层次聚类所需要的总时间为$O(m^2\log{m})$。  

# DBSCAN  

DBSCAN (Density-Based Spatial Clustering of Applications with Noise) 将具有足够高密度的区域划分为簇，并可以发现任何形状的聚类。因此DBSCAN是一个典型的基于密度的聚类算法（其他聚类方法大都是基于对象之间的距离进行聚类，聚类结果是球状的簇）。基于密度的聚类是寻找被低密度区域分离的高密度区域。  

## 对于点的分类  

* 稠密区域内部的点（核心点）：在扫描半径eps内含有超过最小包含点数minPts数目的点。  
* 稠密区域边缘的点（边界点）：在扫描半径eps内点的数量小于最小包含点数minPts，但是落在核心点的邻域内，也就是说该点不是核心点，但是与其他核心点的距离小于eps。  
* 稠密区域中的点（噪声或背景点）：既不是核心点也不是边界点的点，该类点的周围数据点非常少。  

例如，设MinPts=4：  

![][pt_31]  

DBSCAN: 核心点、边界点和噪音点  

![][pt_32]  

![][pt_33]  

![][pt_34]  

## DBSCAN相关名词概念  

* Eps邻域：给定对象半径Eps内的邻域称为该对象的Eps邻域，用$N_{Eps}(p)$标识点p的Eps-半径内的点的集合，即：  

$$
N_{Eps}(p)=\{q|q在数据集D中，distance(p, q)\leq {Eps}\}
$$

* 核心对象：如果对象的Eps邻域至少包含最小数目MinPts的对象，则称该对象为核心对象。  
* 边界点：边界点不是核心点，但落在某个核心点的邻域内。  
* 噪音点：既不是核心点，也不是边界点的任何点。  
* 直接密度可达：给定一个对象集合D，如何p在q的Eps邻域内，而q是一个核心对象，则称对象p从对象q出发时是直接密度可达的（directly density-reachable）。  
* 密度可达：如果存在一个对项链 $p_1, p_2, \dots, p_n, p_1=q, p_n=p$, 对于 $p_i\in{D}(1\leq{i}\leq{n})$, $p_{i+1}$ 是从 $p_i$关于Eps和MinPts直接密度可达的，则对象p是从对象q关于Eps和MinPts密度可达的（density-reachable）。  
* 密度相连：如果存在对象 $O\in{D}$, 使对象p和q都是从O关于Eps和MinPts密度可达的，那么对象p到q是关于Eps和MinPts密度相连的（density-connected）。  

![][pt_35]  

## DBSCAN算法概念示例  

如图所示，Eps用一个相应的半径表示，设MinPts=3，请分析Q,M,P,S,O,R这5个样本点之间的关系。  

![][pt_36]  

**解答：**根据以上概念知道：由于有标记的各点M、P、O和R的Eps近邻均包含3个以上的点，因此它们都是核对象；M是从P“直接密度可达”；而Q则是从M“直接密度可达”；基于上述结果，Q是从P“密度可达”；但P从Q无法“密度可达”(非对称)。类似地，S和R从O是“密度可达”的；O、R和S均是“密度相连”的。  

## DBSCAN算法原理  

* DBSCAN通过检查数据集中每点的Eps邻域来搜索簇，如果点p的Eps邻域包含的点多于MinPts个，则创建一个以p为核心对象的簇。  

* 然后，DBSCAN迭代地聚集从这些核心对象直接密度可达的对象，这个过程可能涉及一些密度可达簇的合并。  

* 当没有新的点添加到任何簇时，该过程结束。  

## DBSCAN示例  

![][pt_37]  

从上述示例可知：DBSCAN对噪音不敏感，并且可以处理不同形状和大小的数据。  

## DBSCAN算法的优缺点  

**优点：**  

* 基于密度定义，相对抗噪音，能处理任意形状和大小的簇。  

**缺点：**  

* 当簇的密度变化太大时，会有麻烦；  
* 对于高维问题，密度定义是个比较麻烦的问题；  
* 当近邻计算需要计算所有的点对近邻度时，DBSCAN可能是开销很大的。  

## 时间复杂性和空间复杂度  

DBSCAN 的基本时间复杂度是 $O(mx找出 Eps 邻域中的点所需要的时间)$，其中 $m$ 是点的个数。在最坏情况下，时间复杂度是 $O(m^2)$。然而，在低维空间，有一些数据结构，如 kd 树，可以有效地检索特定点给定距离内的所有点，时间复杂度可以降低到 $O(m \log{m})$。即便对于高维数据，DBSCAN 的空间也是 $O(m)$，因为对每个点，它只需要维持少量数据，即簇标号和每个点是核心点、边界点还是噪声点的标识。  

# 簇评估（=簇确认）  

为何评价聚类分析结果：  

* 避免发现噪声产生的模式  
* 比较不同的聚类算法  
* 比较不同的簇集合  
* 簇之间的比较  

三种聚类方式在随机点上的表现：  

![][pt_38]  

## 概述  

**簇评估的重要问题：**  

1. 确定数据集的聚类趋势 clustering tendency ，即是否存在非随机结构；  
2. 确定正确的簇的个数；  
3. 评估聚类分析结果对数据的拟合情况 - Use only the data；  
4. 将聚类分析的结果跟已知的客观结果（如，外部提供的类标号）比较；  
5. 比较不同的聚类方法的优劣。

## 簇评估的度量  

用于评估簇的各方面的评估度量或指标一般分成如下三类。  

* 外部指标 External Index: **监督的** 度量聚类算法发现的聚类结构与某种外部结构的匹配程度。例如，监督指标的**熵**，它度量簇标号与外部提供的标号的匹配程度。监督度量通常称为外部指标 (externalindex)，因为它们使用了不在数据集中出现的信息。  

* 内部指标 Internal Index: **非监督的** 聚类结构的优良性度量，不考虑外部信息。例如，**SSE**(Sum of Squared Error)。簇的有效性的非监督度量常常可以进一步分成两类: 簇的凝聚性(cluster cohesion)(紧凑性，紧致性)度量确定中对象如何密切相关，簇的分离性 (cluster separation)(孤立性度量确定某个簇不同于其他簇的地方。非监督度量通常称为内部指标 (intermal index)，因为它们仅使用出现在数据集中的信息。  

* 相对指标 Relative Index: 比较不同的聚类或簇。相对簇评估度量是用于比较的监督或非监督评估度量。因而，相对度量实际上不是一种单独的簇评估度量类型，而是度量的一种具体使用。例如，两个 K 均值聚类可以使用 SSE 或进行比较。  

### 非监督簇评估：邻近性矩阵  

理想簇的点与簇内所有点的相似度为 1，而与其他中的所有点的相似度为0。将相似度矩阵的行和列排序，使得属于相同簇的对象在一起，则理想的相似度矩阵具有**块对角(block diagonal)**结构。  

使用K-means聚类方法对两种数据集分别进行聚类(右边是随机数据点)：  

![][pt_39]  

将上面左图的聚类结果的相似性矩阵按照聚类标签排序并进行可视化的结果如下：  

![][pt_40]  

将随机点数据集的DBSCAN聚类结果的相似性矩阵按照聚类标签排序并进行可视化的结果如下：  

![][pt_41]  

将随机点数据集的K-means聚类结果的相似性矩阵按照聚类标签排序并进行可视化的结果如下：  

![][pt_42]  

将随机点数据集的全链聚类结果的相似性矩阵按照聚类标签排序并进行可视化的结果如下：  

![][pt_43]  

### 非监督簇评估：SSE  

SSE 适合评估多个簇集或者多个簇 (average SSE)，并且SSE属于内部指标，并不需要除数据集外的信息。  

![][pt_44]  

SSE曲线可以使用更为复杂的数据集：  

![][pt_45]  

### 非监督簇评估：凝聚度和分离度  

通常，将K个簇的集合的总体簇有效性表示成个体簇有效性的加权和:  

$$
overall validity =\sum_{i=1}^{K}{\omega_i{validity(C_i)}}
$$

其中，validity 函数可以是凝聚度、分离度，或者这些量的某种组合。权值将因簇有效性度量而异。在某些情况下，权值可以简单地取 1 或者簇的大小；而在其他情况下，它们反映更复杂的性质，如凝聚度的平方根。如果有效性函数是凝聚度，则值越高越好。如果是分离度，则值越低越好。  

![][pt_48]  

#### 凝聚度和分离度的基于图的观点  

对于基于图的簇，簇的凝聚度可以定义为连接簇内点的邻近度图中边的加权和。邻近度图以数据对象为结点，每对数据对象之间一条边，并且每条边指派一个权值它是边所关联的两个数据对象之间的邻近度。两个簇之间的分离度可以用从一个簇的点到另一个簇的点的边的加权和来度量。  

![][pt_46]  

基于图的簇的凝聚度和分离度的公式如下：  

$$
cohesion(C_i)=\sum_{x\in{C_i} \atop y\in{C_i}}{proximity(x, y)}
$$
$$
separation(C_i, C_j)=\sum_{x\in{C_i} \atop y\in{C_j}}{proximity(x, y)}
$$

#### 凝聚度和分离度的基于原型的观点  

对于基于原型的簇，簇的凝聚度可以定义为关于簇原型(质心或中心点)的邻近度的和。同理，两个簇之间的分离度可以用两个簇原型的邻近性度量(其中族的质心用“+”标记)。  

![][pt_47]  

基于原型的凝聚度公式如下(注意，如果取邻近度为平方欧几里得距离，则该公式是簇的SSE)：  

$$
cohesion(C_i)=\sum_{x\in{C_i}}{proximity(x, c_i)}
$$

对于分离性，存在两种度量，这是因为簇原型与总原型的分离度有时与簇原型之间的分离度直接相关。基于原型的凝聚度公式如下：  

$$
separation(C_i, C_j)=proximity(c_i, c_j)  
$$
$$
seperation(C_i)=proximity(c_i, c)
$$

#### 两种基于原型的分离性度量方法  

当邻近度用欧几里得距离度量时，簇之间分离性的传统度量是组平方和 (SSB)，即簇质心$c_i$到所有数据点的总均值c的距离的平方和。通过在所有簇上对 SSB 求和，我们得到总 SSB，公式如下：  

$$
总SSB=\sum_{i=1}^{K}{m_i{dist}(c_i, c)^2}
$$

其中$c_i$是第i个簇的均值，而c是总均值。总SSB 越高，簇之间的分离性越好。  

总 SSB 与质心之间的逐对距离有直接关系。特殊地，如果簇的大小相等，即 $m_i = m/K$，则该关系取公式如下：  

$$
总SSB=\frac{1}{2K}\sum_{i=1}^{K}{\sum_{j=1}^{K}{\frac{m}{K}{dist(c_i, c_j)^2}}}
$$

#### 凝聚度和分离度之间的联系  

在某些情况下，凝聚度和分离度之间也存在很强的联系。具体地说，可以证明总 SSE 和总SSB 之和是一个常数，它等于总平方和(TSS)——每个点到数据的总均值的距离的平方和。这个结果的重要性在于：最小化 SSE(凝聚度)等价于最大化 SSB(分离度)。  

### 轮廓系数  

流行的轮廓系数(silhouette coffcient)方法结合了凝聚度和分离度。下面的步骤解释如何计算个体点的轮廓系数（这里使用距离，可以使用相似度）：  
1. 对第i个对象，计算它到簇中所有其他对象的平均距离。该值记作$a_i$;  
2. 对于第i个对象和不含该对象的任意簇，计算该对象到给定中有对象的平均距离。关于所有的簇，找出最小值；该值记作$b_i$;  
3. 对于第i个对象，轮廓系数是 $S_i=(b_i-a_i)/\max{(a_i, b_i)}$。  

轮廓系数的值在-1 和 1之间变化。负值表示点到内点的平均距离$a_i$大于点到其他的最小平均距离$b_i$，因此轮廓系数最好是正的$(a_i<b_i)$，并且$a_i$越接近越好，因为当$a_i=0$时轮廓系数取其最大值1。  

一般来说，可以简单地取簇中点的轮廓系数的平均值，计算簇的平均轮廓系数。通过计算所有点的平均轮廓系数，可以得到聚类优良性的总度量。  



转载请注明：[南梦婷的博客](https://norah2.github.io) » [点击阅读原文](https://norah2.github.io/2023/03/03/ClusterAnalysis/) 

<!--本文用到的链接-->

[pt_01]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/79_ClusterAnalysis/01.png  
[pt_02]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/79_ClusterAnalysis/02.png  
[pt_03]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/79_ClusterAnalysis/03.png  
[pt_04]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/79_ClusterAnalysis/04.png  
[pt_05]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/79_ClusterAnalysis/05.png  
[pt_06]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/79_ClusterAnalysis/06.png  
[pt_07]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/79_ClusterAnalysis/07.png  
[pt_08]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/79_ClusterAnalysis/08.png  
[pt_09]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/79_ClusterAnalysis/09.png  
[pt_10]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/79_ClusterAnalysis/10.png  
[pt_11]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/79_ClusterAnalysis/11.png  
[pt_12]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/79_ClusterAnalysis/12.png  
[pt_13]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/79_ClusterAnalysis/13.png  
[pt_14]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/79_ClusterAnalysis/14.png  
[pt_15]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/79_ClusterAnalysis/15.png  
[pt_16]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/79_ClusterAnalysis/16.png  
[pt_17]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/79_ClusterAnalysis/17.png  
[pt_18]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/79_ClusterAnalysis/18.png  
[pt_19]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/79_ClusterAnalysis/19.png  
[pt_20]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/79_ClusterAnalysis/20.png  
[pt_21]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/79_ClusterAnalysis/21.png  
[pt_22]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/79_ClusterAnalysis/22.png  
[pt_23]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/79_ClusterAnalysis/23.png  
[pt_24]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/79_ClusterAnalysis/24.png  
[pt_25]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/79_ClusterAnalysis/25.png  
[pt_26]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/79_ClusterAnalysis/26.png  
[pt_27]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/79_ClusterAnalysis/27.png  
[pt_28]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/79_ClusterAnalysis/28.png  
[pt_29]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/79_ClusterAnalysis/29.png  
[pt_30]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/79_ClusterAnalysis/30.png  
[pt_31]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/79_ClusterAnalysis/31.png  
[pt_32]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/79_ClusterAnalysis/32.png  
[pt_33]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/79_ClusterAnalysis/33.png  
[pt_34]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/79_ClusterAnalysis/34.png  
[pt_35]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/79_ClusterAnalysis/35.png  
[pt_36]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/79_ClusterAnalysis/36.png  
[pt_37]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/79_ClusterAnalysis/37.png  
[pt_38]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/79_ClusterAnalysis/38.png  
[pt_39]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/79_ClusterAnalysis/39.png  
[pt_40]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/79_ClusterAnalysis/40.png  
[pt_41]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/79_ClusterAnalysis/41.png  
[pt_42]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/79_ClusterAnalysis/42.png  
[pt_43]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/79_ClusterAnalysis/43.png  
[pt_44]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/79_ClusterAnalysis/44.png  
[pt_45]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/79_ClusterAnalysis/45.png  
[pt_46]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/79_ClusterAnalysis/46.png  
[pt_47]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/79_ClusterAnalysis/47.png  
[pt_48]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/79_ClusterAnalysis/48.png  

{% endraw %}
