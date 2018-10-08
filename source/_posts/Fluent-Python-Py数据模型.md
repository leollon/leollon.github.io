---
title: Fluent_Python - Py数据模型
date: 2017-08-14 11:44:01
categories: Notes
tags: [Python, notes]
---

数据模型其实是对Python框架的描述，它规范了这么语言自身构建模块的接口，这些模块包括但不限于序列、迭代器、函数、类和上下文管理器。

<!-- more -->

特殊方法名能让你自己的对象实现和和支持一下的语言架构，并与之交互:

- 迭代
- 集合类
- 属性访问
- 运算符重载
- 函数和方法的调用
- 对象的创建和销毁
- 字符串表示形式和格式化
- 管理上下文(即with块)

magic和dunder
魔法方法是特殊方法的昵称，即诸如__getitem__(self, key)、__init__(self)这些dunder method，都是模范方法。当Python解释器碰到特殊的句法的时候，会使用这些特殊方法激活一些基本的对象操作。

实现特殊方法(magic function)的一个例子

```python
import collections

Card = collections.namedtuple('Card', ['rank', 'suit']) # 构建一个Card类。namedtuple 用以构建只有少数属性但是没有方法的对象，如数据库条目。

class FrenchDeck:
    ranks = [str(n) for n in range(2, 11)] + list('JQKA')
    suits = 'spades diamonds clubs hearts'.split()

    def __init__(self):
        """Initialize an object when create an object"""

        self._cards = [Card(rank, suit) for suit in self.suits
                                    for rank in self.ranks]

    def __len__(self):
        return len(self._cards)

    def __getitem__(self, position):
        return self._cards[position]
```

#### 使用特殊方法

1. 自定义类的对象，调用特殊方法

   例如`__len__()`这个特殊方法的调用是解释器去调用，而写代码的时候，应该使用`len(your_object)`。your_object是一个自定义类的对象的时候，由Python解释器去调用这个对象所属的类中你自定义的`__len__()`

2. 内置类型，例如列表(list), 字符串(str), 字节序列(bytearray)

   CPython解释器会取捷径，`__len__()`直接返回PyVarObject里的欧版——size属性。
   >PyVarObject是表示内存中长度可变的内置对象的C语言结构提。

   除非有大量的元编程存在，直接调用特殊方法的频率应该远远低于实现这些特殊方法的频率。唯一例外的可能是`__init__()`方法，也就是当子类的`__init__()`方法调用超类的构造器。

  通过内置的函数(len、iter、str，等等)来使用特殊方法是最好的选择。
