---
title: python dunder slots in class
date: 2019-06-28 22:21:00
categories: Python
tags: [Python,dunder slots, notes]
---

```Python

"""使用类变量`__slots__`

`__slots__`为声明的的变量预留空间并且阻止自动创建每个实例的`__dict__` 和 `__weakref__`。
`__slots__`在类层级上通过为每个变量名创建描述器来实现。因此不能用类属性来为通过`__slots__`
定义的实例变量设置默认值。否则类属性会覆盖描述器赋值。

"""


class A0:
    """A class without `__slots__`.
    """

    def __init__(self, ff, bb):
        self.foo = ff
        self.bar = bb


class B0(A0):
    """Inheriting from :class:`A0`.

    This is a child class.
    """

    def __init__(self, ff, bb):
        super(B0, self).__init__(ff, bb)
        self.bb = bb


# __dict__ example
class A1:
    __slots__ = [
        "foo",
        "bar",
        # "__dict__"
    ]
    # __slots__ = ""
    def __init__(self, ff, bb):
        self.foo = ff
        self.bar = bb
        # self.bb = bb  # AttributeError: 'A' object has no attribute 'bb',
        # __slots__ has no reserved space for bb.


class B1(A1):
    """Child class."""

    # __slots__ = ['barr', '__dict__', '__weakref__']

    def __init__(self, ff, bb):
        super(B1, self).__init__(ff, bb)
        # self.barr == bb  # Error


class A2:
    __slots__ = ["foo", "bar", "coo"]

    def __init__(self, ff="A2 foo", bb="A2 bar"):
        self.foo = ff
        self.bar = bb
        self.coo = "cool"

    def __get__(self, attr):
        return self.coo


# __weakref__ example
class MyDict(dict):
    """几个内置的类型，例如`list` 和 `dict`都不支持弱引用，但是可以通过子类来支持弱引用。

    https://docs.python.org/3/library/weakref.html

    """

    __slots__ = [
        "foo",
        "bar",
        # "__weakref__"
    ]

    def __init__(self, ff, bb, **kwargs):
        super(MyDict, self).__init__(**kwargs)
        self.foo = ff
        self.bar = bb


if __name__ == "__main__":
    obj_a0 = A0(12, 23)
    obj_b0 = B0(12, 33)
    # 当继承一个没有`__slots__`的类的时候，实例的`__dict__` 和`__weakref__`属性将会总是
    # 可以访问的。
    print(obj_a0.__dict__, obj_a0.__weakref__)
    print(obj_b0.__dict__, obj_b0.__weakref__)

    # 不可访问`__dict__`的例子
    obj_a1 = A1(12, 23)
    obj_b1 = B1(12, 33)  # AttributeError: 'B1' object has no attribute 'barr'
    # 没有`'__dict__'`变量，实例就不能被赋予一个不在`__slots__`列表中定义的新变量。尝试赋值
    # 则引发:exc:`AttributeError`异常。如果希望动态赋予一个新变量，那么得将字符串
    # `'__dict__'`加入到`__slots__`的声明中。
    # print(obj_a1.__dict__)  # AttributeError: 'A1' object has no attribute '__dict__'

    # `__slots__`的声明动作并不仅限制于定义它的类。在父类中定义的`__slots__`在子类中同样
    # 可用。然而，子类还是会有`__dict__` 和 `__weakref__`，除非子类中也定义`__slots__`
    # （通常应该只包含额外的名字）。
    print(obj_b1.__dict__, obj_b1.__weakref__)


    # 不可访问`__weakref__`的例子
    # 实例中没有`__weakref`变量，类定义`__slots__`不支持弱引用它的的实例。如果需要支持弱
    # 引用，得在声明`__slots__`时，将`'__weakref__'`加入到字符串序列中。
    obj_dict = MyDict(1, 2, red=1, green=2, blue=3)  # weak refereceable object

    # AttributeError: 'MyDict' object has no attribute '__weakref__'
    # print(obj_dict.__weakref__)
```

# 参考

[__slots__](https://docs.python.org/3/reference/datamodel.html?highlight=__slots__#slots)
[usage of slots](https://stackoverflow.com/questions/472000/usage-of-slots)

