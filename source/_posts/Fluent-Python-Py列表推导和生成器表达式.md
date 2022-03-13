---
title: Fluent_Python - Py列表推导和生成器表达式
date: 2017-08-17 11:03:47
categories: Notes
tags: [Python, notes]
---

列表推导是构建列表(list)的快捷方式，而生成器表达式则可以用来创建其他任何类型的序列。

<!-- more -->

列表推导和可读性

下面两个例子都是将一个字符串变成Unicode码位的列表

Example 1:

```bash
>>> symbols = '$£€'
>>> codes = []
>>> for symbol in symbols:
...     codes.append(ord(symbol))
...
>>> codes
[36, 163, 8364]
```
Example 2:

```bash
>>> symbols = '$£€'
>>> codes = [ord(symbol) for symbol in symbols]
>>> codes
[36, 163, 8364]
```

通常规则是，只用列表推导来创建新的列表，并且尽量保持间断，如果列表推到的代码超过两行，可能就要考虑是不是得用for循环重写了。
这并不是一个硬性的规则，关键的度还是得自己把握。

**列表推导不会再有变量泄漏的问题**

python2.x 中，在列表推导中for关键词后面的赋值操作可能会影响列表推导上下文中的同名变量。

Python2.7

```python
x = 'hell world'
dummy = [x for x in 'ABC']
print x # C
print dummy # ['A', 'B', 'C']
```

Python3.5

```python
x = 'hell world'
dummy = [x for x in 'ABC']
print x # hello world
print dummy # ['A', 'B', 'C']
```

列表推导同filter和map的比较

>用列表推导和map/filter组合来创建同样的列表

```bash
>>> symbols = '$£€'
>>> beyond_ascii  = [ord(s) for s in symbols if ord(s) > 127]
>>> beyond_ascii
[163, 8364]
```

```bash
symbols = '$£€'
beyond_ascii = list(filter(lambda c: c > 127, map(ord, symbols)))
>>> beyond_ascii
[163, 8364]

```

笛卡儿积

**列表的长度等于输入变量长度的乘积, 即假设列表1长度M，列表2的长度N，则它们的积为M*N的**

```bash
>>>last_name = ['John ', 'Kyrie '] # len(last_name) = 2
>>>first_name = ['Irving', 'Sown', 'James'] # len(first_name) = 3
>>>full_name = [l+f for l in last_name
             for f in first_name]
>>>full_name
# ['John Irving', 'John Sown', 'John James', 'Kyrie Irving', 'Kyrie Sown', 'Kyrie James']
```

生成器表达式

>生成器表达式遵守迭代器协议，可以逐个元素生成，节省内存空间。

生成器表达式语法与列表推导差不过，只不过中括号改成了圆括号而已。

用生成器表达式初始化元组和数组

```bash
>>> symbols = '$£€'
>>> tuple(ord(symbol) for symbol in symbols)
(36, 163, 8364)
>>> import array
>>> array.array('I', (ord(symbol) for symbol in symbols)) # I表示 C语言中的unsigned integer
array('I', [36, 163, 8364])
```
