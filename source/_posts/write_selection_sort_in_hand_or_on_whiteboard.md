---
title: algorithms - selection_sort
date: 2018-11-30 20:18:30
categories: ds_alg
tags: [ds_alg, algorithms, selection sort]
---


## 选择排序


```Python3
class SelectionSort:
    """Ascending sort
    """
    def selection_sort(self, array):
        for i in range(len(array)):
            min = i  # assume current index is the index of the minimun value on current iteration.
            for j in range(i + 1, len(array)):
                # visit every element remained and find minimun value on each iteration, (len(array)-i) visited times on each iteration
                if l[min] >= l[j]:
                    min = j # get the index of minimun value on each iteration
            if min != i:
                l[min], l[i] = l[i], l[min]
```
