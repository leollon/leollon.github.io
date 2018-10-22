---
title: MongoDB-Performance-Profiler,Tools&Sharding
date: 2017-11-14 11:08:14
categories: MongoDB
tags: [MongoDB, nosql, notes]
---

# Profiler
> 对于任何查询耗时过长的，它会在`system.profile`中写入entries，documents。

等级(Level)：
 - 0: 默认等级，关闭该功能
 - 1: Log Slow Queries
 - 2: Log All Queries, 完全的调试功能

获取等级：

```bash
> db.getProfilingLevel()
```

获取profile记录:

```bash
> db.system.profile.find({}).pretty()
```

{}这里可以填入查询条件，比如某个集合()，或者字段

查询条件使用正则表达式:
```bash
> db.system.profile.find({ns:/school.students/}).sort({ts:1}).pretty()
> db.system.profile.find({millis: {$gt: 1000}}).sort({millis: -1})
```

获取ProfileStatus:
```bash
> db.getProfileStatus()
{ "was" : 1, "slowms" : 100 }
```

设置system.profile等级，以及慢查询的最小时间

这里将在数据库中生成system.profile的文档
```bash
> db.setProfilingLevel(level, time) # level 取0, 1，2, time 取值单位为ms
```

如果是system.profile需要drop，则先关闭profiling才行。

# Mongotop
查看MongoDB中各个文档读写耗时等，搭配`profiler`调试，真是极好的。
更为详细介绍还是找“男人(man)”吧。
```bash
$ man mongotop
```

# Mongostat
查看MongoDB使用资源的各项数据(Create, Read, Update, Delete, etc)
Get more detail, find the manual/docs
```bash
$ man mongostat
```

# Sharding

分布式系统设计

比较关键的是要决定这个shard key，最好是指定shard key，因为mongos可以根据这个shard key 去与哪个mongod进行通信，对数据进行操作，那如果是不指定，mongos将发起广播与所有的mongod进行通信。


## 参考(reference)
[Database Profiler Output](https://docs.mongodb.com/manual/reference/database-profiler/)
