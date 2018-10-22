---
title: Fluent_Python - PySlicing(一)
date: 2017-08-24 15:25:18
categories: Notes
tags: [Python, notes]
---

## 为什么切片和列表范围不包含最后一个元素

> Python，C 和很多其他语言的索引都是从零开始的。一些方便的功能：

1. 只给出停止位置的时候，很容易就看出来切片或序列范围的长度。
2. 可以很容易的计算出切片或者序列范围的长度，给出起始位置和停止位置时，停止位置减去起始位置即可。
3. 可以很容易的无重复地分割一个序列成两个部分，简单的get my_list[:x] 和 my_list[x:]即可：

```bash
>>> l = (10, 20, 30, 40, 50, 60)
>>> l[:2]  # split at 2, the length = 2
(10, 20)
>>> l[2:] # split start at 3
(30, 40, 50, 60)
>>> l[:3] # split at 3, length = 3
(10, 20, 30)
>>> l[1:4] # start at 1, stop at 4, the length = 4-1 = 3
(20, 30, 40)
```

## 切片对象

```python
objectsSliced[a:b:c] # 语法格式
```

使用步长跳过元素

```bash
>>> strVal = 'hello,world'
>>> strVal[::2]  # 步长为2, 亦即当前索引加上这个步长之后，取该索引对应的元素。
'hlowrd'
>>> strVal[::-1]  # 步长为1,倒序取元素
'dlrow,olleh'
>>> t[::-2]  # 步长为2, 倒序取元素
'drwolh'
```

切片还有其他的功能，比如多为空间的切片和省略标记

*a:b:c*只在[]有效，并且是作为索引或下标使用时， 产生一个切片对象`slice(a, b, c)`。

多维切片和省略切片

[]操作符中还可以使用','进行分割。‘numpy‘包的二维空间获取元素方法a[row, col]、a[row1:row2, col1:col2]。

`__getitem__`和`__setitem__`处理[]操作符，这两个特殊方法接收来自a[row, col]中索引的元组，亦即Python调用`a.__getitem__((row, col))`.

Python的内建的序列类型是一维，因此只支持一个下标或者切片，并且他们不是一个元组。

切片可以用于从序列中解包，还可以用户更改可变序列的内的至。

## 指定切片

**可变序列**使用切片标记放在赋值语句左边可以对其进行修改，作为del 表达式目标时，可以对序列中的元素进行去除。
在赋值语句中作为赋值目标时，右边的值必须是一个可迭代的对象。

实例代码：

```bash
>>> mylist  = list(range(10))
>>> mylist
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
>>>mylist[2:3]
2
>>> mylist[2:3] = [10, 12]  # 元素2更改为10,并在其后面插入元素12
>>> mylist
[0, 1, 10, 12, 3, 4, 5, 6, 7, 8, 9]
>>> del mylist[8:9] # 去除元素7
>>> mylist
[0, 1, 10, 12, 3, 4, 5, 6, 8, 9]
>>> mylist[8:9] = 17  # error: not an iterable object
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: can only assign an iterable
>>> mylist[8:9] = [17] # [17] an iterable object
>>> mylist
[0, 1, 10, 12, 3, 4, 5, 6, 17, 9]
```

## 使用 + 和 * 处理序列
- + 操作的左右两个对象必须是同一种数据类型，返回一个同类型的新对象。

实例代码：
```bash
>>> mylist1 = [2, 4, 2.3]
>>> mylist2 = [1, 3, 3.3]
>>> mylist1 * 3
[2, 4, 2.3, 2, 4, 2.3, 2, 4, 2.3]
>>> mylist1 + mylist2
[2, 4, 2.3, 1, 3, 3.3]
>>> mylist1 # 不更改原来的序列
[2, 4, 2.3]
>>> 3 * "233"
'233233233'
>>> my_list = [1, 2, 3, [2, 4, 6]]
>>> my_list * 3
[1, 2, 3, [2, 4, 6], 1, 2, 3, [2, 4, 6], 1, 2, 3, [2, 4, 6]]
```
## 建立列表中列表

**这里的代码大概涉及到深拷贝与浅拷贝吧**

示例代码一：

```bash
>>> board = [['_'] * 3 for i in range(3)] # 生成三个list(inner)
>>> board
[['_', '_', '_'], ['_', '_', '_'], ['_', '_', '_']]
>>> board[1][2] = 'X'
>>> board
[['_', '_', '_'], ['_', '_', 'X'], ['_', '_', '_']]
>>> board[1] is board[2] # 深拷贝
False
```

实例代码二：

```bash
>>> weird_board = [['_'] * 3] * 3
>>> weird_board
[['_', '_', '_'], ['_', '_', '_'], ['_', '_', '_']]
>>> weird_board[1] is weird_board[2]  # 浅拷贝
True
>>> weird_board[1][2] = 'O'
>>> weird_board
[['_', '_', 'O'], ['_', '_', 'O'], ['_', '_', 'O']]
```
