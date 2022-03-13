---
title: MongoDB-CRUD-Update
date: 2017-09-21 18:22:01
categories: MongoDB
tags: [MongoDB, notes]
---
# 更新数据库中的文档

更新方法有：
1. updateOne
2. updateMany
3. replace

<!-- more -->

## updateOne
```bash
> db.COLLECTION_NAME.updateOne({query_document}, { $set: {key: value} })
> db.COLLECTION_NAME.updateOne({ query_document }, { $inc: { key: value1, key: value2 } } })
```

使用字段更新操作符号

|Name|Description|
|:----|:----:|
|$inc|某个字段的值加上inc指定的值|
|$mul|某个字段的值乘上mul指定的值|
|$rename|重命名一个字段的名称|
|$setOnInsert|当更新影响到文档插入的时候，设置一个字段的值。|
|$set|在文档中设置/增加某个字段|
|$unset|移除文档中的某一字段|

[更多更新操作符](https://docs.mongodb.com/manual/reference/operator/update/)


## upsert

> upsert是在options中，默认为falase，如果为true，则插入与查询规则不匹配的新文档。

```bash
> db.movieDetails.updateOne(query, update, options)
```
# 删除文档

 ```bash
> db.COLLECTION_NAME.drop()
 ```