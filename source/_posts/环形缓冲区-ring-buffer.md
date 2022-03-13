---
title: 环形缓冲区(ring buffer)
date: 2019-03-04 16:18:19
categories: Algorithms_4th
tags: [algs, ring-buffer, 算法]
---


```Python3
"""1.3.39 Ring ring_buffer
A ring ring_buffer, or circular queue, is a FIFO data structure of a fixed size N.
It is useful for transferring data between asynchronous processes or for
storing log files.
When the ring_buffer is empty, the consumer waits until data is deposited;
when the ring_buffer is full, the producer waits to deposit data.
Develop an API for a Ringring_buffer and an implementation that uses an array
representation (with circular wrap-around).
"""
import unittest
import collections


class RingBufferIterator(collections.Iterator):

    def __init__(self, first, buffer):
        self.__first = first
        self.__count = 0
        self.__ring_buffer = buffer

    def __iter__(self):
        return self

    def __next__(self):
        if not self.has_next(): raise StopIteration
        item = self.__ring_buffer[self.__first]
        if self.__first == len(self.__ring_buffer) - 1:
            self.__first = 0  # wrap around
        else:
            self.__first += 1
        self.__count += 1
        return item

    def has_next(self):
        return self.__count < len(self.__ring_buffer)


class RingBuffer:

    def __init__(self, size):
        self.__max_size = size                  # 缓冲区最大空间
        self.__ring_buffer = [None] * size      # 缓冲区
        self.__first = -1                       # 读指针
        self.__last = -1                        # 写指针
        self.__size = 0                         # 当前缓冲大小
        self.__producer_aux_ring_buffer = []    # 候补缓冲区队列
        self.data_count_to_be_consumed = 0      # 消费者等待数量

    def __iter__(self):
        return RingBufferIterator(self.__first, self.__ring_buffer)

    def size(self):
        # 获取缓冲区大小
        return self.__size

    def is_empty(self):
        # 缓冲区是否为空
        return self.__size == 0

    def is_full(self):
        # 缓冲区是否满了
        return self.__size == self.__max_size

    def produce(self, item):
        # 生产
        if self.data_count_to_be_consumed > 0:
            # 消费者等待数据数量
            self.consume_data(item)
            self.data_count_to_be_consumed -= 1
        else:
            if self.is_empty():
                # 缓冲去为空
                self.__ring_buffer[self.__size] = item
                self.__first = 0
                self.__last = 0
                self.__size += 1
            else:
                if not self.is_full():
                    # 缓冲区未满
                    if self.__last == len(self.__ring_buffer) - 1:
                        # 写指针到达环形缓冲区结尾元素
                        self.__last = 0  # 形成环，从头开始写入数据
                    else:
                        self.__last += 1
                    self.__ring_buffer[self.__last] = item  # 将数据写入缓冲区中
                    self.__size += 1
                else:
                    # 缓冲区满时，将元素放入到后部缓冲区中
                    self.__producer_aux_ring_buffer.append(item)

    def consume_data(self, item):
        print("Data consumed: ", item)

    def consume(self):
        if self.is_empty():
            # 消费者进行数据消费时，缓冲区为空，消费者处于等待中，这时统计要消费者等待数量
            self.data_count_to_be_consumed += 1
            return
        item = self.__ring_buffer[self.__first]  # 从读指针获取数据
        self.__ring_buffer[self.__first] = None  # 将当前指针指向的元素设置为None，保持缓冲区长度，avoid loitering

        if self.__first == len(self.__ring_buffer) - 1:
            # 读指针到达缓存末尾元素
            self.__first = 0  # 形成环，从头开始读
        else:
            # 移动读指针到下一个元素
            self.__first += 1
        self.__size -= 1

        if len(self.__producer_aux_ring_buffer):
            # 将等待候补缓冲区队列中的元素出队并添加到缓冲区中
            self.produce(self.__producer_aux_ring_buffer.pop(0))
        return item


class TestRingBuffer(unittest.TestCase):

    @staticmethod
    def create_ring_buffer(size):
        return RingBuffer(size=size)

    def test_ring_buffer(self):
        ring_buffer = self.create_ring_buffer(4)
        ring_buffer.produce(0)
        ring_buffer.produce(1)
        ring_buffer.produce(2)
        ring_buffer.produce(3)
        ring_buffer.produce(4)
        ring_buffer.produce(5)


        item1 = ring_buffer.consume()
        self.assertEqual(0, item1)

        item2 = ring_buffer.consume()
        self.assertEqual(1, item2)

        ring_buffer.produce(6)
        ring_buffer.produce(7)

        result = [2, 3, 4, 5]
        ans = list(ring_buffer)
        self.assertEqual(result, ans)


if __name__ == "__main__":
    unittest.main()
```
