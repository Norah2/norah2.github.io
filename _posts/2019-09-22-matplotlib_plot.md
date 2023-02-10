---
layout: post
title: 利用matplotlib库画图  
date: 2019-09-22
description: 利用matplotlib库画基础的柱状图、线型图、堆积柱状图、饼图、直方图、填充区域图表、堆积图、带彩色标记的散点图等等，还记录了有关图表的线型、属性、格式化字符的参数值以及说明；同时还讲解了怎么移动轴线、添加误差线、添加图例、刻度、刻度标签、网格等等部件。  
categories: Visualize
tags: Python Visualize Matplotlib
author: NMt
mathjax: true
---

* content
{:toc}

利用matplotlib库画基础的柱状图、线型图、堆积柱状图、饼图、直方图、填充区域图表、堆积图、带彩色标记的散点图等等，还记录了有关图表的线型、属性、格式化字符的参数值以及说明；同时还讲解了怎么移动轴线、添加误差线、添加图例、刻度、刻度标签、网格等等部件。  

<div style='display: none'>
@@@@
</div>




{% raw %}
可视化中MatLab做的非常出色，Python也做了一个类似于MatLab的库matplotlib。本篇博客主要是针对于matplotlib.pyplot库中的常用图表入手。  

注：本片博客是看《Python数据可视化编程实战(第2版)》这本书第三章的总结。  

### 柱状图、线型图和堆积柱状图  

首先导入本篇博客中所有代码所依赖的库，后续代码中都不会有导入库的操作：  

```python
import matplotlib.pyplot as plt
import pandas as pd
import numpy as np
from pylab import *
import datetime
import matplotlib as mpl
```

matplotlib中的基本图表包括下列元素：  

1. x轴与y轴：水平和垂直的轴线；  
2. x轴与y轴刻度：刻度表示坐标轴的分隔，包括最小刻度和最大刻度；  
3. x轴与y轴刻度标签：表示特定坐标轴的值；  
4. 绘制区域：实际绘图的区域。  

下面是一个简单线图的代码：

```python
plt.plot([1, 2, 3, 2, 3, 2, 2, 1])
```

结果如下图所示：  

![][pt_01]  

在改图的基础上再增加一条线：  

```python
plt.plot([1, 2, 3, 2, 3, 2, 2, 1])
plt.plot([4, 3, 2, 1], [1, 2, 3, 4])
```

结果如下图所示：  

![][pt_02]  

我们可以看到该图形的x轴与y轴的取值范围有所变化，为了适应新的线，y轴的取值由原来的[1, 3]变成了[1, 4]。  

接下来生成了多个图表进行对比：  

```python
x = [1, 2, 3, 4]
y = [5, 4, 3, 2]

plt.subplot(231)
plt.plot(x, y)

plt.subplot(232)
plt.bar(x, y)

plt.subplot(233)
plt.barh(x, y)

plt.subplot(234)
plt.bar(x, y)
y1 = [7, 8, 5, 3]
plt.bar(x, y1, bottom=y, color='r')

plt.subplot(235)
plt.boxplot(x)

plt.subplot(236)
plt.scatter(x, y)

plt.show()
```

结果如下图所示：  

![][pt_03]  

其中，`subplot()`函数用来将一个图片分割为多个块，在不同块上放置不同的图表；`bar()`方法用来绘制柱形图；`barh`方法用来绘制条形图；`bar()`中的bottom参数可以用来绘制堆叠图；`boxplot()`用来绘制箱线图；`scatter()`用来绘制散点图。  

工作原理：  

通过调用`figure()`方法，创建一个新的图表。如果给该方法提供一个字符串参数，例如“sample charts”，这个字符串就会成为窗口的后台标题。如果通过相同的参数（也可以是数字）调用`figure()`方法将会激活相应的图表，并且接下来的汇入操作都在此图表中进行。  

下面的代码对比了箱线图与柱状图的差异：  

```python
dataset = [113, 115, 119, 121, 124, 
           124, 125, 126, 126, 126, 
           127, 127, 128, 129, 130, 
           130, 131, 132, 133, 136]

plt.subplot(121)
plt.boxplot(dataset, vert=False)

plt.subplot(122)
plt.hist(dataset)

plt.show()
```

结果如下图所示：  

![][pt_15]  

### 简单的正弦图与余弦图  

首先，计算从$-\pi$到$\pi$之间具有相同线性距离的256个点的正弦值与余弦值，然后把$sin(x)$与$cos(x)$值在同一个表中表现出来，代码如下：  

```python
x = np.linspace(-np.pi, np.pi, 256, endpoint=True)
y = np.cos(x)
y1 = np.sin(x)
plt.plot(x, y)
plt.plot(x, y1)
plt.show()
```

结果如下图所示：  

![][pt_18]  

在该图的基础上增加其他的部件使得图变得更加完善：  

```python
x = np.linspace(-np.pi, np.pi, 256, endpoint=True)
y = np.cos(x)

y1 = np.sin(x)

plt.plot(x, y)

plt.plot(x, y1)

plt.title("Function $sin$ and $cos$")

plt.xlim(-3.0, 3.0)
plt.ylim(-1.0, 1.0)

plt.xticks([-np.pi, -np.pi/2, 0, np.pi/2, np.pi], 
       [r'$-\pi$', r'$-\pi/2$', r'$0$', r'$+\pi/2$', r'$+\pi$'])
plt.yticks([-1, 0, +1], 
       [r'$-1$', r'$0$', r'$+1$'])
plt.show()
```

结果如下图所示：  

![][pt_04]  

### 设置坐标轴的长度和范围  

```python
axis()

[out]: 
(0.0, 1.0, 0.0, 1.0)
```

除了上述的元组之外还有一个坐标白图出现。  

这里的`(0.0, 1.0, 0.0, 1.0)`四个值，分别代表着xmin、xmax、ymin、ymax。可以设置x轴和y轴的值。  

```python
l = [-1, 1, -10, 10]
axis(l)

[out]: 
[-1, 1, -10, 10]
```

同时也会出现一个坐标白图，但是这时的图上的坐标轴范围则是[-1, 1]以及[-10, 10]。  

再次说明一下，在交互模式下这个操作会更新同一个图形。而且，也可以通过关键字参数(**kwargs)单独更新某一个参数值，例如仅把xmax设置为某个值。  

工作原理：  

matplotlib在画图时axis参数会自动调整至最小值，同时确保能够显示所有的数据点。若是设置的axis参数的范围小于真实数据中的范围，则有点数据无法显示。避免这种情况发生的方法之一就是调用`autoscale()(matplotlib.pyplot.autoscale())`方法，该方法会计算坐标轴的最佳大小以适应数据的显示。  

如果想要向相同图形添加新的坐标轴，可以调用`matplotlib.pyplot.axes()`方法。我们通常会在方法中传入一些属性，例如rect，归一化单位(0, 1)下的left、bottom、width、height 4个属性，或者axisbg，该参数指定坐标轴的背景颜色。  

还有其他一些参数允许我们对新添加的坐标轴进行设置，如sharex/sharey参数，该参数接受其他坐标轴的值并让当前坐标轴(x/y)共享相同的值；或者polar参数，指定是否使用极坐标轴(polar axes)。  

如果只想对当前图形添加一条线，可以调用`matplotlib.pyplot.axhline()`或者`matplotlib.pyplot.axvline()`。`axhline()`和`axvline()`方法会根据给定的x和y值相应的绘制出相对于坐标轴的水平线和垂直线。这两个方法的参数很相似，`axhline()`方法比较重要的参数是y向位置、xmin和xmax，`axvline()`方法比较重要的参数x向位置、ymin、ymax。  

使用示例代码如下：  

```python
axhline()
axvline()
axhline(4)
axis([-1, 1, -0.5, 5])

[out]: [-1, 1, -0.5, 5]
```

结果如下图所示：  

![][pt_16]  

图形中的网格属性默认是关闭的，但打开和定制化很简单。不带参数调用`matplotlib.pyplot.grid()`会切换网格的显示状态。另外一些控制参数如下：  

1. witch：指定绘制的网格刻度类型(major、minor or both)；  
2. axis：指定绘制哪组网格线(both、x or y)。  

坐标轴通常由`matplotlib.pyplot.axis()`控制。坐标轴在内部实现上由几个Python类来表示。其中的一个父类是`matplotlib.axis.Axis`类来表示，`matplotlib.axis.XAxis`表示x轴，`matplotlib.axis.YAxis`表示y轴。  

### 设置图表的线型、属性和格式化字符串  

演示如何改变线的各种属性，这会使图形更加美观新颖，并且可根据要表达的信息以及目标受众来进行更改线条以及图形的属性。  

#### 改变线的属性  

* 在方法中传入属性  

```python
plt.plot(x, y, linewidth=5)

[out]: [<matplotlib.lines.Line2D at 0x1b6074bc7b8>]
```

* 在返回的实例用setter方法来设置不同的属性  

```python
line, = plt.plot(x, y)
line.set_linewidth(5)
```

* 使用MATLAB的人更习惯使用`setp()`方法  

```python
lines = plt.plot(x, y)
plt.setp(lines, 'linewidth', 5)

[out]: [None]
```

* `setp()`方法的另一种使用方式  

```python
plt.setp(lines, linewidth=5)

[out]: [None]
```

这四种方法最终得到的结果如下图所示：  

![][pt_17]  

用来改变线条的所有属性都包含在matplotlib.lines.Line2D类中，下面的表中列举了一些属性：  

|属性|类型|描述|
|:--------------------:|:-----------------------------:|:---------------:|
|alpha                 |浮点值                          |alpha用来设置混色，并不是所有后端都支持|
|color or c            |任意matplotlib颜色              |设置线条颜色|
|dashes                |以点为单位的on/off序列           |设置破折号序列，如果seq=[None, None]，linestyle将被设置为solid|
|label                 |任意字符串                      |为图例设置标签值|
|linestyle or ls       |'-'、'--'、'-.'、':'、'steps'...|设置线条风格(也接受drawstyles的值)|
|linewidth or lw       |以点为单位的浮点值               |设置以点为单位的线宽|
|marker                |'-'、'--'等（后面有详细的说明）   |设置线条标记|
|markeredgecolor or mec|任意matplotlib颜色              |设置标记的边缘颜色|
|markeredgewidth or mew|以点为单位的浮点值               |设置以点为单位的标记边缘宽度|
|markerfacecolor or mfc|任意matplotlib颜色              |设置标记的颜色|
|markersize or ms      |浮点值                          |设置以点为单位的标记大小|
|solid_capstyle        |'butt'、'round'、'projecting'   |设置实线的线段风格|
|solid_joinstyle       |'miter'、'round'、'bevel'       |设置实线的连接风格|
|visible               |True、False                     |显示或隐藏artist|
|xdata                 |np.array                        |设置x的np.array值|
|ydata                 |np.array                        |设置y的np.array值|
|Zorder                |任意数字                         |为artist设置z轴顺序，低Zorder的artist会先绘制；如果在屏幕上x轴水平向右，y轴垂直向上，那么z轴将指向观察者。这样，0表示在屏幕上，1表示上面的一层，以此类推|

线条风格说明：  

|线条风格|描述|
|:------------------:|:--:|
|'-'                 |实线|
|'--'                |破折线|
|'-.'                |点划线|
|':'                 |虚线|
|'None' or ' ' or '' |什么都不画|

线条标记说明：  

|标记|描述|
|:------------------:|:--:|
|'o'                 |圆圈|
|'D'                 |菱形|
|'h'                 |六边形1|
|'H'                 |六边形2|
|'_'                 |水平线|
|'' or 'None' or None|无|
|'8'                 |八边形|
|'p'                 |五边形|
|','                 |像素|
|'+'                 |加号|
|'.'                 |点|
|'s'                 |正方形|
|'*'                 |星号|
|'d'                 |小菱形|
|'v'                 |一角朝下的三角形|
|'<'                 |一角朝左的三角形|
|'>'                 |一角朝右的三角形|
|'^'                 |一角朝上的三角形|
|'\|'                |竖线|
|'x'                 |X|

#### 颜色  

可以通过调用`martplotlib.pyplot.colors()`得到matplotlib支持的所有颜色，如下表所示：  

|别名|颜色|
|:--:|:--:|
|B   |蓝色|
|R   |红色|
|C   |青色|
|M   |洋红色|
|G   |绿色|
|Y   |黄色|
|K   |黑色|
|W   |白色|

#### 背景色  

通过向如`matplotlib.pyplot.axes()`或者`matplotlib.pyplot.subplot()`这样的方法体哦那个一个axisbg参数，我们可以指定坐标轴的背景色。  

```python
subplot(111, axisbg=(0.1843, 0.3098, 0.3098))
```

### 设置刻度、刻度标签和网络  

接下来记录如何设置坐标轴和线条属性，并向图形和图表中添加更多的数据。  

刻度是图形的一部分，由刻度定位器(tick locator)——指定刻度所在的位置、刻度格式器(minor formatter)——指定刻度显示的样式两部分组成。刻度由主刻度(major ticks)和次刻度(minor ticks)，默认不显示次刻度。更重要的是，主刻度和次刻度可以被独立地指定位置和格式化。  

我们可以使用`matplotlib.pyplot.locator_params()`方法控制刻度定位器的行为。尽管刻度位置通常会被自行设置，我们仍然可以控制刻度的数目，并且可以在图形比较小的时候使用紧凑视图(tight view)。  

```python
ax = plt.gca()

ax.locator_params(tight=True, nbins=10)
ax.plot(np.random.normal(10, 0.1, 100))
plt.show()
```

结果如下图所示：  

![][pt_05]  

我们也可以用locator类完成相同的设置。下面代码将主定位器设置为10的倍数。  

```python
ax = plt.gca()

ax.locator_params(tight=True, nbins=10)
ax.plot(np.random.normal(10, 0.1, 100))
ax.xaxis.set_major_locator(matplotlib.ticker.MultipleLocator(10))

plt.show()
```

结果如下图所示：  

![][pt_19]  

刻度格式器的配置非常简单。格式器规定了值（通常是数字）的显示方式。例如，用matplotlib.ticker.FormatStrFormatter可以方便地指定'%2.1f'或者'%1.1f cm'的格式字符串作为刻度标签。  

然后我们可以用`matplotlib.dates.date2num()`、`matplotlib.dates.num2date()`和`matplotlib.dates.drange()`这样的helper方法对日期进行不同形式的转换。  

```python
fig = plt.figure()

start = datetime.datetime(2013, 1, 1)
stop = datetime.datetime(2013, 12, 31)
delta = datetime.timedelta(days=1)

dates = mpl.dates.drange(start, stop, delta)
values = np.random.rand(len(dates))
ax = plt.gca()
ax.plot_date(dates, values, linestyle='-', marker='')
date_format = mpl.dates.DateFormatter('%Y-%m-%d')
ax.xaxis.set_major_formatter(date_format)
fig.autofmt_xdate()

plt.show()
```

结果如下图所示：  

![][pt_06]  

### 添加图例和注解  

```python
x1 = np.random.normal(30, 3, 100)
x2 = np.random.normal(20, 2, 100)
x3 = np.random.normal(10, 3, 100)

plt.plot(x1, label='plot')
plt.plot(x2, label='2nd plot')
plt.plot(x3, label='last plot')

plt.legend(bbox_to_anchor=(0., 1.02, 1., 0.102), loc=3, 
           ncol=3, mode="expand", borderaxespad=0.)

plt.annotate("Important value", (55, 20), xycoords='data', 
             xytext=(5, 28), arrowprops=dict(arrowstyle='->'))
plt.show()
```

结果如下图所示：  

![][pt_07]  

loc位置参数：  

|字符串|数值|
|:----------:|:--:|
|best        |0|
|upper right |1|
|upper left  |2|
|lower right |3|
|lower left  |4|
|right       |5|
|center left |6|
|center right|7|
|lower center|8|
|upper center|9|
|center      |10|

如果不想在图例中显示标签，可以将标签设置为`_nolegend_`。  

<!--
...
-->

### 移动轴线到图中央  

轴线定义了数据区域的边界，把坐标轴刻度标记连接起来。一共有4个轴线，它们可以放置在任何位置。默认情况下，它们被放置在坐标轴的边界，因此我们会看到数据图表有一个框。  

```python
plt.plot(x, y)
ax = plt.gca()

ax.spines['right'].set_color('none')
ax.spines['top'].set_color('none')
ax.spines['bottom'].set_position(('data', 0))
ax.spines['left'].set_position(('data', 0))
plt.show()
```

结果如下图所示：  

![][pt_08]  

另外，轴线可以被限制在数据结束的地方结束，例如通过调用`set_smart_bounds(True)`。在这种情况下，matplotlib会尝试以一种复杂的方式设置边界，例如处理颠倒的界限，或者在数据延伸出视图的情况下裁剪线条以适应视图。  

### 绘制直方图  

可调用`matplotlib.pyploy.hist()`来创建直方图，我们需要传入一些参数，下面是一些比较重要的参数。  

* bins: 可以是一个bin数量的整数值，也可以是表示bin的一个序列。默认值为10。  
* range: bin的范围，当bins参数为序列时，此参数无效。范围外的值将被忽略掉，默认值为None。  
* normed: 如果值为True，直方图的值将进行归一化(normalized)处理，形成概率密度。默认值为False。  
* histtype: 默认为bar类型的直方图。其他选项如下：  
   * barstacked: 用于多种数据的堆叠直方图；  
   * step: 创建未填充的线形图；  
   * stepfilled: 创建默认填充的线形图。  
* align: 用于bin边界之间矩形条的居中设置。默认值为mid，其他值为left和right。  
* color: 指定直方图的颜色，可以是单一颜色值或者颜色的序列。如果指定了多个数据集合，颜色序列将会设置为相同的顺序，如果未指定，将会使用一个默认的线条颜色。  
* orientation: 通过设置orientation为horizontal创建水平直方图。默认值为vertical。  

```python
mu = 100
sigma = 15
x = np.random.normal(mu, sigma, 10000)

ax = plt.gca()

ax.hist(x, bins=35, color='r')

ax.set_xlabel('Values')
ax.set_ylabel('Frequency')
ax.set_title(r'$\mathrm{Histogram:}\ \mu=%d,\ \sigma=%d$'%(mu, sigma))
plt.show()
```

结果如下图所示：  

![][pt_09]  

### 绘制误差条形图  

两个必选参数：left和height。  

可选参数：  

* width: 给定误差条的宽度，默认值为0.8。  
* bottom: 如果指定了bottom，其值会被加到高度中，默认值为None。  
* edgecolor: 给定误差条边界颜色。  
* linewidth: 误差条边界宽度，可以设为None(默认值)和0(此时误差条边界将不显示出来)。  
* orientation: 有vertical和horizontal两个值。  
* xerr和yerr: 用于在柱状图上生成误差条。  

```python
x = np.arange(0, 10, 1)
y = np.log(x)
xe = 0.1 * np.abs(np.random.randn(len(y)))

plt.bar(x, y, yerr=xe, width=0.4, align='center', ecolor='r', 
        color='cyan', label='experiment #1')

plt.xlabel('# measurement')
plt.ylabel('Measured values')
plt.title('Measurements')
plt.legend(loc='upper left')
plt.show()
```

结果如下图所示：  

![][pt_10]  

黑白图可用阴影线：  

|阴影线的值|描述|
|:--:|:--:|
|/   |斜线|
|\\  |反斜线|
|\|  |垂直线|
|-   |水平线|
|+   |十字线|
|x   |交叉线|
|o   |小圆圈|
|0   |大圆圈|
|.   |点|
|*   |星号|

### 绘制饼图  

```python
plt.figure(1, figsize=(6, 6))
ax = axes([0.1, 0.1, 0.8, 0.8])

labels = 'Spring', 'Summer', 'Autumn', 'Winter'
x = [15, 30, 45, 10]

explode = (0.1, 0.1, 0.1, 0.1)
plt.pie(x, explode=explode, labels=labels, 
        autopct='%1.1f%%', startangle=67)

plt.title('Rainy days by season')
plt.show()
```

结果如下图所示：  

![][pt_11]  

如果没有指定startangle，扇区将从x轴（角度0）开始逆时针排列；如果制定startangle的值为90，饼图将从y轴开始。  

### 绘制带填充区域的图表  

```python
x = np.arange(0.0, 2, 0.01)
y1 = np.sin(2*np.pi*x)
y2 = 1.2 * np.sin(4*np.pi*x)

fig = plt.figure()
ax = plt.gca()

ax.plot(x, y1, x, y2, color='black')
ax.fill_between(x, y1, y2, where=y2>=y1, facecolor='darkblue', interpolate=True)
ax.fill_between(x, y1, y2, where=y2<=y1, facecolor='deeppink', interpolate=True)

ax.set_title('filled between')
plt.show()
```

结果如下图所示：  

![][pt_12]  

在生成预定义间隔的随即信息之后，用常规的`plot()`方法绘制出这两个信号的图形。然后，调用`fill_between()`并传入所需的必选参数。  

用where参数制定一个条件来填充曲线，where参数接受布尔值（可以是表达式），这样就只会填充满足where条件的区域。  

### 绘制堆积图  

```python
df = pd.read_csv('ch03-energy-production.csv')

columns = ['Coal', 'Natural Gas (Dry)', 'Crude Oil', 'Nuclear Electric Power', 
           'Biomass Energy', 'Geothermal Energy', 'Solar/PV Energy']

colors = ['darkslategray', 'powderblue', 'darkmagenta', 'lightgreen', 'sienna', 
          'royalblue', 'mistyrose', 'lavender', 'tomato', 'gold']
plt.figure(figsize=(12, 8))
polys = plt.stackplot(df['Year'], df[columns].values.T, colors=colors)
rectangles = []
for poly in polys:
    rectangles.append(plt.Rectangle((0, 0), 1, 1, fc=poly.get_facecolor()[0]))
    legend = plt.legend(rectangles, columns, loc=3)
    frame = legend.get_frame()
    frame.set_color('White')

plt.title('Primary Energy Product by Source', fontsize=16)
plt.xlabel('Year', fontsize=16)
plt.ylabel('Production(Quad BUT)', fontsize=16)
plt.xticks(fontsize=16)
plt.yticks(fontsize=16)
plt.xlim(1973, 2014)

plt.show()
```

结果如下图所示：  

![][pt_13]  

`stackplot()`方法和`plot()`方法很相似，只不过它的第二个参数是一个多维数组。堆积图目前还不支持图例。因此可使用矩形作为第一个参数，每种能源类型对应的名字作为第二个参数，第三个参数用来指定图例的位置。设置图例背景为白色。做法是首先通过图例对象的`get_frame()`方法获得图框对象，然后调用其`set_color()`方法设置颜色。  

### 绘制带彩色标记的散点图  

```python
x = np.random.randn(1000)
y1 = np.random.randn(len(x))

y2 = 1.2 + np.exp(x)

ax1 = plt.subplot(121)
plt.scatter(x, y1, color='indigo', alpha=0.3, edgecolors='white', label='no correl')
plt.xlabel('no correlation')
plt.grid(True)
plt.legend()

ax2 = plt.subplot(122)
plt.scatter(x, y2, color='green', alpha=0.3, edgecolors='grey', label='correl')
plt.xlabel('stronge correlarion')
plt.grid(True)
plt.legend()

plt.show()
```
结果如下图所示：  

![][pt_14]  

终于写完了，敲键盘真的会手疼！！敲这个就能花掉一天时间，希望我没有在做无用功吧。  

转载请注明：[南梦婷的博客](https://norah2.github.io) » [点击阅读原文](https://norah2.github.io/2019/09/22/matplotlib_plot/)   

<!--以下是本文用到的链接-->  

[pt_01]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/36_plot_charts/01.png
[pt_02]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/36_plot_charts/02.png
[pt_03]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/36_plot_charts/03.png
[pt_04]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/36_plot_charts/04.png
[pt_05]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/36_plot_charts/05.png
[pt_06]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/36_plot_charts/06.png
[pt_07]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/36_plot_charts/07.png
[pt_08]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/36_plot_charts/08.png
[pt_09]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/36_plot_charts/09.png
[pt_10]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/36_plot_charts/10.png
[pt_11]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/36_plot_charts/11.png
[pt_12]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/36_plot_charts/12.png
[pt_13]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/36_plot_charts/13.png
[pt_14]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/36_plot_charts/14.png
[pt_15]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/36_plot_charts/15.png
[pt_16]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/36_plot_charts/16.png
[pt_17]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/36_plot_charts/17.png
[pt_18]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/36_plot_charts/18.png
[pt_19]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/36_plot_charts/19.png

{% endraw %}
