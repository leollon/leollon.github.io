---
title: algorithms - binary_search_through_recursive_function
date: 2018-11-23 16:10:03
categories: ds_alg
tags: [ds_alg, algorithms, binary search]
---

递归程序中重要的三个规则：

- 递归都有一个基本的例子，那就是在程序中包含`return`的条件语句作为方法中的第一条语句。
- 递归调用必须解决的子问题在某种意义上要更小，因此递归调用覆盖或者说包含了上面的基本例子。在接下来的代码中，不同之处在于第三个和第四个值都是递减的。
- 递归调用的解决的子问题不应该是有重叠的。在下面的代码中，被两个子问题所引用的数组部分都是分离，也就是不重叠的。

违反这三个规则，有可能导致不正确的结果或者产生无效的程序。

使用递归函数实现二分查找：

```
class BinarySearch:
    def binary_search(self, array, target):
        """
        args:
            :array: list[int]
            :rtype: int
        >>> s = BinarySearch()
        >>> list_ = [1, 2, 3, 4, 5, 6, 7, 8]
        >>> print(s.binary_search(list_, 2))
        1
        >>> print(s.binary_search(list_, 8))
        7
        >>> print(s.binary_search(list_, 9))
        -1
       """

        """
        return find(array, target, 0, len(array) - 1)
    
    def find(array, target, low, height):
        if low > height: return -1
        mid = (low + height) // 2 # 取整
        if array[mid] < target:
            find(array, target, mid + 1, height) # 将处于低位置的指针移动到中间的元素后面的一个,因为中间元素比要查找的目标要小，当然也可以不加1。
        elif array[mid] > target:
            find(array, target, low, mid - 1)  # 将处于高位置的指针移动到中间的元素后面的一个,因为中间元素比要查找的目标要大，当然也可以不加1
        else:
            return mid
```
