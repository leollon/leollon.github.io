---
title: what is the difference between shallowcopy and deepcopy
date: 2017-08-22 15:01:28
categories: Python
tags: [Python, 浅谈]
---

就自己不知道的浅拷贝和深拷贝，浅谈一下自己的理解。

<!-- more -->
## 浅拷贝

- shallow copy(浅拷贝)

   `copy.copy(iterable)`

第一段代码：

```bash
>>> import copy
>>> alist = ['hello', ['world']]
>>> alist2 = copy.copy(alist)
>>> id(alist) # 对象的内存地址
140685187528136
>>> id(alist2) # 对象内存地址
140685152964360
>>> alist2 is alist # 两个对象的内存地址是否一样的
False
>>> alist[1]
['world']
>>> alist
['world']
>>> id(alist[1]) # 对象alist中的对象alist[1]的内存地址
140685187505288
>>> id(alist2[1]) # 对象alist2中的对象alist2[1]的内存地址
140685187505288
>>> alist2[1] is alist[1] # 两个对象的内存地址是否一样的
True
>>> l1 = ['hello', [123, 456]]
>>> l2 = l1[:]
>>> l1
['hello', [123, 456]]
>>> l2
['hello', [123, 456]]
>>> l2[0] = 'world'
>>> l2
['world', [123, 456]]
>>> l1
['hello', [123, 456]]

```

## 深拷贝

- deep copy(深拷贝)

   `copy.deepcopy(iterable)`

第二段代码：

```bash
>>> import copy
>>> alist3 = copy.deepcopy(alist)
>>> id(alist3) # 对象aliste3的内存地址
140685153378760
>>> id(alist) # 对象alist的内存地址
140685187528136
>>> alist3 is alist  # 两个对象的内存地址是否一样
False
>>> alist[1]  # 对象alist中的对象alist[1]的值
['world']
>>> id(alist[1])  # 对象alist中的对象alist[1]的内存地址
140685187505288
>>> alist3[1]  # 对象alist中的对象alist3[1]的值
['world']
>>> id(alist3[1])  # 对象alist3中的对象alist3[1]的内存地址
140685152960904
>>> alist3[1] is alist[1] # 两个对象的内存地址是否一样的
False
```

## 总结

可以发现浅拷贝和深拷贝异同

   1. 相同点就是结果会产生一个新对象

   2. 不同点是当他们是组合对象的时候，即对象中还有别对象的时候。浅拷贝只是拷贝最外层的对象创建一个新对象，新对象存储原对象里层对象的引用(因为内存地址是一样的)。而深拷贝则是递归式的进行拷贝原对象来创建一个新对象。
