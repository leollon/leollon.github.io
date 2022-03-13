---
title: MongoDB-Performance-Index(1)
date: 2017-09-30 14:09:21
categories: MongoDB
tags: [MongoDB, notes]
---

# 索引操作
1. 创建索引
2. 查看索引
3. 删除索引

<!--more-->

## 创建索引
   ```bash
   > db.COLLECTION_NAME.createIndex({field_name: value}, {options})
   ```
   value: 1 ascending
   value: -1 descending
   options:
       1. "unique": true 唯一索引
       2. "sparse": true 防止其他文档不存在该字段也创建该文档中这个字段的索引
       3. "background":前台/后台创建索引

## 查看索引
   ```bash
   > db.COLLECTION_NAME.getIndexes()
   ```

## 删除索引
   ```bash
   >  db.COLLECTION_NAME.dropIndex({field_name: value})
   ```
   value: 1
   value: -1

**创建索引，不允许field字段都是数组**
   如：
   ```json
   { "_id" : ObjectId("59d087c43ccaa84817f65c8e"), "thing" : ["1", "2", "3"] }
   { "_id" : ObjectId("59d089363ccaa84817f65c92"), "thing" : ["apple", "pear"] }
   ```
   使用数组创建组合索引将引发错误

给数组中的某个字段创建索引
   ```bash
   > db.COLLECTION_NAME.createIndex({'field1.field2': value})
   ```

## Option

sparse option 优点：
1. 使得索引存储空间比起不使用sparse option时较小
2. 创建唯一索引时，更灵活

**使用sparse option的索引字段，不能用来进行排序**

background option
1. true: 后台创建索引，所数据库
2. false: 默认值，锁集合的读写操作


## explain() 函数的使用
   > 描述一些函数的执行过程

   ```bash
   > db.COLLECTION_NAME.explain().help()   # 获取explain()的帮助文档
   ```
