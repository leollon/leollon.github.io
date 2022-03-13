---
title: Fluent_Python - Py元组不仅仅是列表(二)
date: 2017-08-21 11:16:08
categories: Notes
tags: [Python, notes]
---

### 不可变列表--tuple

|methods|list|tuple| |
|:----|:----:|:----:|----:|
|`s.__add__(s2)`|1|0| return s + s2 |
|`s.__iadd__(s2)`|1|0| return s += s2 |
|`s.append(e)`|1|0| append one element after last |
|`s.clear()`|1|0| delete all items |
|`s.__contain__(e)`|1|1| e in s |
|`s.copy()`|1|0| shallow copy of the list |
|`s.count(e)`|1|1| count occurences of an element |
|`s.__delitem__(p)`|1|0| remove item at position |
|`s.extend(it)`|1|0| append items from iterable it |
|`s.__getitem__(p)`|1|1| s[p] - get item from iterable it |
|`s.__getnewargs__()`|0|1| support for optimized serialization with pickle |
|`s.index(e)`|1|1| find position of first occurrence of e |
|`s.insert(p, e)`|1|0| insert element e before the item at position p |
|`s.__iter__()`|1|1| get iterator |
|`s.__len__()`|1|1| len(s) - number of items |
|`s.__mul__(n)`|1|1| return s * n |
|`s.__imul__(n)`|1|0| return s *= n |
|`s.__rmul__(n)`|1|1| n * s - reversed repeated concatenation |
|`s.pop(<p>)`|1|0| remove and return last item or item at aptional position p |
|`s.remove(e)`|1|0| remove first occurrence of element e by value |
|`s.reverse()`|1|0| reverse the order of the items in-place, return None |
|`s.__reversed__()`|1|0| get iterator to scan items from last to first |
|`s.__setitem__(p, 3)`|1|0| s[p] = e - put e in position p, overwriting item |
|s.sort(<key>, <reverse>)|1|0| sort items in place with optional keyword arguments key and reverse |
