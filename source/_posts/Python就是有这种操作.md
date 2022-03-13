---
title: Python就是有这种操作
date: 2017-08-30 22:05:28
categories: Python
tags: [notes, Python]
---

发现`Python`有这个操作的是因为再看Fluent Python这本书并且在Python解释器中运行一小段代码的发现这种操作的，现在记录一下这个过程。

<!-- more -->

```bash
>>> t1 = (1, 2, 3)
>>> l1 = [1, 2, 3]
>>> id(t1)
140129474189664
>>> id(l1)
140129446358480
>>> t1
(1, 2, 3)
>>> l1
[1, 2, 3]
>>> a[0] is b[0]
True
>>> a[1] is b[1]
True
>>> a[2] is b[2]
True
```

是的，每一次创建一个对象的时候，都会为这个对象一个新的内存地址。但总有些情况是例外的。后来去[Google](https://www.google.com/search?newwindow=1&safe=active&q=why+is+that+the+id+of+element+in+list++and+tuple+are+the+same&oq=why+is+that+the+id+of+element+in+list++and+tuple+are+the+same&gs_l=psy-ab.3..33i160k1.51115.123511.0.123746.123.101.6.0.0.0.621.12660.2-33j6j6j1.46.0....0...1.1.64.psy-ab..76.18.5559...0j0i131k1j0i3k1j33i22i29i30k1j33i21k1.OGHgGJKSmvw)了一发，然后看到第一条[Stack Overflow](https://stackoverflow.com/questions/38189660/two-variables-in-python-have-same-id-but-not-lists-or-tuples)。

下面引用Kasramvd说的一段话：

>Immutable objects don't have the same id, and as a mater of fact this is not true for any type of objects that you define separately. Every time you define an object in Python, you'll create a new object with a new identity.

>But there are some exceptions for small integers (between -5 and 256) and small strings (interned strings, with a special length (usually less than 20 character)) which are singletons and have same id (actually one object with multiple pointer).

意思就是说，当整形数(small integers)在-5到256这个区间的时候，或者是字符串(small strings)是单例，这些字符串有特别长度(通常小于20个字符)，那么它们就有相同的id，即相同的内存地址。


# 参考内容

- [interning strings](https://en.wikipedia.org/wiki/String_interning)
- [wiki_单例模式](https://zh.wikipedia.org/wiki/%E5%8D%95%E4%
BE%8B%E6%A8%A1%E5%BC%8F)
- [wiki_singleon pattern](https://en.wikipedia.org/wiki/Singleton_pattern)
