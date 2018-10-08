---
title: Fluent_Python - First-Class functions
date: 2017-08-28 11:00:26
categories: Notes
tags: [Python, notes]
---

Python中函数是一级对象，一级对象被定义为程序的实体，这个实体可以：
- 运行时创建
- 可以赋值给数据结构中的一个变量或一个元素
- 可以作为一个参数传递给一个函数
- 返回值可以是一个函数

<!--- more -->
Python 中以及对象的例子：
1. Integer(int())
2. strings(str())
3. dictionaries(dict())

将函数作为一个对象对待

```bash
>>> def fact(n):   # 在Python解释器运行时创建一个函数
...     '''returns n!'''
...     return 1 if n < 2 else n * fact(n-1)
...
>>> fact.__doc__  # 函数功能说明，或者说是一个函数的帮助文档
'returns n!'
>>> type(fact)  # 函数的类型
<class 'function'>
```

将函数赋值给一个对象或作为一个参数传入一个函数中

```bash
>>> f = fact
>>> f
<function fact at 0x7f750492d9d8>
>>> f(5)
120
>>> map(fact, range(11))
<map object at 0x7f7ee7de16a0>
>>> list(>>> map(fact, range(11)))
[1, 1, 2, 6, 24, 120, 720, 5040, 40320, 362880, 3628800]
```
