---
title: MongoDB-Performance-IndexRelated-(2)
date: 2017-11-12 11:26:00
category: MongoDB
tags: [MongoDB, notes, nosql]
---

# Choosing an index

检查查询中的字段，加上一些其他信息比如排序使用的字段等信息。然后根据字段进行索引的匹配，相似的索引字段将用来进行查询，最优的查询索引将存入缓存中，提供给下一次相似查询使用。


如果使用索引进行查询的同时，又对结果进行排序，那么排序中的字段的排序顺序要么跟索引的一样，要么是相反方向的排序。也就是说，如果索引中的每个字段使用的升序，那么查询的时候，排序方法的中的每个字段排序要么使用升序进行排序，要么使用降序排序。

# Index Size
索引存储在内存中，是working set(在内存中存储)的一部分。所谓working set 就是客户端频繁浏览的数据。

查询Index的size

```bash
> db.COLLECTION_NAME.stats();
> db.COLLECTION_NAME.totalIndexSize();
```

# Geospatial Indexes/Spherical

## Indexes(2d)

```bash
> db.COLLECTION_NAME.find({
    locaction: {$near : [x, y]
    }
});
```

## Spherical(2dsphere)

```bash
> db.COLLECTION_NAME.find({
    loc: {
        $near: {
            $geometry: {
                type: "Point",
                coordinates: [-130, 39]
            },
            $maxDistance: 1000000
        }
    }
});
```

# Text Indexes
    全文本搜索匹配

创建该字段的索引

```bash
> db.COLLECTION_NANE.createIndex({field_name: 'text'})
```

全文本匹配查找
```bash
> db.COLLECTION_NAME.find({$text: {$search: value}})
```
如果字段中的值中都含有$search 操作符对应的value，则全部返回查询结果。

# 使用Hint方法
在根据索引进行查询的时候，可以使用`Hint`方法指导数据库使用指定的索引进行查询。

```bash
> db.COLLECTION_NAME.find().sort().
```