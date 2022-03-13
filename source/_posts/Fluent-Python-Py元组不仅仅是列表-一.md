---
title: Fluent_python - Py元组不仅仅是列表(一)
date: 2017-08-20 09:39:02
categories: Notes
tags: [Python, notes]
---

除了用作不可变的列表，它还可以用于没有字段名的记录。元组其实是对数据的记录： 元组中的每个元素都存放了记录中一个字段的事数据，外加这个字段的位置。

<!-- more -->

占位符的使用：

```python
alist = [('USA', "209"), ("China", "+86")]
for country, _ in alist:
    print(country)
```

### 元组拆包：

> 元组拆包可以应用到任何可迭代对象上，唯一的硬件的硬性要求是，被可迭代对象中的元素数量必须要跟接受这些元素的元组的空档数一致。


拆包例子：
```python
a, b  = b, a  # 不借助中间变量，交换它们的值
```

平行赋值，把一个可迭代对象里的元素，一并复制到有对应的变量组成的元组中。

用 * 来表示忽略多余的元素。

用 * 运算符把一个可迭代对象拆开作为函数的参数：

```bash
>>> divmod(16, 8)  # divmod return tuple(16//8, 16%8)
(2, 0)
>>> t = (16, 8)
>>> divmod(*t)
(2, 0)
```

**用 * 来处理剩下的元素**

```bash
>>> a, b, *rest = range(5)
(0, 1, [2, 3, 4])
>>> *head, a, b = range(5)
([0, 1, 2], 3, 4)
>>> a, *middle, b = range(5)
(0, [1, 2, 3], 4)
```

元组拆包的强大功能，可以应用在嵌套结构中

### 嵌套元组拆包

```python
metro_areas = [('Tokyo', 'JP', 36.933, (35.6895, 127.38323)),
               ('Delhi NCR', 'IN', 21.234,
                (23.233533, 77.208889)), ('Mexico City', 'MX', 20.142,
                                          (19.2323, -99.13333)),
               ('New York-Newark', 'US', 20.104,
                (40.808611, -74.020386)), ('Sao Paulo', 'BR', 19.649,
                                           (-23.54778, -46.635833))]

print('{:15} | {:^9} | {:^9}'.format('', 'lat.', 'long.'))
fmt = '{:15} | {:9.4f} | {:9.4f}'

for name, cc, pop, (latitude, longitude) in metro_areas:
    if longitude <= 0:
        print(fmt.format(name, latitude, longitude))
```

### 具名元组
`collections.namedtuple`是一个工厂函数，可以用来构建一个带字段名的元组和一个有名字的类——对于调试程序大有用处。

使用collections.namedtuple创建一个类：

```python
import collections
Card = collections.namedtuple('Card', ['rank', 'suit']) # Returns a new subclass of tuple with named fields.
```

定义和使用一个named tuple 类型

```bash
>>> from collections import namedtuple
>>> Person = namedtuple('Person', 'name gender birthday')
>>> walter = Person('walter', 'M', (2017, 8, 20))  # an instance of Person
>>> walter  # print the tuple
Person(name='walter', gender='M', birthday=(2017, 8, 20))
>>> walter.name
'walter'
>>> walter.birthday
(2017, 8, 20)
>>> walter.gender # read by field
'M'
>>> walter[1]  # read by index
'M'
```

namedtuple 类型除了那些继承自tuple之外的东西还有下面这些最有用的属性：

- _fields -- 类属性, 返回一个该类中含有的字段名的元组
- _make(iterable) -- 类方法， 从一个可迭代对象实例化一个named tuple的类型
- _asdict() -- 实例方法， 返回collections.OrderedDict，我理解为一个字典

```bash
>>> Person._fields
('name', 'gender', 'birthday')
>>> Birthday = namedtuple('Birthday', 'year month day')
>>> brien_data = ('walter brien', 'M', Birthday(2017, 8, 23))
>>> brien = Person._make(walter_data) # Person(*walter_data) is the same
>>> brien
Person(name='walter brien', gender='M', birthday=Birthday(year=2017, month=8, day=23))
>>> brien._asdict()
OrderedDict([('name', 'walter brien'), ('gender', 'M'), ('birthday', Birthday(year=2017, month=8, day=23))])
>>> for key, value in brien._asdict().items():
...     print(key + ':', value)
...
name: walter brien
gender: M
birthday: Birthday(year=2017, month=8, day=23)
```
