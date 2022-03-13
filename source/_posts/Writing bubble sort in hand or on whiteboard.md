---
title: Writing bubble sort in hand or on whiteboard
date: 2018-11-10 22:09:28
categories: ds_alg
tags: [bubble sort,algorithms,sort]
---


```Python
"""
Write bubble sort in hand or on whiteboard.
"""
import doctest

class BubbleSort:
    """
    >>> s = BubbleSort()
    >>> arr = [5, 6, 7, 8, 9]
    >>> s.sort(arr)
    >>> arr
    [9, 8, 7, 6, 5]
    """
    def sort(self, array):
        for i in range(len(array)):
            exchange = False
            for j in range(len(array) - i - 1):
                # 不能取到最后面一个元素，当取到最后面一个元素的时候，下一个元素是越界，是未知的。
                if array[j] < array[j+1]:
                    array[j], array[j + 1] = array[j + 1], array[j]
                    exchange = True
            if not exchange: break

if __name__ == "__main__":
    doctest.testmod(verbose=True)
```
