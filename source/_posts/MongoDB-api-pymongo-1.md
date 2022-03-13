---
title: MongoDB-api-pymongo(1)
date: 2017-09-21 23:19:06
categories: Python
tags: [MonogoDB, notes, pymongo]
---

## MongoDB api 之 pymongo

安装pymongo
```bash
$ pip install pymongo
```

<!--more-->

Python使用pymongo连接数据库

```python
improt pymongo  # 导入pymongo包

# 连接mongodb

connection = pymongo.MongoClient("mongodb://localhost")

# 使用数据库
db = connection.DATABASE_NAME

# 使用集合
COLLECTION_NAME  = db.COLLECTION_NAME_IN_DATABASE_NAME

# 使用find函数返回cursor
def find():
    query = {'' : ''}
    # 返回cursor
    try:
        cursor = COLLECTION_NAME.find(query)
    except Exception as e:
      print("Unexpected error, ", type(e), e)
    # iterate
    for doc in cursor:
        print(doc)

# 使用find_one返回一个文档

def find_one():
    query = {'' : ''}
    try:
      doc = COLLECTION_NAME.find_one(query)
    except Exception as e:
      print("Unexpected error,", type(e), e)
```
