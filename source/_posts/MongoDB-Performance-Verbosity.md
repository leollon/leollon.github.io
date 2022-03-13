---
title: MongoDB-Performance-Verbosity
date: 2017-10-04 00:25:11
categories: MongoDB
tags: [MongoDB, notes]
---

## Explain

explain()运行模式
1. queryPlanner
2. executionStats
3. allPlansExecution

<!--more-->

到目前位置，explain命令运行的模式都是queryPlanner mode。

## executionStats mode
   > 可用于查看执行最优操作时的状态

   ```bash
   > var exp = db.COLLECTION.explain("executionStats");
   > exp.find({field_name: value})
   ```
   value:
      - 1
      - -1

## allPlansExecution
   > 用于查看执行所有操作时的状态

   ```bash
   > var exp = db.COLLECTION_NAME.explain("allPlansExecution");
   > exp.find({field_name: value})
   ```
   value取值:
     - 1
     - -1

关于query与index之间注意的事情是：
为有必要的查询创建索引，即是有查询的地方有必要创建索引，不需要创建无用的索引。
