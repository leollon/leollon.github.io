---
title: MongoDB - CRUD_C&R(1)
date: 2017-09-07 16:20:54
categories: NoSQL
tags: [MongoDB, notes]
---

数据库的操作---Create, Read, Update, Delete

## MongoDB 帮助文档
```bash
$ mongo # mongo shell
```
在mongo shell中输入`help`命令

```bash
> help
```

<!-- more -->

## 创建数据库
```bash
> use video
```
即使不存在这个数据，也没什么关系。当向第一个集合中插入一个文档的时候，这个数据库就创建好了。

## 创建文档(CRUD ---> C(Ceate)

1. 创建一个文档

```bash
> db.movies.insertOne({"title":  "Jaws", "year": 1975, "imdb": "tt0073195"})
{
        "acknowledged" : true,
        "insertedId" : ObjectId("59b100ccacf5accda2c1152f")
}
```

2. 创建多个文档

```bash
> db.movies.insertMany([{<document1>}, {<document2>}])
```

未指定document ID的情况下，自动生成一个唯一 underscore ID值， BSON支持这个数据类型。
如上面显示insertedId，则是该文档的 document ID。


## 读取文档(CRUD ---> R(Read))

```bash
> db.movies.find()
> db.movies.find().pretty()
```

- find() 查询所有
- pretty() 格式化查出来的数据

按照指定的字段进行查询：

```bash
> db.movies.find({"year": 1975}).pretty()
{
        "_id" : ObjectId("59b100ccacf5accda2c1152f"),
        "title" : "Jaws",
        "year" : 1975,
        "imdb" : "tt0073195"
}
```

**find()的返回值不仅是简单的文档数据，还是一个cursor object，因此我们可以在mongo shell中将返回值赋值给一个对象。mongo shell是一个全功能的JavaScript解释器。**
例如：

```bash
> var c = db.movies.find({})
> c.hasNext()
true
> c.next()
```

- c.hasNext() 返回值为true，说明这个cursor指向的地方还有文档可以继续读取。
- c.next()  返回当前cursor执行的文档
