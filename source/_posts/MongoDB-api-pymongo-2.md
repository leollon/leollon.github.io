---
title: MongoDB-api-pymongo(2)
date: 2017-09-22 11:48:17
categories: Python
tags: [MongoDB, notes, pymongo]
---

## Use sort(), skip(), limit() 方法。

只有当遍历查询结果的时候，正是查询的开始。sort先运行，然后skip，最后是limit。

<!--more-->

```python
import pymongo

connection = pymongo.MongoClient('mongodb://localhost')

db = connection.school
scores = db.scores

def find():
  query = {}
  try:
      cursor = scores.find(query).skip(4)
      cursor.limit(1)
#     cursor.sort('student_id', pymongo.ASCENDING).skip(4).limit(1)
      cursor.sort([('student_id', pymongo.ASCENDING),
                   ('score', pymongo.DES CENDING)])

  except Exception as e:
      print "Unexpected error:", type(e), e

  for doc in cursor:
      print doc
```

skip(n): 从当前位置跳过三个文档，取第n+1个文档。
JSON会获取键值对的顺序。sort函数使用tuple进行多个字段排序，tuple是有顺序的，而python的dict的键值对没有顺序。
pymongo.ASCENDING 升序排序
pymongo.DESCENDING 降序排序

## insert document

- insert_one
   > only insert a document at a times

- insert_many
   > insert many document at a times in order

  ```python
  db.COLLECTION_NAME.insert_many([document1, document2], ordered=True) # if no specified, ordered is default
  ```

## update a document
- update_one
   > update a document at a times

- update_many
   > update many documents at a times

底层都是调用_update()函数，只不过update_many多了一个multi=True参数

### upsert
   > default value is False, if specified True, match filter, then update document, or insert a new document

   ```python
   db.COLLECTION_NAME.update_one({filter}, {'$set': {'key': value}}}}, upsert=True)
   ```

如果在使用replace_one(replaceOne)的时候，同时还设置upsert参数为true，并且filter还使用了_id,则当不存在该_id的文档时，插入新文档时，同时使用这个_id为这个新文档的_id.


## delete document
- delete_one
- delete_many
