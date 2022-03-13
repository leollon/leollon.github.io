---
title: what_is_hashable
date: 2017-11-15 22:16:32
categories: Python
tags: [Python, notes]
---

哈希表是`Python`高性能词典的基础。
使用`isinstance()`判断某些函数的参数是否是`dict`类型, 而不使用`type`。因为这个参数可能使用不一样的`mapping type`。

什么可散列(哈希)？
一个对象是可散列的，如果在它的生命周期中，有一个散列(哈希)值从不发生改变，并且可以与其他对象相比较。可散列的对象相比较是相等的，它们一定有同样的散列值。

原子不可变类型(str, bytes,numeric)都是可散列(哈希)的。`frozenset`也总是可散列(哈希)的，因为通过定义得知，它的元素必须是可散列(哈希)的。

可变类型：`list`， `dict`

不可变类型tuple，有时也可以是可散列(哈希)的。
```bash
>>> t1 = ('a', 'b', 'c')
>>> type(t1[0])
<class 'str'>
>>> hash(t1)
6076729038523449333
>>> t2 = ('a', 'b', {'a': 1, 'b': 2}]) # t2[2] 是可变类型
>>> hash(t2)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: unhashable type: 'dict'
>>> t3 = (b'a', b'b', b'c') # bytes
>>> type(t3[0])
<class 'bytes'>
>>> hash(t3)
6076729038523449333
>>>
```