---
layout: post
title: 迭代器
date: 2019-04-09
description: 相对于其他序列数据类型来说迭代器更加高效。
categories: Python
tags: Python Iter
author: NMt
mathjax: true
--- 

* content
{:toc}

在不少编程语言中都会存在一种名叫迭代器的数据类型，这种数据类型理解起来可能会会花一点时间，但是在进行遍历的时候在效率上会比常见的列表之类的数据类型效率更高。  

本篇主要讲解有关于迭代器的函数和用例。  

<div style='display: none'>
@@@@
</div>




{% raw %}
特点：  
1. 它能够用来遍历标准模板库容器中的部分或全部元素，
2. 可迭代的数据，一次只生成一个数，效率会比列表等效率更高。
3. 返回值是是一个迭代器地址，没有进行计算。  

* range(start, stop[, step])  实际上它不是一个函数，而是`range`一个不可变的序列类型，如范围和序列类型。根据传入的参数创建一个新的range对象  

参数说明：  

1. start: 计数从start开始。默认是从0开始。  
2. stop: 计数到stop结束，但不包括stop。  
3. step: 步长，默认为1。  
  
```python
range(stop)
range(start, stop[, step])
>>> a = range(10)
>>> b = range(1,10)
>>> c = range(1,10,3)
>>> a,b,c # 分别输出a,b,c
(range(0, 10), range(1, 10), range(1, 10, 3))
>>> list(a),list(b),list(c) # 分别输出a,b,c的元素
([0, 1, 2, 3, 4, 5, 6, 7, 8, 9], [1, 2, 3, 4, 5, 6, 7, 8, 9], [1, 4, 7])
>>> list(range(1, 5))
[1, 2, 3, 4]
```

* map(function, iterable, ...)  返回一个iterable，他将函数应用于每个iterable项，从而产生结果。如果传递了其他可迭代参数，则函数必须传入那么多参数，并且并行地应用于所有迭代的项。对于多个迭代，迭代器在最短地iterable耗尽时停止，对于函数输入已经安排到参数元组的情况。  

参数说明：  
1. function: 函数  
2. iterable: 一个或多个序列。  

```python
>>> a = map(ord,'abcd')
>>> a
<map object at 0x03994E50>
>>> list(a)
[97, 98, 99, 100]
>>> def square(x):
...     return x ** 2
...
>>> map(square, [1, 2, 3, 4, 5])
<map object at 0x000001A9DB3799E8>
>>> list(map(square, [1, 2, 3, 4, 5]))
[1, 4, 9, 16, 25]
```
  
**注: 在python 2.x 中返回列表，在python 3.x 中返回迭代器。**  

**注：如果传入的参数长度不匹配，则以长度最小的序列为准，截断其他序列。**   

```python
>>> def add(x, y, z):
...     return x+y+z
...
>>> list1 = [1, 2, 3]
>>> list2 = [1, 2, 3, 4]
>>> list3 = [1, 2, 3, 4, 5]
>>> res = map(add, list1, list2, list3)
>>> print(list(res))
[3, 6, 9]
```

* filter(function, iterable)  从iterable的那些元素给function构造一个迭代器，并且返回true。如果function为None，这个标识function被假定，即所有iterable中的元素都为False或被移除。  

参数说明：  
function: 判断函数。该函数只能接收一个参数，并且返回值是True或False。  
iterable: 可迭代对象。  

```python
>>> a = list(range(1,10)) #定义序列
>>> a
[1, 2, 3, 4, 5, 6, 7, 8, 9]
>>> def if_odd(x): #定义奇数判断函数
    return x%2==1
>>> list(filter(if_odd,a)) #筛选序列中的奇数
[1, 3, 5, 7, 9]
```

**注: 内置函数filter()将一个单参数函数作用到一个序列上，返回该序列中使得该函数返回值为True的那些元素组成的filter对象，如果指定函数为None，则返回序列中等价于True的元素。**  

```python
>>> seq = ['foo', 'x41', '?!', '***']
>>> def func(x):
...     return x.isalnum()
...
>>> filter(func, seq)
<filter object at 0x000001A9DB379EB8>
>>> list(filter(func, seq))
['foo', 'x41']
```

**注：**  
**1. 关于`filter()`方法，python 2.x 返回的是过滤后的列表，python3中返回的是一个filter类。**   
**filter 类实现了`__iter__`和`__next__`方法，可以看成是一个迭代器，有惰性运算的特性，相对python 2.x提升了性能，可以节约内存。**  

```python
>>> list(filter(None, [0, 1, 2, 3, 0, 0, 0]))
[1, 2, 3]
>>> list(filter(lambda x: x>2, [0, 1, 2, 3, 0, 0]))
[3]
>>> list(filter(lambda x: x%2 == 0, range(10))
... )
[0, 2, 4, 6, 8]
>>> list(filter(lambda x: len(x)>3, ['a', 'b', 'abcd']))
['abcd']
>>>
```

* zip([iterable, ...])  聚合传入的每个迭代器中相同位置的元素，返回一个新的元组类型迭代器。为来自每个迭代的元素的迭代器创建一个聚合。返回元组的迭代器，其中第i个元组包含来自每个参数序列或迭代的第i个元素。相当于：

参数说明：  
1. iterable: 一个或多个迭代器。  

返回元组列表。  

```python
def zip(*iterables):
    #zip('ABCD', 'xy') --> Ax By
    sentinel = object()
    iterators = [iter(it) for it in iterables]
    while iterators:
        result = []
        for it in iterators:
            elem = next(it, sentinel)
            if elem is sentinel:
                return
            result.append(elem)
        yield tuple(result)
#实例
>>> x = [1,2,3] #长度3
>>> y = [4,5,6,7,8] #长度5
>>> list(zip(x,y)) # 取最小长度3
[(1, 4), (2, 5), (3, 6)]
```

**注：`zip()`函数用于将可迭代的对象作为参数，将对象中对应的元素打包成一个个元组，然后返回由这些元组组成的列表。如果哥哥迭代器的元素个数不一致，则返回列表长度与最短的对象相同。利用`*`操作符，可以将元组解压为列表**  

**注: zip方法在python 3中返回的是一个对象，如需展示列表，需手动`list()`转换。**  

```python
>>> a = [1, 2, 3]
>>> b = [4, 5, 6]
>>> zipped = zip(a, b)
>>> list(zipped)
[(1, 4), (2, 5), (3, 6)]
>>> list(zip(*zipped))
[(1, 2, 3), (4, 5, 6)]
```

**注：与zip相反，*zipped可理解为解压，返回二维矩阵式。**

```python
>>> a=['name', 'aeg', 'sex']
>>> b=['Dong', 38 , 'Male']
>>> c=dict(zip(a,b))
>>> c
{'name': 'Dong', 'aeg': 38, 'sex': 'Male'}
>>> x='abcd'
>>> y='abcde'
>>> [i==j for i, j in zip(x, y)]
[True, True, True, True]
```

* enumerate(iterable, start = 0)  返回一个枚举对象。iterable必须是一个序列，一个迭代器或一些支持迭代的对象。  

参数说明：  
1. sequence: 一个序列、迭代器或其他支持迭代对象。  
2. start: 下标起始位置。  

返回enumerate(枚举)对象。  

```python
>>> seasons = ['Spring', 'Summer', 'Fall', 'Winter']
>>> list(enumerate(seasons))
[(0, 'Spring'), (1, 'Summer'), (2, 'Fall'), (3, 'Winter')]
>>> list(enumerate(seasons, start=1))
[(1, 'Spring'), (2, 'Summer'), (3, 'Fall'), (4, 'Winter')]
```

* reduce(function, iterable[, initializer])  用传给reduce中的函数function（有两个参数）先对集合中的第1、2个元素进行操作，得到的结果在与第三个数据用function函数运算，最后得到一个结果。  

参数说明：  
1. function: 函数，传入两个参数。  
2. iterable: 可迭代对象。  
3. initializer: 可选，初始参数。  

返回函数的计算结果。

**注：在python3中，`reduce()`函数已经被从全局名字空间里移除了，他现在被放置在functools模块里，如果想要使用它，则需要通过引入functoools模块来调用`reduce()`函数**  

```python
>>> from functools import reduce
>>> reduce(lambda x, y: sum([x, y]), range(1, 101))
5050
>>> map(lambda x, y: sum([x, y]), range(1, 101))
<map object at 0x000001A9DB379A20>
>>> print(map(lambda x, y: sum([x, y]), range(1, 101)))
<map object at 0x000001A9DB3799E8>
>>> list(map(lambda x, y: sum([x, y]), range(1, 101)))
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: <lambda>() missing 1 required positional argument: 'y'
>>> list(map(lambda x, y: sum([x, y]), range(1, 101), range(1, 101)))
[2, 4, 6, 8, 10, 12, 14, 16, 18, 20, 22, 24, 26, 28, 30, 32, 34, 36, 38, 40, 42, 44, 46, 48, 50, 52, 54, 56, 58, 60, 62, 64, 66, 68, 70, 72, 74, 76, 78, 80, 82, 84, 86, 88, 90, 92, 94, 96, 98, 100, 102, 104, 106, 108, 110, 112, 114, 116, 118, 120, 122, 124, 126, 128, 130, 132, 134, 136, 138, 140, 142, 144, 146, 148, 150, 152, 154, 156, 158, 160, 162, 164, 166, 168, 170, 172, 174, 176, 178, 180, 182, 184, 186, 188, 190, 192, 194, 196, 198, 200]
```


转载请注明：[南梦婷的博客](https://norah2.github.io) » [点击阅读原文](https://norah2.github.io/2019/04/09/iteration/) 

{% endraw %}