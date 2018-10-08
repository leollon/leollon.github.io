---
title: MongoDB-CRUD-Read(2)
date: 2017-09-12 11:07:06
categories: MongoDB
tags: [MongoDB, notes]
---

CRUD - Create/Read document(s)

mongo shell中find函数返回的批量的文档。
批量的文档的文档大小不超过最大BSON文档大小，第一个批量文件的大小是101个文档抑或是1MB

<!-- more -->

# Create document(s)

使用自动生成的ID插入一个文档

```bash
> db.COLLECTION_NAME.insertOne({})
```

指定文档ID插入一个文档
这个ID是唯一的，如果与已经存在的文档的id相同，则文档插入操作立刻终止。

```bash
> db.COLLECTION_NAME.insertOne({"_id": ""})
```

一次插入多个文档

```bash
> db.COLLECTION_NAME.insertMany([{"document1": "document1"}, {"document2": "document2"}])
```

`_id` 组成：
12个字节的十六进制字符串
DATE(4个字节)，MAC Address(3个字节)，PID(2个字节)，Counter(3个字节)

# Read document(s)

```bash
> db.COLLECTION_NAME.find({"key1": "value1", "key2": value2})
```

查找字段中存储的嵌套的文档
```bash
> db.COLLECTION_NAME.find({"field_name.key": value})
```

Equality Matches On Array:
- 使用完整的数组查找文档
   ```bash
   > db.COLLECTION_NAME.find({ "key": [value1, value2] })
   ```
- 使用任意元素查找所有文档
   ```bash
    > db.COLLECTION_NAME.find({"key" : value})
    ```
- 使用特定的元素查找某些文档
   ```bash
   > db.COLLECTION_NAME.find({"key.0" : value})
   ```
- 使用操作符号进行复杂的匹配方式


`find()`返回一个cursor。

mongo shell中的手动迭代文档

```bash
> var col_name = db.COLLECTION_NAME.find({});
> var doc_iter = function () { return col_name.hasNext() ? c.next() : null; };
> col_name.objsLeftInBatch(); # 查看可以迭代的文档数量
100
> doc()    # 手动迭读取文档
```

## 返回只含有有特定字段的文档
```#!/usr/bin/env bash
> db.COLLECTION_NAME.find({"key" : value}, {field_name: 1, _id: 0})
```
- 1 包含该字段
- 0 不包含该字段



# 查询选择器具

**Comparison operator**

> eq, gt, gte, lt, lte, ne

|Name|Description|
|:----|----:|
|$eq|只匹配出所有文档中与某个特定值相等的文档|
|$gt|只匹配出所有文档中大于某个特定值的文档|
|$gte|只匹配出所有文档中大于或等于某个特定值的文档|
|$ne|只匹配出所有文档与某个特定值不相等的文档|
|$in|匹配出符合数组中任意特定值的所有文档|
|$nin|匹配出不符合数组任意特定值的所有文档|

查找大于90分钟并且小于120分钟的电影
```bash
> db.COLLECTION_NAME.find({ runtime: {$gt: 90, $lt: 120} })
```

匹配出符合在数组中任意特定值的文档
```bash
> db.COLLECTION_NAME.find({ rated: { $in: [value1, value2] } })
```

**Element**

|Name|Description|
|:----|----:|
|$exists| 匹配出存在该字段的文档 |
|$type| 选出字段是指定类型的文档 |

$type query:
```bash
> db.COLLECTION_NAME.find({"key" : {$type: "value_type"}})
```

值类型：
> double(1), string(2), object(3), array(4), binary data(5), object id(7), boolean(8), Date(9), null(10)

**Logical operator**

|Name|Description|
|:----|----:|
|$or|或|
|$and|与|
|$not|非|
|$nor|都不|

```bash
> db.COLLECTION_NAME.find({ $nor: [ { condition1 }, { condition2 } ] })
```

$and 使用于同一个字段比较多。

**使用正则表达式**

```bash
> db.COLLECTION_NAME.find({ "key" : { $regex: /reg_expression/ } })
```
