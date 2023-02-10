---
layout: post
title: 字符串方法总结  
date: 2019-03-24
description: 本篇文章总结了关于字符串类型数据结构的全部方法   
categories: Python
tags: Python Fun Str
author: NMt
mathjax: true
---

* content
{:toc}
本篇博客是收集关于str方法，并针对每个方法配有示例。函数的分类主要参考老师的课件。  

<div style='display: none'>
@@@@
</div>




{% raw %}
### 字符串的类型判断  

* str.isdigit()   

* str.isdecimal()  

* str.isnumeric()

查看字符串里的字符是否全为数字。如果字符串中的所有字符**全部**都是数字，并且至少有一个字符则返回`true`，否则为`False`。   

```python
>>> s1 = '123456'
>>> s1.isdigit()
True
>>> s2 = '123sadf'
>>> s2.isdigit()
False
>>> s1.isdecimal()
True
>>> s2.isdecimal()
False
>>> s1.isnumeric()
True
>>> s2.isnumeric()
False
```

**注：对于非Unicode字符串，上面三个方法是等价的。**  

* str.isalpha()  查看字符串中的字符是否**全部**为字母（包括大小写）  

```python
>>> s1 = 'hjdfHKJbjh'
>>> s1.isalpha()
True
>>> s2 = 'HjnH12H.'
>>> s2.isalpha()
False
```

* str.isalnum()  如果字符串中的字符**全部**为字符或是数字。如果字符串中的所有字符都为字母或数字，则返回`True`, 否则返回`False`。  

```python
>>> s3 = '13246'
>>> s3.isalnum()
True
>>> s4 = 'sf123'
>>> s4.isalnum()
True
>>> s5 = 'sfd12.?csd'
>>> s5.isalnum()
False
```

* str.islower()  判断字符串中所含有的字母是否**全部**都为小写。（非字母的数字忽略不看）如果没有字母或者字母全为小写则返回`True`, 否则返回`False`。  

```python
>>> s5 = 'sfd12.?csd'
>>> s5.islower()
True
```

* str.isupper()  判断字符串中所含有的字母是否**全部**都为大写。（非字母的数字忽略不看）如果没有字母或者字母全为大写则返回`True`, 否则返回`False`。

```python
>>> s7 = 'VHBHB123.JH'
>>> s7.isupper()
True
```

* str.istitle()  判断字符串中的每一个单词的第一个字母是否为大写。（标题一般都是每个单词的首字母大写）  

```python
>>> s8 = 'I Love Python.'
>>> s8.istitle()
True
```

* str.isspace()  查看字符串中的字符是否**全部**都为空白字符。  

```python
>>> s9 = '\n'
>>> s9.isspace()
True
>>> s9 = '\n h'
>>> s9.isspace()
False
```

**注：空不是空白字符，空白字符包括空格、换行、换页等。**  

* str.isidentifier()  检查字符串是否符合python变量命名规则。  

```python
>>> s10 = '_hello_'
>>> s10.isidentifier()
True
>>> s11 = '1cd'
>>> s11.isidentifier()
False
```

**注：变量名命名规则是以字母或下划线开头，不能用数字开头。变量名中只能包含字母、数字、下划线**  

* str.isprintable()  判断字符串是否**全部**是可打印字符。（制表符、换行符是不可打印字符，空格是可打印字符）  

```python
>>> s12 = ' '
>>> s12.isprintable()
True
>>> s13 = 'as\n'
>>> s13.isprintable()
False
```

* str.isascii()  入轨字符串为空或字符串中的所有字符都是ASCII，则返回`true`，否则返回`False`。  

**注：3.7中的新功能，我用的3.6版本，因此不进行演示。**  

### 大小写转换  

* str.lower()  将字符串中所有字母转换成小写。  

```python
>>> S = 'abcDFdsJ'
>>> S.lower()
'abcdfdsj'
```

* str.upper()  将字符串中所有字母转换成大写。

```python
>>> S = 'abcDFdsJ'
>>> S.upper()
'ABCDFDSJ'
```

* str.capitalize()  将字符串中的首字母转换成大写，其他变为小写。  

```python
>>> s = 'anfGHB sncHd HncdH'
>>> s.capitalize()
'Anfghb snchd hncdh'
```

* str.title()  将字符串中所有单词的首字母转换成大写，其他转换成小写。  

```python
>>> s = 'anfGHB sncHd HncdH'
>>> s.title()
'Anfghb Snchd Hncdh'
```

* str.swapcase()  将字符串中的答谢转换成小写，小写转换成大写。  

```python
>>> s = 'anfGHB sncHd HncdH'
>>> s.swapcase()
'ANFghb SNChD hNCDh'
```

* str.casefold()  类似于函数`lower()`, 但是它旨在删除字符串中的所有大小写区别。  

```python
>>> s = 'anfGHB sncHd HncdH'
>>> s.casefold()
'anfghb snchd hncdh'
```

### 字符串的填充与对齐  

* bytes.center(width[, fillbyte])  将字符串居中输出，width是输出字符的总长度。  

```python
>>> s1 = 'python'
>>> s1.center(50)
'                      python                      '
```

* str.ljust()  将字符串左对齐输出，width是输出字符的总长度。  

```python
>>> s1 = 'python'
>>> s1.ljust(40)
'python                                  '
```

* str.rjust()  将字符串右对齐输出，width是输出字符的总长度。 

```python
>>> s1 = 'python'
>>> s1.rjust(40)
'                                  python'
```

* str.zfill()  将字符串的左端用"0"进行填充。  

```python
>>> s2 = '13456'
>>> s2.zfill(10)
'0000013456'
```

* str.expandtabs(tabsize=8)  将字符串中的制表符转换为四个空格，默认tab键转换为8个空格，但tabsize参数可以设置。  

```python
>>> s1 = '\t'
>>> s2 = s1.expandtabs()
>>> s2
'        '
>>> s2 = s1.expandtabs(tabsize=4)
>>> s2
'    '
```

### 字符串的修剪  

* str.strip([chars])  把字符串中两边的指定字符去掉  

```python
>>> s1 = '    spacious    '
>>> s1.strip()
'spacious'
>>> s2 = 'www.halloworld.com'
>>> s2.strip('wcom.')
'halloworld'
```

* str.lstrip([chars])  把字符串中的左边的指定字符去掉  

```python
>>> s1 = '    spacious    '
>>> s1.lstrip()
'spacious    '
>>> s2 = 'www.halloworld.com'
>>> s2.lstrip('wcom.')
'halloworld.com'
```

* str.rstrip()  把字符串中右边的指定字符去掉  

```python
>>> s1 = '    spacious    '
>>> s1.rstrip()
'    spacious'
>>> s2 = 'www.halloworld.com'
>>> s2.rstrip('wcom.')
'www.halloworld'
```

### 字符串的测试与查找  

* str.startswith(prefix[, start[, end]])  如果字符串以prefix开头，则返回`True`，否则返回`False`，也可以是要查找的prefix元组。start参数为可选参数，测试字符串从该位置开始。end参数为可选的参数，停止在该位置比较字符串。  

```python
>>> s1 = 'abcdefghi'
>>> s1.startswith('as')
False
>>> s1.startswith('ab')
True
>>> s1.startswith('cd')
False
>>> s1.startswith('cd',2)
True
>>> s1.startswith(('cd','ab'))
True
```

* str.endswith(suffix[, start[, end]])  如果字符串以指定的后缀结尾，则返回True，否则返回False。start，end是可选参数，测试从start位置开始，停止在end位置进行比较。要搜索的后缀可能是任何类似于字节的对象。  

```python
>>> s1 = 'abcdefghi'
>>> s1.endswith('hi')
True
>>> s1.endswith('h')
False
>>> s1.endswith(('ds','hi', 'hh'))
True
>>> s1.endswith('h', 0, -1)
True
```

* str.count(sub[, start[, end]])  返回范围[start, end]中子字符串的非重叠出现次数。可选参数的start与end被解释为切片表示法。  

```python
>>> x = 'sffefwsf'
>>> x.count('sf',0,8)
2
```

* str.find(sub[, start[, end]])  返回找到序列中子序列的最小索引值（第一次被找到的位置），start与end是可选参数，指定开始搜索的初始位置与终止位置。找不到则返回-1。  

```python
>>> x = 'sffefwsf'
>>> x.find('fe')
2
>>> x.find('fe', 3)
-1
```

* str.rfind(sub[, start[, end]])  返回在字符串中找到子序列的最大的索引（最后一次出现的位置），start与end是可选参数，指定开始搜索对的初始位置与终止位置。找不到则返回-1。  

```python
>>> x = 'sffefwsf'
>>> x.rfind('f')
7
```

* str.index(sub[, start[, end]])  类似于`find()`函数，但是这个函数在subsequence没有找到时会返回`ValueError`错误。  

```python
>>> x = 'sffefwsf'
>>> x.index('f')
1
>>> x.index('a')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ValueError: substring not found
```

* str.rindex(sub[, start[, end]])  类似于`rfind()`函数，但是当subsequent没有找到时会返回`ValueError`错误。  

```python
>>> x = 'sffefwsf'
>>> x.rindex('f')
7
>>> x.rindex('a')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ValueError: substring not found
```

### 字符串的替换  

* S.replace(old, new[, count])  返回序列的副本，并将所有出现的旧的子序列替换为新的序列。给出可选的参数计数，则只替换第一个计数匹配项。  

```python
>>> x = 'sffefwsf'
>>> x.replace('s','S')
'SffefwSf'
```

* static str.maketrans(from, to)  此静态方法返回一个转换表可用于bytes.translate()将每个字符映射在from到to的字符串进行一一对应，两个字符串长度必须一致，否则返回`ValueError`错误。  

```python
x = 'aaa'
>>> x.maketrans('aaa', 'bbb')
{97: 98}
>>> x.maketrans('a', 'b')
{97: 98}
```

* str.translate(table)  返回字符串的副本，其中每个字符都已通过给定的转换表映射。显示由`maketrans()`形成的映射。  

```python
x = 'aaa'
>>> x.translate(x.maketrans('a', 'b'))
'bbb'
>>> x.translate(x.maketrans('c', 'b'))
'aaa'
>>> x = 'aabbccddeeff'
>>> x.translate(x.maketrans('c', 'b'))
'aabbbbddeeff'
>>> x.translate(x.maketrans('c', 'dd'))
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ValueError: the first two maketrans arguments must have equal length
```

### 字符串的拆分与组合  

* S.split(sep=None, maxsplit=-1)  使用sep作为分隔符字符串，maxsplit为非负数字时代表最大拆分次数。  

```python
>>> b'1,2,3'.split(b',')
[b'1', b'2', b'3']
>>> b'1,2,3'.split(b',', maxsplit=1)
[b'1', b'2,3']
>>> b'1,2,,3,'.split(b',')
[b'1', b'2', b'', b'3', b'']
```

* str.rsplit(sep=None, maxsplit=-1)  使用sep作为分隔符分隔字符串，返回字符串中单词的列表。如果给出了最大拆分，最多只能进行最大拆分，并且从右边开始。  

```python
>>> x = 'sffefwsf'
>>> x.rsplit('s')
['', 'ffefw', 'f']
>>> x.rsplit('s', maxsplit=1)
['sffefw', 'f']
```

* S.splitlines([keepends])  返回字符串中的行的列表，在行边界处断开。除非给出了keepends且为true，否则换行符不包括在生成的列表中。  

此方法在以下边界上分隔。边界时普遍换行的超集。

|Representation|Description|中文|
|:------------:|:---------:|:--:|
|`\n`|Line Feed|换行|
|`\r`|Carriage Return|回车|
|`\r\n`|Carriage Return + Line Feed|回车+换行|
|`\v`or`\x0b`|Line Tabulation|行列表|
|`\f`or`\x0c`|Form Feed|换页|
|`\xlc`|File Separator|文件分隔符|
|`\xld`|Group Separator|组分隔符|
|`\xle`|Record Separator|记录分隔符|
|`\x85`|Next Line(C1 Control Code)|下一行（C1控制代码）|
|`\u2028`|Line Separator|行分隔符|
|`\u2029`|Paragraph Separator|段落分隔符|

```python
>>> 'ab c\n\nde fg\rkl\r\n'.splitlines()
['ab c', '', 'de fg', 'kl']
>>> 'ab c\n\nde fg\rkl\r\n'.splitlines(keepends=True)
['ab c\n', '\n', 'de fg\r', 'kl\r\n']
```

* str.partition(sep)  在第一次出现sep时拆分字符串，并返回包含分隔符之前的部分的元组，分隔符本身以及分隔符之后的部分。如果找不到分隔符，则返回字符串本身以及两个空字符串的元组。  

```python
>>> x = 'abccbacbd'
>>> x.partition('cb')
('abc', 'cb', 'acbd')
>>> x.partition('z')
('abccbacbd', '', '')
```

* S.rpartition(sep)   在最后一次出现sep时拆分字符串，并返回包含分隔符之前的部分的元组，分隔符本身以及分隔符之后的部分。如果找不到分隔符，则返回包含两个空字符串以及字符串本身的元组。  

```python
>>> x = 'abccbacbd'
>>> x.rpartition('cb')
('abccba', 'cb', 'd')
>>> x.rpartition('z')
('', '', 'abccbacbd')
```

* S.join(iterable)  返回一个字符串，把字符串以指定字符串进行相连。  

```python
>>> x = 'aaa'
>>> '-'.join(x)
'a-a-a'
```

* str.format(*args, **kwargs)  执行字符串格式化操作。调用此方法的字符串包含由大括号分隔的文字文本或替换字符。每个替换字段都包含位置参数的数字索引或关键字参数的名称。返回字符串的副本，其中每个替换字段都替换为相应参数的字符串值。  

```python
>>> "The sum of 1 + 2 is {0}".format(1+2)
'The sum of 1 + 2 is 3'
```

* str.format_map(mapping)  类似于`str.format(**mapping)`。  

```python
>>> class Default(dict):
...     def __missing__(self, key):
...         return key
...
>>> '{name} was born in {country}'.format_map(Default(name='Guido'))
'Guido was born in country'
```

参考文献：  
https://docs.python.org/3/library/stdtypes.html  
https://www.cnblogs.com/single-boy/p/7309461.html  

转载请注明：[南梦婷的博客](https://norah2.github.io) » [点击阅读原文](https://norah2.github.io/2019/03/24/str_function/)   

{% endraw %}
