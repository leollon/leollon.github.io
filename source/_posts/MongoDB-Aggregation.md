---
title: MongoDB-Aggregation
date: 2017-12-07 12:02:39
categories: MongoDB
tags: [MongoDB, nosql, notes]
---

## 简单聚合查询
  > 使用$group、 $sum进行计数统计
```bash
> db.COLLECTION_NAME([
  {"$group": 
      {"_id":"($key|{"field1":"$key1", "field2":"$key2", ...}),
       "name":{$sum:(1|"$field")}
      }
  }])
#分组统计key的个数
```

## aggregation pipeline
  > 向各个操作符(operator)传递文档

### operator
  1. $project(最后显示的数据的结构)
  2. $match(过滤， 放在最前面， 会使用索引)
  3. $group(聚合， 分组)
  4. $sort(排序，会使用索引)
  5. $skip(跳过数据数量)
  6. $limit(显示数据数量)
  7. $unwind(扩充数据)
     > 如处理tags: ["red", "green", "blue"]后的数据

     tags: "red"
     
     tags: "green"

     tags: "blue"
  8. $out(输出重定向输出到文档中)

### 表达式(Expression)
  1. $sum
  2. $avg(计数自动进行)
  3. $min
  4. $max
  5. $push(创建数组)
  6. $addToSet(创建数组，added one by one)
  7. $first
  8. $last


### 全文搜索($text)
  > If using the $text operator in aggregation, the following restrictions also apply.

  > The $match stage that includes a $text must be the first stage in the pipeline.
    A text operator can only occur once in the stage.
    The text operator expression cannot appear in $or or $not expressions.
    The text search, by default, does not return the matching documents in order of 
    matching scores. Use the $meta aggregation expression in the $sort stage.

```bash
> db.COLLECTTION_NAME.aggregate([
  {$match:
      {$text: {$search: "<value>"}}
  },
  {$project: {key: (1|0)}}
])
```

### 排序($sort)
  > 1. aggregation 均支持disk and memory based sorting。在内存进行排序默认使用的空间是100megabytes。
  2. 可以在`$group`之前或之后使用。
```bash
> db.COLLECTION_NAME.aggregate([{$sort: {"key1":1, "key2": -1}}])
```

### Aggregation Options

  1. explain - query plan (value: boolean, used in mongo shell)
  2. allowDiskUse - 100MB(default sorting memory based) (value: boolean, use in python)
  3. cursors - value: ({}, used in python)

### SQL to Aggregation Mapping
|SQL|MongoDB|
|:----|----:|
|WHERE|$match|
|HAVING|$match|
|SELECT|$project|
|ORDER BY|$sort|
|SUM()|$sum|
|COUNT()|$sum|

### Limitations in Aggregation

- 100MB limit pipeline, use allowDiskUse=True to handle it
- return 16MB single document by default in Python, use cursor={} to handle it
- sharded, brought back the results in the first shard with `group` and `sort`


### $substr
  返回某字段值的子字符串
  > {key: {$substr: ["$field", beg, end]}}

返回的字符个数是end-beg。
```bash
> db.getCollection('zips').aggregate([
     {$project: { first_char: {$substr: ["$city",0, 1]},
                  pop: "$pop"
                }
     },
     {$match: {'first_char': {$in: ['B', 'D', 'O', 'G', 'N', 'M'] } } },
     {$group: {'_id': null, 'totle': {$sum: "$pop"} } }
  ])
```