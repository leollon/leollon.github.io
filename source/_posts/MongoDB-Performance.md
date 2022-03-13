---
title: MongoDB-Performance
date: 2017-09-28 14:24:51
categories: MongoDB
tags: [MongoDB, notes, nosql]
---

## the way to impact performance
1. add indexes to collections
2. distribute the load across multiple servers using sharding

<!--more-->

## storage engines
存储引擎是一个处于MongoDB server 与 disks之间的程序。

存储引擎有：
1. MMAP
2. WiredTiger

存储引擎直接决定数据文件格式、索引格式，不能够影响api以及MongoDb服务器之间的通信。


## MMAP
- 提供collection level locking
- 提供In place updates
- automatically allocates power-of-two-sized documents padding

## WiredTiger

- 提供documents level concurrency
- 提供comperssions - of data or indexes
- No in place updates


## Index

使用BTree数据结构构建索引，维护索引需要时间，因为进行文档存储(写操作)会影响到索引的结构，需要更新索引，而读取文档却很快。

1. 创建索引
   ```bash
   > db.COLLECTION_NAME.createIndex({field: value})
   ```
   value 取值:
   1. 1 (升序)
   2. -1 (降序)
