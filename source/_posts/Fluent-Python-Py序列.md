---
title: Fluent_Python - Py序列
date: 2017-08-16 14:52:36
categories: Notes
tags: [Python, notes]
---

无论哪种数据结构，字符串、列表、字节学列、数组、XML元素，抑或是数据库查询结果，他们的都是共用一套丰富的操作：迭代、切片、排序， 还有拼接。

<!-- more -->

### 内置序列类型概览

容器序列

   list、tuple、collections.deque能存放不懂类型的数据
   ```bash
   >>> alist = []
   >>> alist.append('1')
   >>> alist.append(1)
   >>> alist
   ['1', 1]
   ```

扁平序列

   str、bytes、bytearray、memoryview和array.array，只能容纳一种类型的数据。
   ```bash
   >>> str_var += 'a'
   >>> str_var
   'a'
   >>> str_var += 1
   Traceback (most recent call last):
     File "<stdin>", line 1, in <module>
   TypeError: cannot concatenate 'str' and 'int' objects
   ```
   错误提示： 不可以连接'str' 与 'int' 对象

容器序列存放任意类型对象的**引用**，而扁平学列存放的是**值**。

按序列能否更改划分类：
   1. 可变序列

      list、bytearray、array.array、collections.deque
      he memoryview
   2. 不可变序列

      tuple、str、bytes
