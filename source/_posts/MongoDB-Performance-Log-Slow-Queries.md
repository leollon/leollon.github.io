---
title: MongoDB-Performance-Log_Slow_Queries
date: 2017-11-13 16:56:35
categories: MongoDB
tags: [MongoDB, nosql, notes]
---


# Log Slow Queries

MongoDB 默认在日志中记录下查询时间大于100ms的慢查询。

当尝试执行查询的时候，可以在终端中使用下述命令：
```bash
$ sudo tail -f /var/lob/mongodb/mongod.log # 这个可以滚动地查看日志的变化情况
```

在此之前，如果根据某个字段进行Query之前，已经创建了该索引，那么日志中可能就没有记录下这个查个慢查询了。

偶然发现，如果将之前的索引都删除了，然后再进行查询的时候，第一次查询耗时都是挺长的，后续如果再根据这个字段进行查询的时候，耗时相对减少了许多。

这里是日志：
>2017-11-13T17:38:02.130+0800 I COMMAND  [conn2] command school.students command: find { find: "students", filter: { student_id: 99999.0 } } planSummary: COLLSCAN keysExamined:0 docsExamined:1000000 cursorExhausted:1 keyUpdates:0 writeConflicts:0 numYields:7814 nreturned:10 reslen:2676 locks:{ Global: { acquireCount: { r: 15630 } }, Database: { acquireCount: { r: 7815 } }, Collection: { acquireCount: { r: 7815 } } } protocol:op_command 862ms
2017-11-13T17:38:35.757+0800 I COMMAND  [conn2] command school.students command: find { find: "students", filter: { student_id: 99999.0 } } planSummary: COLLSCAN keysExamined:0 docsExamined:1000000 cursorExhausted:1 keyUpdates:0 writeConflicts:0 numYields:7812 nreturned:10 reslen:2676 locks:{ Global: { acquireCount: { r: 15626 } }, Database: { acquireCount: { r: 7813 } }, Collection: { acquireCount: { r: 7813 } } } protocol:op_command 346ms
2017-11-13T17:38:43.311+0800 I COMMAND  [conn2] command school.students command: find { find: "students", filter: { student_id: 99999.0 } } planSummary: COLLSCAN keysExamined:0 docsExamined:1000000 cursorExhausted:1 keyUpdates:0 writeConflicts:0 numYields:7812 nreturned:10 reslen:2676 locks:{ Global: { acquireCount: { r: 15626 } }, Database: { acquireCount: { r: 7813 } }, Collection: { acquireCount: { r: 7813 } } } protocol:op_command 348ms
2017-11-13T17:38:48.133+0800 I COMMAND  [conn2] command school.students command: find { find: "students", filter: { student_id: 99999.0 } } planSummary: COLLSCAN keysExamined:0 docsExamined:1000000 cursorExhausted:1 keyUpdates:0 writeConflicts:0 numYields:7812 nreturned:10 reslen:2676 locks:{ Global: { acquireCount: { r: 15626 } }, Database: { acquireCount: { r: 7813 } }, Collection: { acquireCount: { r: 7813 } } } protocol:op_command 346ms

上述时间862ms(1st time), 346ms(2nd time),后面的都是一次进行的。这个大概可以说明之前说的经过第一次查询之后，这个最优查询方式已经在内存中缓存起来了吧。为什么呢？
之前有说到[working set](https://quantuminit.com/MongoDB-Performance-IndexRelated-2/)这个概念，所以后来的查询结果放在了内存中了。