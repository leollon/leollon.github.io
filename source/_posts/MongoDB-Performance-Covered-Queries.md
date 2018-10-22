---
title: MongoDB-Performance-Covered_Queries
date: 2017-11-11 23:33:31
categories: MongoDB
tags: ['MongoDB', 'notes']
---

# Covered Queries

意思是说查询条件完全满足某一个索引，查询出来的结果显示的字段是跟查询字段能够一一对应得上的。

正确的覆盖查询：
```bash
> db.COLLECTION_NAME.find({field_1: value1, field_2: value2}, {_id: 0, field_1: 1, field_1: 1})
```

上面的查询条件是`{field_1: value1, field_2: value2}`, 需要查询结果显示的字段是`field_1`, `field_2`。但是结果又不能显示`_id`，因为`_id`不在我们的查询条件中。

`totalDocsExamined=0`是表明Covered Queries的关键字。