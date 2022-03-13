---
title: Fluent-Python - list.sort() and sorted()
date: 2017-09-06 18:04:50
categories: Notes
tags: [notes, Python]
---

<args>: 代表参数

list.sort() 和 sorted()异同：

<!-- more -->

异：

- list.sort()
>不创建新序列，返回值为None
>
>不接收可迭代的对象

- sorted(<iterable object>)
> 返回值一个排序后的新创建的序列。
>
> 不可接收可迭代对象

```bash
>>> l2test1 = [32, 3, 2, 4, 1, 9, 8, 7]
>>> l2test2 = [33, 22, 10, 7, 9, 6, 5, 35]
>>> l2test1.sort() # return None
>>> l2test1
[1, 2, 3, 4, 7, 8, 9, 32]
>>> sorted(l2test2) # return a new object
[5, 6, 7, 9, 10, 22, 33, 35]
```

同：
函数参数可以有：
   1. reverse
      > reverse=True: 递减排序
      > reverse=False: 递增排序
   2. key
      > key=function_name: 根据这个函数的提供的信息进行排序


```bash
>>> l2test1.sort(reverse=True)
>>> l2test1
[32, 9, 8, 7, 4, 3, 2, 1]
>>> s2test = ['abcba', 'aacc']
>>> s2test.sort(key=len)
>>> s2test
['aacc', 'abcba']

```
