---
title: MongoDB - what is MongoDb
date: 2017-09-07 15:10:16
categories: NoSQL
tags: [MongoDB, notes]
---

MongoDB是一个文档型数据库，并使用JSON这种数据存储结构存储在MongoDB中。
类似如下数据：
```JSON
{
  "_id": "hello-world",
  "date": "2017-09-07T15:32:37",
}
```
MongoDB自身通过分片支持负载均衡，能够满足负载均衡和性能，还支持敏捷开发模式。

<!-- more -->

运行mongodb需要两个东西：
- MongoDB的守护进程： mongod
- 进行数据操作的命令行客户端： mongo

## 在ubuntu 16.04上面安装并运行MongodDB：

```bash
$ sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv EA312927 # 安装这个软件的key
$ sudo echo "deb https://mirrors.tuna.tsinghua.edu.cn/mongodb/apt/ubuntu trusty/mongodb-org/stable multiverse" >> /etc/apt/sources.list.d/mongodb.list # 添加MongoDB的软件源
$ sudo apt-get update # 更新软件源
$ sudo apt-get install mongodb-org -y # 安装mongodb数据库
```

## 系统根目录创建mongodb的数据库默认存储目录：
```bash
$ sudo mkdir -p /data/db
```

## 运行MongoDB

打开一个终端，执行
```bash
$ sudo mongod
```

另打开一个终端
```bash
$ mongo  # mongo shell
```

JSON objects 使用键值对，即key:values;
- key 一定是字符串
- value 类型可以是string,boolean,array,number

[了解更过JSON](http://json.org)

**MongoDB实际上使用BSON进行数据存储，BSON--> binary JSON**

应用与MongoDB之间有一个数据库驱动，这个数据库驱动编码过JSON并存储到MongoDB中，从MongoDB中读取BSON，然后解码，变成JSON返回给应用程序。

[了解更多BSON](http://http://bsonspec.org/)
