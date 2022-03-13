---
title: quick sort based on linked list
date: 2019-04-09 23:21:37
categories: Algorithms_4th
tags: [quick sort, double linked list, 算法, algos-4th]
---

```python
"""Quick Sort based on double linkedlist
"""
import collections
import random


class DataNode:
    def __init__(self, val):
        self._pre = None
        self._val = val
        self._nxt = None


class DoubleLinkedListIterator(collections.Iterator):
    def __init__(self, head):
        self._head = head

    def __iter__(self):
        return self

    def __next__(self):
        if not self.has_next():
            raise StopIteration
        val = self._head._val
        self._head = self._head._nxt
        return val

    def has_next(self):
        return self._head != None


class DoubleLinkedList:
    def __init__(self):
        self._first = None
        self._last = None

    def create(self, array):
        for val in array:
            new_node = DataNode(val)
            new_node._pre = self._last
            if not self._first:
                self._first = new_node
            else:
                self._last._nxt = new_node
                new_node._pre = self._last
            self._last = new_node
        return self._first

    def add(self, val):
        new_node = DataNode(val)
        new_node = self._last
        if not self._first:
            self._first = new_node
        self._last = new_node

    def show(self):
        ptr = self._first
        while ptr:
            print(ptr._val, end=' ')
            ptr = ptr._nxt
        print()

    def __iter__(self):
        return DoubleLinkedListIterator(self._first)


def quick_sort(linkedlist, left, right):
    if left and right and left != right and right._nxt != left:
        pivot = partition(linkedlist, left, right)
        quick_sort(linkedlist, left, pivot)
        quick_sort(linkedlist, pivot._nxt, right)


def partition(linkedlist, left, right):
    head, tail = left, right
    pivot_node = left
    while True:
        cross = False
        while head._val <= pivot_node._val:
            head = head._nxt
            if head == tail:
                cross = True
            if head == right:
                break
        while tail._val >= pivot_node._val:
            tail = tail._pre
            if head == tail:
                cross = True
            if tail == left:
                break
        if cross:
            break
        head._val, tail._val = tail._val, head._val
    if left._val >= tail._val:
        left._val, tail._val = tail._val, left._val
    else:
        tail = tail._nxt
    return tail


nums = [2, 5, 2, 3, 3, 4]
head = DoubleLinkedList()
head.create(nums)
quick_sort(head, head._first, head._last)
print(list(head), '\n\n', sorted(nums))
print(list(head) == sorted(nums))

nums = random.sample(
    [random.randint(1, 100) for _ in range(100)], k=random.randint(1, 100)
)
head = DoubleLinkedList()
head.create(nums)
quick_sort(head, head._first, head._last)
print(list(head), '\n\n', sorted(nums))
print(list(head) == sorted(nums))
```
