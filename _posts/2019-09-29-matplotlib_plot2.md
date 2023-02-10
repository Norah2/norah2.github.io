---
layout: post
title: matplotlib中有关图表的细节设置  
date: 2019-09-29
description: 本文中包括如何设置坐标轴标签透明度以及大小、添加线条阴影、添加数据表格、划分子区、定制网格grid、等高线图、填充图表底层区域、极线图。  
categories: Visualize
tags: Python Visualize Matplotlib
author: NMt
mathjax: true
---

* content
{:toc}

本文中包括如何设置坐标轴标签透明度以及大小、添加线条阴影、添加数据表格、划分子区、定制网格grid、等高线图、填充图表底层区域、极线图。  

<div style='display: none'>
@@@@
</div>




{% raw %}

注：本篇博客是看《Python数据可视化编程实战(第2版)》这本书第四章的总结。  

### matplotlib如何组织图表  

最上层是一个Figure实例，包含了所有可见的和其他一些不可见的内容。该Figure实例包含了一个Axes实例字段`Figure.axes`。Axes实例几乎包含了我们所关心的所有东西，如线、点、刻度、标签。因此，当调用`plot()`方法时，程序就会向`Axes.lines`列表添加一个线条的实例(`matplotlib.lines.Line2D`)。如果绘制了一个直方图（通过调用`hist()`），程序就会向`Axes.patches`列表添加许多矩形（"patches"是从MATLABTM继承来的一个术语，表示“颜色补片”的概念）。  

Axes实例也包含了XAxis和YAxis实例的引用，分别指向相应的x轴与y轴。XAxis和YAxis管理坐标轴、标签、刻度、刻度标签、定位器和格式器的绘制，我们可以通过`Axes.xaxis`和`Axes.yaxis`分别引用它们。其实不必按照前面所说的方式通过XAis或YAxis实例得到标签对象，因为matplotlib提供了helper方法（实际上是一个捷径）来迭代这些标签，它们是`matplotlib.pyplot.xlabel()`和`matplotlib.pyplot.ylabel()`。  

### 设置坐标轴标签的透明度和大小  

```python
import matplotlib.pyplot as plt
from matplotlib import patheffects
import numpy as np

data = np.random.randn(70)

fontsize = 18
plt.plot(data)

title = "This is figure title"
x_label = "This is x axis label"
y_label = "This is y axis label"

title_text_obj = plt.title(title, fontsize=fontsize, verticalalignment='bottom')

title_text_obj.set_path_effects([patheffects.withSimplePatchShadow()])

#offset_xy -- set the 'angle' of the shadow
#shadow_rgbFace -- set the color of the shadow
#patch_alpha -- setup the transparaency of the shadow

offset_xy = (1, -1)
rgbRed = (1.0,0.0,0.0)
alpha = 0.8

#customize shadow properties
pe = patheffects.withSimplePatchShadow(offset = offset_xy,
                                       shadow_rgbFace = rgbRed,
                                       alpha = alpha)
#apply them to the xaxis and yaxis labels
xlabel_obj = plt.xlabel(x_label, fontsize=fontsize, alpha=0.5)
xlabel_obj.set_path_effects([pe])

ylabel_obj = plt.ylabel(y_label, fontsize=fontsize, alpha=0.5)
ylabel_obj.set_path_effects([pe])

plt.show()
```

结果如下图所示：  

![][pt_02]  

代码说明：  

首先，添加一个标题，接着设置标题字体的大小，并将标题文本的垂直对齐方式设置为'bottom'。  

如果不带参数地调用`matplotlib.patheffects.withSimplePatchShadow()`，会为标题添加默认的阴影效果。参数的默认值为`offset=(2, -2), shadow_rgbFace=None, alpha=0.7`。标题的垂直对齐方式有center、top以及baseline，因为要为文本添加阴影，所以选择bottom。路径效果（path effects）是`matplotlib.patheffects`模块的部分功能，支持`matplotlib.text.Text`和`matplotlib.patches.Patch`。  

接着为x轴和y轴添加不同的阴影设置。首先，我们自定义相对于父对象（标题文本对象）的阴影的位置(offset，偏移)，然后设置阴影的颜色。  

透明度alpha（0-1）。  

设置完毕后，实例化`matplotlib.patheffects.withSimplePatchShadow`对象，并将其引用保存在pe变量中以供后面的代码重用它。  

为了能应用阴影效果，我们需要得到label对象。`matplotlib.pyplot.xlabel()`返回了该对象（matplotlib.text.Text）的引用。然后用它来调用`set_path_effects([pe])`方法。  

### 为图表线条添加阴影  

为了向图表中的线条或者矩形条添加阴影，我们需要使用matplotlib内置的transformation框架，它位于matplotlib.transforms模块中。  

transformation知道如何将给定的坐标从其坐标系转换到显示坐标系中，也知道如何将坐标从显示坐标系转换成他们自己的坐标系。  

<!--
<style>
table th:first-of-type {
	width: 200px;
}
</style>
-->

|坐标系|transformation对象|描述|
|:----:|:----------------:|:---:|
|Data  |Axes.transData    |表示用户的数据坐标系|
|Axes  |Axes.transAxes    |表示Axes坐标系，其中(0, 0)表示轴的左下角，(1, 1)表示轴的右上角|
|Figure|Figure.transFigure|是Figure坐标系，其中(0, 0)表示图表的左下角，(1, 1)表示图表的右上角|
|Display|None             |表示用户视窗的像素坐标系，其中(0, 0)表示视窗的左下角，(width, height)元组表示显示界面的右上角。<br>这里的width和height都是以像素为单位的。|

注意：在transformation对象列中，视窗坐标系是没有值的。这是因为默认的坐标系就是Display坐标系，坐标总是在视窗坐标系下并以像素为单位。但这并没有太大的用处，因为大多数情况下我们想把坐标归一化到Figure、Axes或者一个Data坐标系中。这个框架能让我们把现有对象转化成一个偏移对象，也就是说，把对象放置到偏离原来对象一段距离的地方。  

```python
def setup(layout=None):
    assert layout is not None
    
    fig = plt.figure()
    ax = fig.add_subplot(layout)
    return fig, ax

def get_signal():
    t = np.arange(0., 2.5, 0.01)
    s = np.sin(5 * np.pi * t)
    return t, s

def plot_signal(t, s):
    line, =axes.plot(t, s, linewidth=5, color='magenta')
    return line

def make_shadow(fig, axes, line, t, s):
    delta = 2 / 72.  # how many points to move the shadow
    offset = transforms.ScaledTranslation(delta, -delta, fig.dpi_scale_trans)
    offset_transform = axes.transData + offset
    #We plot the same data, bue now using offset transform
    #zorder -- to render it below the line
    axes.plot(t, s, linewidth=5, color='gray', transform=offset_transform, zorder=0.5 * line.get_zorder())

if __name__ == "__main__":
    fig, axes = setup(111)
    t, s = get_signal()
    line = plot_signal(t, s)
    make_shadow(fig, axes, line, t, s)
    axes.set_title('Shadow effect using an offset transform')
    plt.show()
```

结果如下图所示：  

![][pt_03]  

代码说明：  

首先，通过`setup()`创建figure和axes，然后生成一个正弦波数据。在`plot_signal()`方法中绘制出基本的信号图。最后，进行阴影坐标转换并在`make_shadow()`方法中绘制出阴影。  

使用偏移效果创建一个偏移对象，把阴影放置在原始对象之下并偏移几个点的距离。  

原始对象是一个简单的正弦波，用`plot()`方法绘制。  

matplotlib包含一个transformation helper——matplotlib.transforms.ScaledTranslation。它用来添加偏移转换。  

dx和dy的值以点为单位。点是0.353mm，向右移动偏移对象2pt，向下移动偏移对象2pt。  

可以使用`matplotlib.transforms.ScaledTransformation(xtr, ytr,scaletr)`方法。这里xtr和ytr是转换的偏移量，scaletr是一个转换可调用对象（callable），在转换和显示之前对xtr和ytr进行比例调整。其最常用的情况是从点转换到显示区域，如DPI，这样偏移始终保持在相同的位置而与实际的输出设备无关。我们使用的可调用对象已经内置在matplotlib中，可以从`Figure.dpi_scale_trans`中得到。  

另一串代码：  

```python
import matplotlib.pyplot as plt
import matplotlib.patheffects as path_effects

fig = plt.figure(figsize=(8, 1))
ax = plt.gca()
t = ax.text(0.02, 0.5, 'Hatch shadow', fontsize=75, weight=1000, va='center')
t.set_path_effects([path_effects.PathPatchEffect(offset=(4, -4), hatch='xxxx',
                                                  facecolor='gray'),
                    path_effects.PathPatchEffect(edgecolor='white', linewidth=1.1,
                                                 facecolor='black')])
plt.axis('off')
plt.show()
```

结果如下图所示：  

![][pt_01]  

### 向图表添加数据表  

通过精心挑选的、或者来自数据整体集合的总结性的或者突出强调的值，我们可以识别出图表的重要部分，并在一些地方强调一些非常重要的值。在这些地方，这些精确的值是非常重要的。  

```python
import matplotlib.pylab as plt
import numpy as np

plt.figure()
ax = plt.gca()
y = np.random.randn(9)

col_labels = ['col1', 'col2', 'col3']
row_labels = ['row1', 'row2', 'row3']
table_vals = [[11, 12, 13], [21, 22, 23], [28, 29, 30]]
row_colors = ['red', 'gold', 'green']
my_table = plt.table(cellText=table_vals, colWidths=[0.1]*3, rowLabels=row_labels, 
                     colLabels=col_labels, rowColours=row_colors, loc='upper right')
plt.plot(y)
plt.show()
```

结果如下图所示：  

![][pt_04]  

代码说明如下：  

使用plt.table()方法创建一个带单元格的表格，并把它添加到当前坐标轴中。表格可以有(可选的)行标题和列标题。每个单元格包含文本或补片。表格的列宽和行高是可以指定的。返回只是一个组成表格的对象（文本、线条、补片实例）序列。  

基本的函数如下：  

table(cellText=None, cellColours=None,  
cellLoc='right', colWidth=None,   
rowLabels=None, rowColours=None, rowLoc='left',  
colLabels=None, colColours=None, colLoc='center',  
loc='bottom', bbox=None)  

函数实例化并返回一个matplotlib.table.Table类的实例，再把它添加到axes实例前，你可以有更多的控制。可以使用Axes.add_table(table)方法把table实例添加axes，这里的table是matplotlib.table.Table类的实例。  

### 使用子区（subplots）  

subplot派生自Axes，位于subplot实例的规则网络中。我们将要解释和演示如何以高级的方式使用子区。  

子区的基类是matplotlib.axes.SubplotBase。子区是matplotlib.axes.Axes的实例，但提供了helper方法来生成和操作图表中一系列Axes。  

有一个名为`matplotlib.figure.SubplotParams`的类，它包括subplot的所有参数。尺寸是被归一化的图表的宽度或者高度。如果不指定任何定制化的值，subplot将会从rc参数中读取参数值。  

脚本层（matplotlib.pyplot）有操作子区的一些helper方法。  

`matplotlib.pyplot.subplots`可以方便地创建普通布局的子区，我们可以指定网格的大小——子区网格的行数和列数。  

我们可以创建共享x或者y轴的子区，这通过使用sharex或者sharey关键字参数来完成。sharex参数可以设置为True，这样x轴就被所有的子区共享。这样一来，刻度和标签只在最后一行的子区上可见。它们也可以设置为字符串，枚举值如：row、col、all或者none。值all和True相同；值none和False相同；如果设置为row，则每一个子区行共享x轴坐标；如果设置为col，则每一个子区列共享y轴坐标。  

`matplotlib.pyplot.subplots`方法返回一个(fig, ax)元组，其中ax可以是一个坐标轴实例。当创建多个子区时，ax是一个坐标轴实例的数组。  

我们用`matplotlib.pyplot.subplots_adjust`来调整子区的布局。关键字参数指定了图表中子区的坐标（left、right、bottom、top），其值是归一化的图表大小的值。可以用wspace和hspace参数指定子区间空白区域的大小，参数值为相应宽度和高度的归一化值。  

```python
import matplotlib.pyplot as plt

plt.figure(0)
axes1 = plt.subplot2grid((3, 3), (0, 0), colspan=3)
axes2 = plt.subplot2grid((3, 3), (1, 0), colspan=2)
axes3 = plt.subplot2grid((3, 3), (1, 2))
axes4 = plt.subplot2grid((3, 3), (2, 0))
axes5 = plt.subplot2grid((3, 3), (2, 1), colspan=2)

#tidy up tick labels size

plt.suptitle("Demo of subplot2grid")
plt.show()
```

结果如下图所示：  

![][pt_05]  

以下是一个以另一种方式定制化当前axes或者subplot的例子：
axes = fig.add_subplot(111)
rectangle = axes.patch
rectangle.set_facecolor('blue')

我们看到每一个axes实例包含了一个引用rectangle实例的patch字段，此字段代表当前axes实例的背景，我们可以更新该实例的属性，进而更新当前axes的背景，例如，可以改变其颜色，也可以加载一幅图像以添加水印保护；也可以先创建一个补片，然后把它添加到axes的背景上。

```python
fig = plt.figure()
axes = fig.add_subplot(111)
rect = matplotlib.patch(rect)
axes.add_patch(rect)
#We have to manually force a figure draw
axes.figure.canvas.draw()
```

### 定制化表格  

在线条或者图表下面添加网格是非常有用的，我们可以使用matplotlib.pyplot.grid来设置网格的可见度、密度和风格，或者是否显示网格。  

本节讲解如何打开或关闭网格，以及如何改变网格上的主刻度和次刻度。  

最常用的网格定制化功能可以用matplotlib.pyplot.grid辅助函数来完成。  

```python
#绘制一个线图
plt.plot([1, 2, 3, 3.5, 4, 4.3, 3])
```

结果如下图所示：  

![][pt_07]  

```python
#在线图的基础上加上默认类型的网格
plt.plot([1, 2, 3, 3.5, 4, 4.3, 3])
plt.grid()
```

结果如下图所示：  

![][pt_08]  

```python
#对网格进行设置（颜色、线条类型、线宽）
plt.plot([1, 2, 3, 3.5, 4, 4.3, 3])
plt.grid(color='g', linestyle='--', linewidth=1)
```

结果如下图所示：  

![][pt_08]  

我们可以只通过主刻度或者次刻度，或者同时通过两个刻度来操作网格。因此，函数参数which可以是'major'、'minor'或者'both'。  

所有其他属性通过kwargs参数传入，这些参数和matplotlib.lines.Line2D实例可以接受的标准属性集合相同，比如color、linestyle、linewidth。这里有一个例子，代码如下：  

```python
ax.grid(color='g', linestyle='--', linewidth=1)
```

需要更多的定制化，就需要更深入的了解matplotlib，然后找到mpl_toolkits中的AxesGrid模块。这个模块能让我们以一种更加简单且可管理的方式创建坐标轴网格。  

```python
#关于网格的应用
import numpy as np
import matplotlib.pyplot as plt
from mpl_toolkits.axes_grid1 import ImageGrid
from matplotlib.cbook import get_sample_data

def get_demo_image():
    f = get_sample_data("axes_grid/bivariate_normal.npy", asfileobj=False)
    #z is a numpy array of 15x15
    Z = np.load(f)
    return Z, (-3, 4, -4, 3)

def get_grid(fig=None, layout=None, nrows_ncols=None):
    assert fig is not None
    assert layout is not None
    assert nrows_ncols is not None
    
    grid = ImageGrid(fig, layout, nrows_ncols=nrows_ncols, axes_pad=0.05, add_all=True, label_mode='L')
    return grid

def load_images_to_grid(grid, Z, *images):
    mins, maxs = Z.min(), Z.max()
    for i, image in enumerate(images):
        axes = grid[i]
        axes.imshow(image, origin="lower", vmin=mins, vmax=maxs, interpolation='nearest')

if __name__ == "__main__":
    fig = plt.figure(1, (8, 6))
    grid = get_grid(fig, 111, (1, 3))
    Z, extent = get_demo_image()
    
    #slice image
    image1 = Z
    image2 = Z[:, :10]
    image3 = Z[:, 10:]
    
    load_images_to_grid(grid, Z, image1, image2, image3)
    
    plt.draw()
    plt.show()
```

结果如下图所示：  

![][pt_09]  

### 创建等高线图  

Z矩阵的等高线图有许多等高线表示，这里的Z被视为相对于X-Y平面的高度。Z的最小值为2，并且必须包含至少两个不同的值。  

等高线图的缺陷之一是如果在编码时不为等值线添加标签，它便毫无意义，因为我们不能分辨出最高点最低点，也无法找出局部极小值。  

对于为等高线添加标签，可以使用标签（`clabel()`）或者colormaps为等高线添加标签。如果输出媒介允许使用颜色，colormaps是首选，这会更方便观察者理解数据。  

等高线图的另一个风险是如何选择要绘制的等高线数量。  

函数`contour()`会猜测出将绘制的等高线数量，但也可以指定等高线数量。  

在matplotlib中，使用`matplotlib.pyplot.contour`绘制等高线图。  

两个相似的函数：`contour()`绘制等高线，`contourf()`绘制填充的等高线。  

`contour()`函数可以有不同的调用签名，这取决于我们拥有的数据和我们想可视化的属性。  

|调用签名|描述|
|:-------------------------------:|:---------------------------:|
|contour(Z)                       |绘制Z(数组)的等高线。自动选择水平值|
|contour(X, Y, Z)                 |绘制X、Y和Z的等高线。X和Y数组为(x, y)平面坐标(surface coordinates)|
|contour(Z, N) <br> contour(X, Y, Z, N)|绘制Z的等高线，其中水平数由N决定。自动选择水平值|
|contour(Z, V) <br> contour(X, Y, Z, V)|绘制等高线，水平值在V中指定|
|contour(..., V)                  |填充V序列中的水平值之间的len(V)-1个区域|
|contour(Z, **kwargs)             |使用关键字参数控制一般线条的属性（颜色、线宽、起点，颜色映射表（color map）等|

X、Y、Z的形状和维度存在一定的限制关系。例如：X和Y可以是二维的，与Z形状相同。如果它们是一维的，则X的长度等于Z的列数，Y的长度将等于Z的列数。  

代码如下：  

```python
import numpy as np
import matplotlib as mpl
import matplotlib.pyplot as plt

def process_signal(x, y):
    return (1 - (x ** 2 + y ** 2)) * np.exp(-y ** 3 / 3)

x = np.arange(-1.5, 1.5, 0.1)
y = np.arange(-1.5, 1.5, 0.1)

#Make grids of points
X, Y = np.meshgrid(x, y)

Z = process_signal(X, Y)

#Number of is olines
N = np.arange(-1, 1.5, 0.3)

#adding the Contour lines with labels
CS = plt.contour(Z, N, linewidths=2, cmap=mpl.cm.jet)
plt.clabel(CS, inline=True, fmt='%1.1f', fontsize=10)
plt.colorbar(CS)

plt.title('My function: $z=(1-x^2+y^2) e^{(y^3)/3}$')
plt.show()
```

结果如下图所示：  

![][pt_10]  

在对`process_signals`函数求值并将其存储在Z之后，简单的调用contour，并传入Z和等高线水平数量。  

可以尝试用`N arange()`调用中的第三个参数做个实验。尝试相同数据进行不同编码会有何差异。  

此外，传入了一个CS（matplotlib.contour.QuadContourSet实例）向图表添加了一个颜色映射表。  

### 填充图表底层区域  

在matplotlib中绘制填充多边形的基本方式是使用matplotlib.pyplot.fill。  

该方法接收与matplotlib.pyplot.plot相似的参数，即多个x、y对和其他Line2D属性。方法返回被添加的Patch实例的列表。  

除了如`histogram()`等固有的绘制闭合的、填充多边形的绘图函数之外，`matplotlib.fill_between()`和`matplotlib.pyplot.fill_betweenx()`函数。这些方法用于填充两条曲线间的多边形区域。`fill_between()`和`fill_betweenx()`主要的区别是后者填充x轴的值之间的区域，而前者填充y轴的值之间的区域。  

代码如下：  

```python
import numpy as np
import matplotlib.pyplot as plt
from math import sqrt

t = range(1000)
y = [sqrt(i) for i in t]
plt.plot(t, y, color='red', lw=2)
plt.fill_between(t, y, color='silver')
plt.show()
```

结果如下图所示：  

![][pt_11]  

另一个技巧：  

```python
import matplotlib.pyplot as plt
import numpy as np

x = np.arange(0.0, 2, 0.01)
y1 = np.sin(np.pi * x)
y2 = 1.7 * np.sin(4 * np.pi * x)

fig = plt.figure()
axes1 = fig.add_subplot(211)
axes1.plot(x, y1, x, y2, color='grey')
axes1.fill_between(x, y1, y2, where=y2<=y1, facecolor='blue', interpolate=True)
axes1.fill_between(x, y1, y2, where=y2>=y1, facecolor='gold', interpolate=True)
axes1.set_title('Blue where y2 <= y1. Gold-color where y2 >= y1.')
axes1.set_ylim(-2, 2)

#Mask values in y2 with value greater than 1.0
y2 = np.ma.masked_greater(y2, 1.0)
axes2 = fig.add_subplot(212, sharex=axes1)
axes2.plot(x, y1, x, y2, color='black')
axes2.fill_between(x, y1, y2, where=y2<=y1, facecolor='blue', interpolate=True)
axes2.fill_between(x, y1, y2, where=y2>=y1, facecolor='gold', interpolate=True)
axes2.set_title('Same as above, but mask')
axes2.set_ylim(-2, 2)
axes2.grid()

plt.show()
```

结果如下图所示：  

![][pt_12]  

首先创建了两个在某些点重叠的正弦曲线函数。  

之外，该示例创建了两个子区，用来比较两种渲染填充区域方式的差异。  

在这两种情况下，我们使用了带参数where的`fill_between()`方法填充where等于True的区域，其中where参数接受一个长度为N的布尔数组。  

下面的子区演示了`mask_greater`，它屏蔽了数组中大于给定值的所有制。这是numpy.ma中的一个方法，用来处理缺失或者无效的值。我们在底部的坐标轴上添加网格使其更加直观。  

### 绘制极线图  

为了在极坐标下显示数据，我们必须有合适的数据值。在极坐标系统中，点被描述为半径距离（通常表示为r）和角度（通常表示为theta）。角度可以用弧度或者角度表示，但是在matplotlib使用角度表示。  

我们需要告诉matplotlib坐标轴要在极坐标系统中。这通过向`add_axes`或`add_subplot`提供`polar=True`参数来完成。  

此外，为了设置图表中的其他属性，如半径网格或者角度，我们需要使用matplotlib.pyplot.rgrids()来切换半径网格的显示或者设置标签。同样，需要使用`matplotlib.pyplot.thetagrid()`来配置角度刻度和标签。  

```python
import numpy as np
import matplotlib.cm as cm
import matplotlib.pyplot as plt

figsize = 7
colormap = lambda r: cm.Set2(r / 20.)
N = 18 # number of bars

fig = plt.figure(figsize=(figsize, figsize))
ax = fig.add_axes([0.2, 0.2, 0.7, 0.7], polar=True)

theta = np.arange(0.0, 2 * np.pi, 2 * np.pi/N)
radii = 20 * np.random.rand(N)
width = np.pi / 4 * np.random.rand(N)
bars = ax.bar(theta, radii, width=width, bottom=0.0)
for r, bar in zip(radii, bars):
    bar.set_facecolor(colormap(r))
    bar.set_alpha(0.6)

plt.show()
```

结果如下图所示：  

![][pt_13]  

首先，创建一个正方形的图表，并向其添加极坐标轴。其实图表不必是正方形的，但是如果不这样的话，极线图便是椭圆形（而不是圆形）的。
然后，为角度（theta）集合极线距离（radii）生成随机值。因为绘制的是极线条，需要给定每一个极线条一个宽度集合，这就需要生成一些宽度值。因为m`atplotlib.axes.bar`接收值的数组（几乎matplotlib中所有的绘图函数都是如此），所以不必在这个生成的数据集合上做循环遍历，只需要调用bar函数并传入所有的参数即可。
为了区分每一个极线条，我们需要循环遍历添加到ax（坐标轴）的每一个极线条，并定制化其外观（表面颜色和透明度）。


转载请注明：[南梦婷的博客](https://norah2.github.io) » [点击阅读原文](https://norah2.github.io/2019/09/29/matplotlib_plot2/)   

<!--以下是本文用到的链接-->  

[pt_01]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/37_plot_seting/01.png
[pt_02]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/37_plot_seting/02.png
[pt_03]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/37_plot_seting/03.png
[pt_04]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/37_plot_seting/04.png
[pt_05]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/37_plot_seting/05.png
[pt_06]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/37_plot_seting/06.png
[pt_07]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/37_plot_seting/07.png
[pt_08]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/37_plot_seting/08.png
[pt_09]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/37_plot_seting/09.png
[pt_10]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/37_plot_seting/10.png
[pt_11]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/37_plot_seting/11.png
[pt_12]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/37_plot_seting/12.png
[pt_13]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/37_plot_seting/13.png

{% endraw %}
