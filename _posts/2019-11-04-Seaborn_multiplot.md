---
layout: post
title: Seaborn——Building structured multi-plot grids  
date: 2019-11-04
description: 有关分类变量的绘制  
categories: Visualize
tags: Visualize Python Seaborn
author: NMt
mathjax: true
---

* content
{:toc}

有关分类变量的绘制 

<div style='display: none'>
@@@@
</div>




{% raw %}
当探索medium-dimensional数据时，一种有用的方法是在数据集的不同子集上绘制同一绘图的多个实例。这种技术有时被称为“lattice”or“trellies”图，它与“small multiples”的概念有关。它允许查看器快速提取有关复杂数据的大量信息。maltiplotlib为制作多轴图形提供了很好的支持。seaborn在此基础上构建，将绘图结构直接连接到数据集结构。  

要使用这些特征，数据必须在DataFrame中，并且采用 Hadley Whickam 所称的“tidy”数据的形式。简而言之，这意味着数据帧的结构应该使每一列都是一个变量，每一行都是一个观察值。  

对于高级使用，您可以直接使用教程中的这部分讨论的对象，这将提供最大的灵活性。一些seaborn函数（如lmplot（）、catplot（）和pairplot（））也在幕后使用它们。不同于其他“轴级”的Sabrn函数，并绘制到特定的（可能已经存在）MatpTrLIB轴上，而不用另外处理图形，这些高级函数在调用时创建一个图形，并且通常对它如何设置更严格。在某些情况下，这些函数的参数或它们所依赖的类的构造函数的参数将提供不同的接口属性，如图大小，例如lmplot（）的情况，您可以为每个方面设置高度和纵横比，而不是图的总体大小。但是，任何使用这些对象之一的函数都会在打印后返回它，而且这些对象中的大多数都有方便的方法来更改绘图的方式，通常是以更抽象和简单的方式。

导入所需要的包：  

```python
import seaborn as sns
import matplotlib.pyplot as plt

sns.set(style='ticks')
```

### Conditional small multiples  

当需要在数据集的子集内分别可视化变量的分布或多个变量之间的关系时，FacetGrid类非常有用。FacetGrid最多可以绘制三个维度：行、列和色调。前两个与所得到的轴阵列具有明显的对应关系；将色调变量视为沿深度轴的第三个维度，其中不同级别绘制不同颜色的色调。  

该类通过使用数据帧初始化FacetGrid对象以及将形成网格的行、列或色调维度的变量的名称来使用。这些变量应该是分类的或离散的，然后在该变量的每个级别上的数据将用于沿该轴的刻面。例如，假设我们想在tips数据集中检查午餐和晚餐之间的差异。  

此外，relplot（）、catplot（）和lmplot（）中的每一个都在内部使用该对象，并且在完成后返回该对象，以便可以使用它进行进一步的调整。  

导入数据集：  

```python
tips = sns.load_dataset("tips")
```

```python
g = sns.FacetGrid(tips, col="time")
```

结果如下图所示：  

![][pt_01]  

这样初始化网格可以设置matplotlib图形和轴，但不会在它们上绘制任何内容。

在这个网格上可视化数据的主要方法是`facetgrid.map()`方法。为它提供一个绘图函数和要绘图的数据框中变量的名称让我们使用直方图来查看每个子集中的提示分布。

```python
g = sns.FacetGrid(tips, col="time")
g.map(plt.hist, "tip")
```

结果如下图所示：  

![][pt_02]  

此函数将绘制图形并注释轴，希望一步就能生成完成的绘图。要绘制关系图，只需传递多个变量名。还可以提供关键字参数，这些参数将传递给绘图函数：

```python
g = sns.FacetGrid(tips, col="sex", hue="smoker")
g.map(plt.scatter, "total_bill", "tip", alpha=.7)
g.add_legend()
```

结果如下图所示：  

![][pt_03]  

有几个用于控制网格外观的选项可以传递给类构造函数。

```python
g = sns.FacetGrid(tips, row="smoker", col="time", margin_titles=True)
g.map(sns.regplot, "size", "total_bill", color=".3", fit_reg=False, x_jitter=.1)
```

结果如下图所示：  

![][pt_04]

注意margin_标题并没有得到matplotlib API的正式支持，并且在所有情况下可能都不能很好地工作尤其是，它目前不能用于情节之外的传说。

通过提供每个侧面的高度以及纵横比来设置图形的大小：

```python
g = sns.FacetGrid(tips, col="day", height=4, aspect=.5)
g.map(sns.barplot, "sex", "total_bill")
```

结果如下图所示：  

![][pt_05]

facet的默认顺序是从dataframe中的信息派生的。如果用于定义facet的变量具有类别类型，则使用类别的顺序。否则，面将按照类别级别的外观顺序排列但是，可以使用适当的*顺序参数指定任何方面维度的顺序：

```python
ordered_days = tips.day.value_counts().index
g = sns.FacetGrid(tips, row="day", row_order=ordered_days,
                  height=1.7, aspect=4,)
g.map(sns.distplot, "total_bill", hist=False, rug=True)
```

结果如下图所示：  

![][pt_06]

可以提供任何Seaborn调色板（即可以传递给color_palette（）的内容）。也可以使用字典将色调变量中的值的名称映射为有效的matplotlib颜色：

```python
pal = dict(Lunch="seagreen", Dinner="gray")
g = sns.FacetGrid(tips, hue="time", palette=pal, height=5)
g.map(plt.scatter, "total_bill", "tip", s=50, alpha=.7, linewidth=.5, edgecolor="white")
g.add_legend()
```

结果如下图所示：  

![][pt_07]  

您还可以让绘图的其他方面在色调变量的不同级别上变化，这有助于绘制在黑白打印时更易于理解的绘图。为此，请将字典传递给hue-kws，其中键是绘图函数的名称关键字参数，值是关键字值列表，每个级别的hue变量一个。  

```python
g = sns.FacetGrid(tips, hue="sex", palette="Set1", height=5, hue_kws={"marker": ["^", "v"]})
g.map(plt.scatter, "total_bill", "tip", s=100, linewidth=.5, edgecolor="white")
g.add_legend()
```

结果如下图所示：  

![][pt_08]

如果一个变量有多个级别，可以沿列打印，但可以“包装”它们，使它们跨多行执行此操作时，不能使用行变量。

```
#attend = sns.load_dataset("attention").query("subject <= 12")
import pandas as pd

attend = pd.read_csv('./seaborn_data/attention.csv').query("subject <= 12")
g = sns.FacetGrid(attend, col="subject", col_wrap=4, height=2, ylim=(0, 10))
g.map(sns.pointplot, "solutions", "score", color=".3", ci=None)
```

结果如下图所示：  

![][pt_09]  

使用facetgrid.map（）绘制绘图后（可以多次调用），可能需要调整绘图的某些方面。FacetGrid对象上还有许多方法用于在更高抽象级别上操作图形。最常见的是FAutGrave.Stand（），还有其他更专业的方法，如FAXGRADE。SETAXISISLabLeSLs（），它们尊重内部方面没有轴标签的事实。例如：

```python
with sns.axes_style("white"):
    g = sns.FacetGrid(tips, row="sex", col="smoker", margin_titles=True, height=2.5)
g.map(plt.scatter, "total_bill", "tip", color="#334488", edgecolor="white", lw=.5);
g.set_axis_labels("Total bill (US Dollars)", "Tip");
g.set(xticks=[10, 30, 50], yticks=[2, 6, 10]);
g.fig.subplots_adjust(wspace=.02, hspace=.02);
```

结果如下图所示：  

![][pt_10]

对于更多自定义，可以直接使用下划线matplotlib figure和axes对象，它们分别存储为fig和axes（二维数组）的成员属性。在生成没有行或列镶嵌面的图形时，还可以使用ax属性直接访问单轴。

```python
g = sns.FacetGrid(tips, col="smoker", margin_titles=True, height=4)
g.map(plt.scatter, "total_bill", "tip", color="#338844", edgecolor="white", s=50, lw=1)
for ax in g.axes.flat:
    ax.plot((0, 50), (0, .2 * 50), c=".2", ls="--")
g.set(xlim=(0, 60), ylim=(0, 14));
```

结果如下图所示：  

![][pt_11]

### Using custom functions  

在使用FAGETGRID时，不局限于现有的MatpTLILB和SabOn函数。但是，要正常工作，您使用的任何函数都必须遵循以下规则：

1. 它必须绘制到“当前活动”的matplotlib轴上。matplotlib.pyplot名称空间中的函数也是这样，如果希望直接使用当前轴的方法，可以调用plt.gca来获取对当前轴的引用。

2. 它必须接受在位置参数中打印的数据。在内部，FacetGrid将为传递给FacetGrid.map（）的每个命名位置参数传递一系列数据。

3. 它必须能够接受颜色和标签关键字参数，并且，理想情况下，它将对它们做一些有用的事情。在大多数情况下，最容易捕获**kwargs的通用字典并将其传递给底层的绘图函数。

让我们看一个可以绘制函数的最小示例此函数将只获取每个方面的单个数据向量：

```python
from scipy import stats
def quantile_plot(x, **kwargs):
    qntls, xr = stats.probplot(x, fit=False)
    plt.scatter(xr, qntls, **kwargs)

g = sns.FacetGrid(tips, col="sex", height=4)
g.map(quantile_plot, "total_bill")
```

结果如下图所示：  

![][pt_11]

如果我们要做一个二元图，你应该写函数，以便它接受x轴变量第一和y轴变量第二：  

```python
def qqplot(x, y, **kwargs):
    _, xr = stats.probplot(x, fit=False)
    _, yr = stats.probplot(y, fit=False)
    plt.scatter(xr, yr, **kwargs)

g = sns.FacetGrid(tips, col="smoker", height=4)
g.map(qqplot, "total_bill", "tip")
```

结果如下图所示：　　

![][pt_12]

因为plt.scatter接受颜色和标签关键字参数，并且对它们做了正确的处理，所以我们可以毫无困难地添加色调方面：

```python
g = sns.FacetGrid(tips, hue="time", col="sex", height=4,
                  hue_kws={"marker": ["s", "D"]})
g.map(qqplot, "total_bill", "tip", s=40, edgecolor="w")
g.add_legend()
```

结果如下图所示：  

![][pt_13]

此方法还允许我们使用附加美学来区分色调变量的级别，以及不依赖于镶嵌面变量的关键字参数：

```python
def hexbin(x, y, color, **kwargs):
    cmap = sns.light_palette(color, as_cmap=True)
    plt.hexbin(x, y, gridsize=15, cmap=cmap, **kwargs)

with sns.axes_style("dark"):
    g = sns.FacetGrid(tips, hue="time", col="time", height=4)
g.map(hexbin, "total_bill", "tip", extent=[0, 50, 0, 10])
```

结果如下图所示：  

![][pt_14]

### Plotting pairwise data relationship  

PairGrid还允许您使用相同的绘图类型快速绘制一个由小的子块组成的网格，以可视化每个子块中的数据在PairGrid中，每一行和每一列都被分配给一个不同的变量，因此结果图显示了数据集中的每一对关系这种绘图风格有时被称为“散点图矩阵”，因为这是显示每种关系的最常用方法，但pairgrid不限于散点图。

理解facetgrid和pairgrid之间的区别很重要。在前者中，每个方面都显示了相同的关系，条件是其他变量的不同水平在后者中，每个图都显示不同的关系（尽管上下三角形将具有镜像图）使用PairGrid可以让您对数据集中有趣的关系进行非常快速、非常高级别的总结。

这个类的基本用法与FacetGrid非常相似首先初始化网格，然后将绘图函数传递给map方法，并在每个子块上调用它。还有一个同伴函数，PoopTrand（），用一些灵活性来加快绘图速度。

```python
iris = sns.load_dataset("iris")
g = sns.PairGrid(iris)
g.map(plt.scatter)
```

结果如下图所示：  

![][pt_15]

可以在对角线上绘制不同的函数，以显示每列中变量的单变量分布。请注意，轴蜱将不对应于计数或密度轴这个阴谋，虽然。

```python
g = sns.PairGrid(iris)
g.map_diag(plt.hist)
g.map_offdiag(plt.scatter)
```

结果如下图所示：  

![][pt_16]

使用此图的一种非常常见的方法是通过一个单独的分类变量对观测值进行着色例如，虹膜数据集对三种不同种类的虹膜花分别进行了四次测量，因此您可以看到它们之间的差异。

```python
g = sns.PairGrid(iris, hue="species")
g.map_diag(plt.hist)
g.map_offdiag(plt.scatter)
g.add_legend()
```

结果如下图所示：  

![][pt_17]

默认情况下，数据集中的每一个数值列都会被使用，但是如果需要，您可以关注特定的关系。

```python
g = sns.PairGrid(iris, vars=["sepal_length", "sepal_width"], hue="species")
g.map(plt.scatter)
```

结果如下图所示：  

![][pt_18]

也可以在上下三角形中使用不同的函数来强调关系的不同方面。

```python
g = sns.PairGrid(iris)
g.map_upper(plt.scatter)
g.map_lower(sns.kdeplot)
g.map_diag(sns.kdeplot, lw=3, legend=False)
```

结果如下图所示：  

![][pt_19]

对角线上具有标识关系的正方形网格实际上只是一种特殊情况，您可以使用行和列中的不同变量进行绘图。

```python
g = sns.PairGrid(tips, y_vars=["tip"], x_vars=["total_bill", "size"], height=4)
g.map(sns.regplot, color=".3")
g.set(ylim=(-1, 11), yticks=[0, 5, 10])
```

结果如下图所示：  

![][pt_20]

当然，审美属性是可配置的例如，可以使用不同的调色板（例如，显示色调变量的顺序）并将关键字参数传递到绘图函数中。

```python
g = sns.PairGrid(tips, hue="size", palette="GnBu_d")
g.map(plt.scatter, s=50, edgecolor="white")
g.add_legend()
```

结果如下图所示：  

![][pt_21]

配对网格是灵活的，但是为了快速查看数据集，它可以更容易地使用对偶（）。默认情况下，此函数使用散点图和直方图，但会添加一些其他类型的散点图和直方图（当前，您也可以在非对角线和对角线上绘制回归图）。

```python
sns.pairplot(iris, hue="species", height=2.5)
```

结果如下图所示：  

![][pt_22]

还可以使用关键字参数控制绘图的美观性，并返回pairgrid实例以进行进一步调整。

```python
g = sns.pairplot(iris, hue="species", palette="Set2", diag_kind="kde", height=2.5)
```

结果如下图所示：  

![][pt_23]

参考文献：  

http://seaborn.pydata.org/tutorial/axis_grids.html  


转载请注明：[南梦婷的博客](https://norah2.github.io) » [点击阅读原文](https://norah2.github.io/2019/11/04/Seaborn_multiplot/)   

<!--以下是本文用到的链接-->  

[pt_01]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/47_Seaborn_multiplot/01.png
[pt_02]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/47_Seaborn_multiplot/02.png
[pt_03]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/47_Seaborn_multiplot/03.png
[pt_04]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/47_Seaborn_multiplot/04.png
[pt_05]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/47_Seaborn_multiplot/05.png
[pt_06]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/47_Seaborn_multiplot/06.png
[pt_07]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/47_Seaborn_multiplot/07.png
[pt_08]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/47_Seaborn_multiplot/08.png
[pt_09]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/47_Seaborn_multiplot/09.png
[pt_10]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/47_Seaborn_multiplot/10.png
[pt_11]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/47_Seaborn_multiplot/11.png
[pt_12]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/47_Seaborn_multiplot/12.png
[pt_13]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/47_Seaborn_multiplot/13.png
[pt_14]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/47_Seaborn_multiplot/14.png
[pt_15]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/47_Seaborn_multiplot/15.png
[pt_16]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/47_Seaborn_multiplot/16.png
[pt_17]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/47_Seaborn_multiplot/17.png
[pt_18]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/47_Seaborn_multiplot/18.png
[pt_19]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/47_Seaborn_multiplot/19.png
[pt_20]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/47_Seaborn_multiplot/20.png
[pt_21]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/47_Seaborn_multiplot/21.png
[pt_22]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/47_Seaborn_multiplot/22.png
[pt_23]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/47_Seaborn_multiplot/23.png

{% endraw %}
