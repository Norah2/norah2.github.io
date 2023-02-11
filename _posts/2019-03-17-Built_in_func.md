---
layout: post
title: Python Built-in Function ——内置函数总结
date: 2019-03-17
description: 一份关于python内置函数的详细文档说明
categories: Python
tags: Python Func  
author: NMt
mathjax: true
---

* content
{:toc}

今天来总结一下python的内置函数，一共有69个内置函数（问我怎么知道具体数字的？数的呗！！），基于python 3.x的编译环境。我预感到写这篇博客又是一场腥风血雨，不过我准备好了，来吧！！！  

<div style='display: none'>
@@@@
</div>





{% raw %}
![built-in function][func]

### 输入输出函数  
* print(* objects，sep =''，end ='\ n'，file = sys.stdout，flush = False)  将对象打印到文本流文件，以sep分隔，然后结束。sep，end，file和flush必须作为关键字参数给出。  

```python
>>> print('Hello World!')
Hello World!
>>> print('Hello','world')  # 默认将参数以空格为间隔输出  
Hello world
>>> print('Hello');print('World!')  # 默认结尾为换行符\n
Hello
World!
>>> print('Hello',end='');print('world')  # 改变结尾的符号为空
Helloworld
>>> print('Hello','world',sep = '/')  # 改变间隔符为/
Hello/world
```

* input([ 提示] )  如果存在prompt参数，则将其写入标准输出而不带尾随换行符。然后，该函数从输入中读取一行，将其转换为字符串（剥离尾部换行符），然后返回该行。读取EOF时，会引发`EOFError`  

```python
>>> s = input('-->')
-->Monty Python's Flying Circus
>>> s
"Monty Python's Flying Circus"
```

### 运算类函数  
* abs(x) 返回数字的绝对值。参数可以是整数或浮点数。如果参数是一个复数，则返回其大小。 

```python
>>> abs(-2)
2
```

* max() 返回可迭代对象中的元素中的最大值或者所有参数的最大值  

```python
max(iterable, *[, key, default])
max(arg1, arg2, *args[, key])
>>> max(1, 2)
2
>>> max(2, 5, 4, 6)
6
>>> max([1, 2, 3, 0, 9, 5, 4])
9
```

* min()  返回可迭代中的最小项或两个或多个参数中的最小项。  

```python
min(iterable，* [，key，default ])  
min(arg1，arg2，* args [，key ])
>>> min(2, 3)
2
>>> min(2, 3, 4, 5)
2
>>> min([2, 3, 4, 5])
2
>>>
```

* round(x)  对浮点数进行四舍五入  

```python
round(x, [, n])  # 对参数x的第n+1位小数进行四舍五入，返回一个小数位数为n的浮点数。n的默认值是0.
>>> round(4, 6)
4
>>> round(0.5)
0
>>> round(1.5)
2
```

**注：round(0.5)以及round(-0.5)都是0，这是因为大多数小数部分不能完全表示为浮点数。**  

* pow(x, y[, z]) 如果z不存在，则返回x的y次方，等同于`x**y`; 如果z存在，则返回x的y次方且对z取余，等同于`(x**y)%z`  

```python
>>> pow(2, 5)
32
>>> pow(2, 3, 5)
3
>>> pow(2, 3, 2)
0
```

* sum(iterable [, start])  从左到右开始和可迭代的项目并返回总数，开始默认为0，迭代的参数通常是数字，起始值不能是一个字符串   

```python
>>> sum(2,3)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'int' object is not iterable
>>> sum([2, 3, 5, 6])
16
```

**对于某些用例，有很好的方案替代sum()。连接字符串序列的首选快速方法是调用 ''.join(sequence)。要以扩展精度添加浮点值，请参阅math.fsum()。要连接一系列迭代，请考虑使用 itertools.chain()。**

* divmod(a，b)  返回一个元组，第一个数是`a // b`，第二个数是`a % b`  

```python
>>> divmod(4, 9)
(0, 4)
>>> divmod(10, 3)
(3, 1)
```

### 文件操作  

* open(file，mode ='r'，buffering = -1，encoding = None，errors = None，newline = None，closefd = True，opener = None)  打开文件并返回相应的文件对象，如果无法打开文件，则引发OSError。 

file是一个类似路径的对象，给出要打开文件的路径名（绝对或相对于当前工作目录）  


mode是一个可选字符串，用于指定打开文件的模式。它默认‘r’为在文本模式下打开以进行读取。其他常见值‘w’用于写入（如果文件已存在则截断文件），‘x’创建，‘a’附加。  


|字符|含义|
|:--:|:--:|
|'r'|开放阅读（默认）|
|'w'|打开写入，先截断文件|
|'x'|打开以进行独占创建，如果文件已存在则失败|
|'a'|打开以进行写入，如果存在则附加到文件的末尾|
|'b'|二进制模式|
|'t'|文字模式（默认）|
|'+'|打开磁盘文件进行更新（读写）|

### 反射操作  

* __import__ 动态导入模块  

```python
>>> time.sleep(1)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
NameError: name 'time' is not defined
>>> time = __import__('time')
>>> time.sleep(1)
```

* isinstance：判断对象是否是类或者类型元组中任意类元素的实例  

```python
>>> isinstance(1,int)
True
>>> isinstance(1,str)
False
>>> isinstance(1,(int,str))
True
```

* issubclass：判断类是否是另外一个类或者类型元组中任意类元素的子类  

```python
>>> issubclass(bool,int)
True
>>> issubclass(bool,str)
False
>>> issubclass(bool,(str,int))
True
```

* setattr(object, name, values)  这是对应的getattr()。参数是一个对象，一个字符串和一个任意值。该字符串可以民航名现有属性或新属性。  

```python
setattr(x, 'foobar', 123)
x.foobar = 123
```

* delattr(object, name)  参数是一个对象和一个字符串。该字符串必须是对象属性之一的名称。如果对象允许，该函数将删除命名属性。  

```python
delattr(x, 'foobar')
del x.boobar
```

* hasattr(object, name)  参数是一个对象和字符串。如果字符串是对象属性之一的名称，则结果是True；如果不是则返回False。  

* getattr(object, name[, default])  返回object的name属性的值。name必须是一个字符串。如果字符串是对象属性之一的名称，则结果是该属性的值。如果name属性不存在，则返回default（如果提供），否则`getattr(x, 'foobar')`引发`AttributeError`错误。  

* callable：检测对象是否可被调用  

```python
>>> class B:
...     def __call__(self):
...         print('instances are callable now.')
...
>>> callable(B)   # 类B时刻调用对象
True
>>> b = B()    # 调用类B
>>> callable(b)    # 实例b是可调用对象
True   
>>> b()       # 调用实例b成功
instances are callable now.
```  

* breakpoint(* args，** kws)  此函数会将您至于调用站点的调试器中，具体来说，它呼叫sys.breakpointhook()，传递args和kws直接通过。默认情况下，sys.breakpointhook()调用pdb.set_trace()不需要参数。在这种情况下，它纯粹是一个便利功能，因此不必显示写入pdb或输入尽可能多的代码来进入调试器，但是sys.breakpointhook()可以设置为其他一些功能并breakpoint()自动调用它，允许进入选择的调试器。注意以下几点：  
   1. breakpoint 中的 hook 变量将引用 sys.breakpointhook 函数对象；
   2. breakpoint 会将自己所有的实参都传递给 sys.breakpointhook()；
   3. 如果 sys.breakpointhook 缺失，则会抛出 RuntimeError

```python
#In builtins.
def breakpoint(*args, **kws):
    import sys
    missing = object()
    #设置钩子函数
    hook = getattr(sys, 'breakpointhook', missing)
    if hook is missing:
        raise RuntimeError('lost sys.breakpointhook')
    #返回钩子函数的调用
    return hook(*args, **kws)
```

### 数据类型（创建与转换）  

* bool([x]) 返回一个bool值，即`True`或`Flase`之一。x为false或者省略则返回false；其他则返回True。bool类是int的一个子类。它不能继续被分成子类。它唯一的例子是False和True。  

```python
>>> bool()
False
>>> bool(0)
False
>>> bool(1)
True
```

* int 返回由数字或字符串x构造的整数对象。

```python
int([x]) 
int(x, base = 10)
>>> int(1)
1
>>> int('1')
1
>>> int('100', base=11)
121
>>> int('100', base=12)
144
>>> int('10', base=12)
12
```

* float([x])  返回由数字或字符串x构造的浮点数，如果参数是一个字符串，它应该包含一个十进制数字，可选地以符号开头，并且可选地嵌入在空格中。可选标志可以是'+'或'-'；再删除前导和尾随空格字符后，输入必须符合一下语法：

|意义|符号|
|:--:|:--:|
|sign|'+'/'-'|
|infinity|'Infinity'/'inf'|
|nan|'nan'|
|numerric_value| floatnumber/ infinity|
|numeric_string | nansignnumeric_value|

```python
>>> float('+1.23')
1.23
>>> float('   -12345\n')
-12345.0
>>> float('1e-003')
0.001
>>> float('+1E6')
1000000.0
>>> float('-Infinity')
-inf
```

* enumerate(iterable, start = 0)  返回一个枚举对象。iterable必须是一个序列，一个迭代器或一些支持迭代的对象。
```python
>>> seasons = ['Spring', 'Summer', 'Fall', 'Winter']
>>> list(enumerate(seasons))
[(0, 'Spring'), (1, 'Summer'), (2, 'Fall'), (3, 'Winter')]
>>> list(enumerate(seasons, start=1))
[(1, 'Spring'), (2, 'Summer'), (3, 'Fall'), (4, 'Winter')]
```

* tuple([iterable])  实际上它不是一个函数，而是一个不可变的序列类型  

```python
>>> tuple() #不传入参数，创建空元组
()
>>> tuple('121') #传入可迭代对象。使用其元素创建新的元组
('1', '2', '1')
```

* dict()  创建一个新的字典，该对象是dict类  
```python
>>> dict() # 不传入任何参数时，返回空字典。
{}
>>> dict(a = 1,b = 2) #  可以传入键值对创建字典。
{'b': 2, 'a': 1}
>>> dict(zip(['a','b'],[1,2])) # 可以传入映射函数创建字典。
{'b': 2, 'a': 1}
>>> dict((('a',1),('b',2))) # 可以传入可迭代对象创建字典。
{'b': 2, 'a': 1}
```

* str(object='', object=b'', encoding='utf-8', errors='strict')  返回一个对象str版本。  

```python
>>> str()
''
>>> str(None)
'None'
>>> str('abc')
'abc'
>>> str(132)
'132'
```

* list([iterable])  实际上，它不是一个函数，而是一个可变序列类型list。

```python
>>>list() # 不传入参数，创建空列表
[] 
>>> list('abcd') # 传入可迭代对象，使用其元素创建新的列表
['a', 'b', 'c', 'd']
```

* bytearray([source[, encoding[, errors]]])  返回一个新的字节数组，根据传入的参数创建一个新的字节数组。  

```python
>>> bytearray('中文', 'utf-8')
bytearray(b'\xe4\xb8\xad\xe6\x96\x87')
```

* bytes([source[, encoding[, errors]]])  返回一个新的“bytes”对象，该对象是该范围内不可变的整数序列。根据传入的参数创建一个新的不可变字节数组。  

```python
>>> bytes('中文', 'utf-8')
b'\xe4\xb8\xad\xe6\x96\x87'
```

* memoryview()  根据传入的参数创建一个新的内存查看对象。  

```python
>>> v = memoryview(b'asdfg')  
>>> v  
<memory at 0x000001C960D01048>  
>>> v[1]
115
```

* complex([real[, imag]])  返回值为real+imag * 1j的复数或将字符串转换为复数。如果第一个参数是一个字符串，他将被解释为一个复数，并且在没有第二个参数的情况下调用该函数。第二个参数不能是字符串。每个参数可以 是任何数字类型（包括复数）。如果imag被省略，则默认为0，并且构造用作数字转换等`int`和`float`。如果省略两个参数，则返回`0j`。  

**注：从字符串转换时，字符串中`+`或`-`运算符旁边的空格**  

```python
complex('1 + 2j')
ValueError
```

* set([iterable])  返回一个新set对象，可选的包含从iterable中获取的元素。set是一个内置的类。  

```python
>>> set()
set()
>>> a = set(range(10))
>>> a
{0, 1, 2, 3, 4, 5, 6, 7, 8, 9}
```  

* frozenset([iterable])  返回一个新frozenset对象，可选的包含从iterable中获取的元素。frozenset是一个内置的类。根据传入的参数创建一个新的不可变集合。  

```python
>>> a = frozenset(range(10))
>>> a
frozenset({0, 1, 2, 3, 4, 5, 6, 7, 8, 9})
```

* range()  实际上它不是一个函数，而是`range`一个不可变的序列类型，如范围和序列类型。根据传入的参数创建一个新的range对象  

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
```

* iter(object[, sentinel])  返回一个迭代器对象。根据第二个参数是否存在，第一个参数的解释不同。如果没有第二个参数，对象必须是支持迭代的集合对象(__iter__()方法)，或者它必须支持序列(从零开始的整数参数的__getitem__()方法)，如果不支持则引发TyprError；如果给出第二个参数sentinel，则object必须是可调用对象，在这种情况下创建的迭代器将为没有参数的对象调用__next__()方法，如果返回的值等于sentinel，StopIteration会被返回，否则它的值被返回。  

```python
>>> a = iter('abcd') #字符串序列
>>> a
<str_iterator object at 0x03FB4FB0>
>>> next(a)
'a'
>>> next(a)
'b'
>>> next(a)
'c'
>>> next(a)
'd'
>>> next(a)
Traceback (most recent call last):
  File "<pyshell#29>", line 1, in <module>
    next(a)
StopIteration
```

* slice  根据传入参数创建一个新的切片对象，返回表示由`range(start, stop, step)`指定的索引集的切片对象。start和step默认为None。切片对象具有只读数据属性，并且仅返回参数值（或其默认值）。  

```python
slice(stop)
slice(start, stop[, step])
>>> c1 = slice(5) # 定义c1
>>> c1
slice(None, 5, None)
>>> c2 = slice(2,5) # 定义c2
>>> c2
slice(2, 5, None)
>>> c3 = slice(1,10,3) # 定义c3
>>> c3
slice(1, 10, 3)
```

* super([type[, object-or-type]]) 根据传入的参数创建一个新的子类和父类关系的代理对象。返回一个将方法调用委托给父类或兄弟类类型的代理对象。这对于访问已在类中重写的继承方法很有用。  

```python
#定义父类A
>>> class A(object):
...     def __init__(self):
...         print('A.__init__')

#定义子类B，继承A
>>> class B(A):
...     def __init__(self):
...         print('B.__init__')
...         super().__init__()

#super调用父类方法
>>> b = B()
B.__init__
A.__init__
```

* object()  创建一个新的object对象，返回一个新的无特征对象。object是所有类的基础。此函数不接受任何参数。

```python
>>> a = object()
>>> a.name = 'kim' # 不能设置属性Traceback (most recent call last):
  File "<pyshell#9>", line 1, in <module>
    a.name = 'kim'
AttributeError: 'object' object has no attribute 'name'
```

### 迭代器操作  

* all(iterable)  如果所有迭代元素逻辑值为真（或者迭代为空）则返回True。相当于：  

```python
def all(iterable):
    for element in iterable:
        if not element:
            return False
    return True
```

* any(iterable)  如果可迭代对象任有一个元素为true，则返回True，否则返回False（若可迭代对象为空，也返回False）。相当于: 

```python
def any(iterable):
    for element in iterable:
        if element:
            return True
    return False
```

* next(iterable[, default])  通过调用方法从迭代器中检索下一个项目__next__()。如果给定default，则在迭代器耗尽时返回，否则返回StopIteration。  

```python
>>> a = iter('abcd')
>>> next(a)
'a'
>>> next(a)
'b'
>>> next(a)
'c'
>>> next(a)
'd'
>>> next(a)
Traceback (most recent call last):
  File "<pyshell#18>", line 1, in <module>
    next(a)
StopIteration
#传入default参数后，如果可迭代对象还有元素没有返回，则依次返回其元素值，如果所有元素已经返回，则返回default指定的默认值而不抛出StopIteration 异常
>>> next(a,'e')
'e'
>>> next(a,'e')
'e'
```

* filter(function, iterable)  从iterable的那些元素给function构造一个迭代器，并且返回true。如果function为None，这个标识function被假定，即所有iterable中的元素都为False或被移除。  

```python
>>> a = list(range(1,10)) #定义序列
>>> a
[1, 2, 3, 4, 5, 6, 7, 8, 9]
>>> def if_odd(x): #定义奇数判断函数
    return x%2==1
>>> list(filter(if_odd,a)) #筛选序列中的奇数
[1, 3, 5, 7, 9]
```

* map(function, iterable, ...)  返回一个iterable，他将函数应用于每个iterable项，从而产生结果。如果传递了其他可迭代参数，则函数必须传入那么多参数，并且并行地应用于所有迭代的项。对于多个迭代，迭代器在最短地iterable耗尽时停止，对于函数输入已经安排到参数元组的情况。  

```python
>>> a = map(ord,'abcd')
>>> a
<map object at 0x03994E50>
>>> list(a)
[97, 98, 99, 100]
```

* sorted(iterable, *, key = None, reverse = False)  从iterable中的项返回一个新的排序列表。有两个可选参数，必须指定为关键字参数。key指定一个参数的函数，该函数用于从iterable中的每个元素提取比较键(eg：key = str.lower)。默认值为None（直接比较元素）。  

reverse 是一个布尔值，如果设置为True，则列表元素将按照每个比较相反的方式进行排序。  

内置sorted()功能保证稳定。如果排序保证不更改比较相等的元素的相对顺序，则排序是稳定的，这有助于在多个过程中进行排序。  

```python
>>> a = ['a','b','d','c','B','A']
>>> a
['a', 'b', 'd', 'c', 'B', 'A']
>>> sorted(a) # 默认按字符ascii码排序
['A', 'B', 'a', 'b', 'c', 'd']
>>> sorted(a,key = str.lower) # 转换成小写后再排序，'a'和'A'值一样，'b'和'B'值一样
['a', 'A', 'b', 'B', 'c', 'd']
```

* zip(*iterables)  聚合传入的每个迭代器中相同位置的元素，返回一个新的元组类型迭代器。为来自每个迭代的元素的迭代器创建一个聚合。返回元组的迭代器，其中第i个元组包含来自每个参数序列或迭代的第i个元素。相当于：

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

* reversed(seq) 反转序列生成新的可迭代对象。  

```python
>>> a = reversed(range(10)) # 传入range对象
>>> a # 类型变成迭代器
<range_iterator object at 0x035634E8>
>>> list(a)
[9, 8, 7, 6, 5, 4, 3, 2, 1, 0]
```

### 编译执行  

* compile(source, filename, mode, flags = 0, dont_inherit = False, optimize = -1)  将源代码编译为代码或AST对象。代码对象可以由exec()或执行eval()。source可以是普通字符串，字节字符串或AST对象。  

```python
>>> #流程语句使用exec
>>> code1 = 'for i in range(0,10): print (i)'
>>> compile1 = compile(code1,'','exec')
>>> exec (compile1)
0
1
2
3
4
5
6
7
8
9
>>> #简单求值表达式用eval
>>> code2 = '1 + 2 + 3 + 4'
>>> compile2 = compile(code2,'','eval')
>>> eval(compile2)
10
```

* eval(expression, globals=None, locals=None)  执行动态表达式求值。参数是一个字符串和可选的全局变量和本地变量。globals必须是字典，locals可以是任何映射对象。  

```python
>>> x = 1
>>> eval('x+1')
2
```

**此函数还可用于执行任意代码对象（例如尤其创建的代码对象compile()）。在这种情况下，传递代码对象不是字符串**  

* exec(object[, globals[, locals]])  执行动态语句块。此函数支持Python代码的动态执行。object必须是字符串或代码对象。如果它是一个自渡川，则将该字符串解析为一组Python语句，然后执行该语句。如果是代码对象，则只执行它。在所有情况下，执行的代码应该作为文件输入有效。  

```python
>>> exec('a=1+2') #执行语句
>>> a
3
```

* repr(object)  返回一个对象的字符串表现形式(给解释器)。返回包含对象的可打印表示的字符串，对于许多类型，此函数尝试返回一个字符串，该字符串在传递时会产生具有相同值的对象eval()，否则表示形式是一个括在尖括号中的字符串。其中包含对象类型的名称以及其它信息通常包括对象的名称和地址。  

```python
>>> a = 'some text'
>>> str(a)
'some text'
>>> repr(a)
"'some text'"
```


### Unicode与字符之间的转换  

* ord()  返回Unicode字符对应的整数  

```python
>>> ord('a')
97
```

* chr()  返回整数所对应的Unicode字符  

```python
>>> chr(97)
'a'
```

* id() 返回对象的唯一标识符  

```python
>>> a = 'some text'
>>> id(a)
69228568
```

**注：这是内存中对象的地址。**  

* ascii(object)  返回对象的可打印表字符串表现方式  

```python
>>> ascii(1)
'1'
>>> ascii('&')
"'&'"
>>> ascii(9000000)
'9000000'
>>> ascii('中文') #非ascii字符"'\\u4e2d\\u6587'"
```

* hash(object)  返回对象的哈希值（如果有的话）。哈希值是整数。它们用于在字典查找期间快速比较字典键，比较相等的数字值具有相同的哈希值（即使它们具有不同的类型）  

```python
>>> hash('good good study')
1032709256
```

**注：对于具有自定义__hash__()方法的对象，请注意hash()根据主机的位宽截断返回值**  

* format(value[, format_spec])  将值转换为“格式化”表示，由format_spec控制。format_spec的解释取决于value参数的类型，但是大多数内置类型都是用标准格式化语法  

```python
#字符串可以提供的参数 's' None
>>> format('some string','s')
'some string'
>>> format('some string')
'some string'

#整形数值可以提供的参数有 'b' 'c' 'd' 'o' 'x' 'X' 'n' None
>>> format(3,'b') #转换成二进制
'11'
>>> format(97,'c') #转换unicode成字符
'a'
>>> format(11,'d') #转换成10进制
'11'
>>> format(11,'o') #转换成8进制
'13'
>>> format(11,'x') #转换成16进制 小写字母表示
'b'
>>> format(11,'X') #转换成16进制 大写字母表示
'B'
>>> format(11,'n') #和d一样
'11'
>>> format(11) #默认和d一样
'11'

#浮点数可以提供的参数有 'e' 'E' 'f' 'F' 'g' 'G' 'n' '%' None
>>> format(314159267,'e') #科学计数法，默认保留6位小数
'3.141593e+08'
>>> format(314159267,'0.2e') #科学计数法，指定保留2位小数
'3.14e+08'
>>> format(314159267,'0.2E') #科学计数法，指定保留2位小数，采用大写E表示
'3.14E+08'
>>> format(314159267,'f') #小数点计数法，默认保留6位小数
'314159267.000000'
>>> format(3.14159267000,'f') #小数点计数法，默认保留6位小数
'3.141593'
>>> format(3.14159267000,'0.8f') #小数点计数法，指定保留8位小数
'3.14159267'
>>> format(3.14159267000,'0.10f') #小数点计数法，指定保留10位小数
'3.1415926700'
>>> format(3.14e+1000000,'F')  #小数点计数法，无穷大转换成大小字母
'INF'

#g的格式化比较特殊，假设p为格式中指定的保留小数位数，先尝试采用科学计数法格式化，得到幂指数exp，如果-4<=exp<p，则采用小数计数法，并保留p-1-exp位小数，否则按小数计数法计数，并按p-1保留小数位数
>>> format(0.00003141566,'.1g') #p=1,exp=-5 ==》 -4<=exp<p不成立，按科学计数法计数，保留0位小数点
'3e-05'
>>> format(0.00003141566,'.2g') #p=1,exp=-5 ==》 -4<=exp<p不成立，按科学计数法计数，保留1位小数点
'3.1e-05'
>>> format(0.00003141566,'.3g') #p=1,exp=-5 ==》 -4<=exp<p不成立，按科学计数法计数，保留2位小数点
'3.14e-05'
>>> format(0.00003141566,'.3G') #p=1,exp=-5 ==》 -4<=exp<p不成立，按科学计数法计数，保留0位小数点，E使用大写
'3.14E-05'
>>> format(3.1415926777,'.1g') #p=1,exp=0 ==》 -4<=exp<p成立，按小数计数法计数，保留0位小数点
'3'
>>> format(3.1415926777,'.2g') #p=1,exp=0 ==》 -4<=exp<p成立，按小数计数法计数，保留1位小数点
'3.1'
>>> format(3.1415926777,'.3g') #p=1,exp=0 ==》 -4<=exp<p成立，按小数计数法计数，保留2位小数点
'3.14'
>>> format(0.00003141566,'.1n') #和g相同
'3e-05'
>>> format(0.00003141566,'.3n') #和g相同
'3.14e-05'
>>> format(0.00003141566) #和g相同
'3.141566e-05'
```

* vars(object)  返回当前作用域内的局部变量和其值组成的字典，或者返回对象的属性列表。

```python
#作用于类实例
>>> class A(object):
    pass

>>> a.__dict__
{}
>>> vars(a)
{}
>>> a.name = 'Kim'
>>> a.__dict__
{'name': 'Kim'}
>>> vars(a)
{'name': 'Kim'}
```

* help([object])  调用内置帮助系统，可查看函数的介绍以及使用方法  

```python
>>> help(str) 
Help on class str in module builtins:

class str(object)
 |  str(object='') -> str
 |  str(bytes_or_buffer[, encoding[, errors]]) -> str
 |  
 |  Create a new string object from the given object. If encoding or
 |  errors is specified, then the object must expose a data buffer
 |  that will be decoded using the given encoding and error handler.
 |  Otherwise, returns the result of object.__str__() (if defined)
 |  or repr(object).
 |  encoding defaults to sys.getdefaultencoding().
 |  errors defaults to 'strict'.
 |  
 |  Methods defined here:
 |  
 |  __add__(self, value, /)
 |      Return self+value.
 |  
  ***************************
复制代码
dir：返回对象或者当前作用域内的属性列表
复制代码
>>> import math
>>> math
<module 'math' (built-in)>
>>> dir(math)
['__doc__', '__loader__', '__name__', '__package__', '__spec__', 'acos', 'acosh', 'asin', 'asinh', 'atan', 'atan2', 'atanh', 'ceil', 'copysign', 'cos', 'cosh', 'degrees', 'e', 'erf', 'erfc', 'exp', 'expm1', 'fabs', 'factorial', 'floor', 'fmod', 'frexp', 'fsum', 'gamma', 'gcd', 'hypot', 'inf', 'isclose', 'isfinite', 'isinf', 'isnan', 'ldexp', 'lgamma', 'log', 'log10', 'log1p', 'log2', 'modf', 'nan', 'pi', 'pow', 'radians', 'sin', 'sinh', 'sqrt', 'tan', 'tanh', 'trunc']
```

* type(object) or type(name, bases, idct)  只有一个参数时，返回对象的类型。返回值是一个类型对象，通常与返回的对象相同。使用三个参数时，返回一个新类型对象。  

```python
>>> class X:
...     a = 1
...
>>> X = type('X', (object,), dict(a=1))
```

* len(s)  返回对象的长度。参数可以是序列（字符串、字节、元组、列表、范围）或集合（字典、集合、不可变集合）  

```python
>>> len('abcd') # 字符串
>>> len(bytes('abcd','utf-8')) # 字节数组
>>> len((1,2,3,4)) # 元组
>>> len([1,2,3,4]) # 列表
>>> len(range(1,5)) # range对象
>>> len({'a':1,'b':2,'c':3,'d':4}) # 字典
>>> len({'a','b','c','d'}) # 集合
>>> len(frozenset('abcd')) #不可变集合
```

* dir([object])  如果没有参数，则返回当前本地范围中的名称列表。使用参数，尝试返回该对象的有效属性列表。  

```python
>>> import struct
>>> dir()   # show the names in the module namespace  # doctest: +SKIP
['__builtins__', '__name__', 'struct']
>>> dir(struct)   # show the names in the struct module # doctest: +SKIP
['Struct', '__all__', '__builtins__', '__cached__', '__doc__', '__file__',
 '__initializing__', '__loader__', '__name__', '__package__',
 '_clearcache', 'calcsize', 'error', 'pack', 'pack_into',
 'unpack', 'unpack_from']
>>> class Shape:
...     def __dir__(self):
...         return ['area', 'perimeter', 'location']
>>> s = Shape()
>>> dir(s)
['area', 'location', 'perimeter']
```


### 进制转换  

* bin(x)  将整数转换为二进制字符串  

```python
>>> bin(3)
'0b11'
```

* oct(x)  将整数转化为八进制字符串  

```python
>>> oct(10)
'0o12'
```

* hex(x)   将整数转换成十六进制字符串   

```python
>>> hex(16)
'0x10'
```

### 装饰器  

* property(fget = None, fset = None. fdel = None, doc = None)  表示属性的装饰器  

fget是获取属性值的函数。fset是用于设置属性值的函数。fdel是用于删除属性值的函数。和文档创建的属性的文档字符串。

典型用法是定义托管属性x：

```python
>>> class C:
    def __init__(self):
        self._name = ''
    @property
    def name(self):
        """i'm the 'name' property."""
        return self._name
    @name.setter
    def name(self,value):
        if value is None:
            raise RuntimeError('name can not be None')
        else:
            self._name = value

            
>>> c = C()

>>> c.name # 访问属性
''
>>> c.name = None # 设置属性时进行验证
Traceback (most recent call last):
  File "<pyshell#84>", line 1, in <module>
    c.name = None
  File "<pyshell#81>", line 11, in name
    raise RuntimeError('name can not be None')
RuntimeError: name can not be None

>>> c.name = 'Kim' # 设置属性
>>> c.name # 访问属性
'Kim'

>>> del c.name # 删除属性，不提供deleter则不能删除
Traceback (most recent call last):
  File "<pyshell#87>", line 1, in <module>
    del c.name
AttributeError: can't delete attribute
>>> c.name
'Kim'
```

* @classmethod  标示方法为类方法的装饰器  

```python
>>> class C:
    @classmethod
    def f(cls,arg1):
        print(cls)
        print(arg1)

        
>>> C.f('类对象调用类方法')
<class '__main__.C'>
类对象调用类方法

>>> c = C()
>>> c.f('类实例对象调用类方法')
<class '__main__.C'>
类实例对象调用类方法

```

* @staticmethod 标示方法为静态方法的装饰器  

```python
#使用装饰器定义静态方法
>>> class Student(object):
    def __init__(self,name):
        self.name = name
    @staticmethod
    def sayHello(lang):
        print(lang)
        if lang == 'en':
            print('Welcome!')
        else:
            print('你好！')

            
>>> Student.sayHello('en') #类调用,'en'传给了lang参数
en
Welcome!

>>> b = Student('Kim')
>>> b.sayHello('zh')  #类实例对象调用,'zh'传给了lang参数
zh
你好
```

参考文献：  
https://blog.csdn.net/oaa608868/article/details/53506188  
http://www.cnblogs.com/xiao1/p/5856890.html  
https://docs.python.org/3/library/functions.html  
https://www.jianshu.com/p/5d3c52c5c2a2  

转载请注明：[南梦婷的博客](https://norah2.github.io) » [点击阅读原文](https://norah2.github.io/2019/03/17/Built_in_func/) 

<!--以下是本文中的链接-->

<script>
{{ site.image_src_prefix }}

{% assign src = site.image_src_prefix %}
</script>

<div>{{site.image_src_prefix}}</div>

[func]: https://{{src}}/05_Built_in_func/01.png

{% endraw %}
