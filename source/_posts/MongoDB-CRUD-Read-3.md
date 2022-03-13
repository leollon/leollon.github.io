---
title: MongoDB-CRUD-Read(3)
date: 2017-09-21 15:21:23
categories: MongoDB
tags: [MongoDB, notes]
---

## 使用数组进行查找

<!--more -->

1. $all
    > 在查询匹配数组中含有的所有元素

   ```bash
   > db.COLLECTION_NAME.find({"key" { $all : ["value1", "value2"]}})
   ```

2. $size
    > 选出文档中的数组含有指定多少个元素的文档

   ```bash
   > db.COLLECTION_NAME.find({ key : { $size: value}})
   ```
   value = 1,则表示数组中含有元素的个数为1，也就是数组的大小

3. $elemMatch

    > 如果数组字段中的元素同时符合`$elemMatch`中的条件，则选出该文档

    假设有如下文档：
      ```
      boxOffice： [ { "country": "USA", "revenue": 41.3 },
                    { "country": "Australia", "revenue": 2.9 },
                    { "country": "UK", "revenue": 10.1}]
      boxOffice： [ { "country": "USA", "revenue": 11.3 },
                    { "country": "Australia", "revenue": 22.9 },
                    { "country": "UK", "revenue": 12.1}]
      ```
      ```bash
      > db.COLLECTION_NAME.find({ boxOffice: {$elemMatch: { country: "UK", revenue: {$gt: 15} } } })
      ```
