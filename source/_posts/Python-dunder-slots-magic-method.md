---
title: Python-dunder_slots_magic_method
date: 2017-11-21 13:52:45
categories: Python
tags: [Python, notes, IHaveNoIdea]
---

# magic method __slots__()

> 默认情况下Python用一个字典来保存一个对象的实例的属性。它允许在运行时去设置人一的新属性。

对于有着已知属性的小类来说，它可能是个瓶颈。这个字典因为当要创建许多对象时，就浪费了很多内存。
Python不能在对象创建时分配一个固定量的内存来保存所有的属性。

因此，可以使用魔法方法`__slots__()`来告诉Python不要使用字典，而且只给一个固定及和的属性分配空间。

- 不使用`__slots__`:
  ```python
  class MyClass(object):
      def __init__(self, name, identifier):
          self.name = name
          self.identifier = identifier
          self.set_up()
      # ...
  ```
- 使用`__slots`:
  ```python
  class MyClass(object):
      __slots__ = ['name', 'identifier']
      def __init__(self, name, identifier):
          self.name = name
          self.identifier = identifier
          self.set_up()
      # ...
  ```