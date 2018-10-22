---
title: '[翻译]mongoengine-docs-turtorial-zh_CN'
date: 2017-12-22 15:32:18
categories: 翻译
tags: [mongoengine, MongoDB, 翻译, 文档]
---

教程
========

   这篇教程使用例子来介绍**MongoEngine** --- 也就是我们会通过如何建立一个简单的**轻博客**
应用来介绍。一个轻博客是一个支持多媒体内容的博客，其中包含文字，图片，链接，视频，音频等等。
简而言之，我们的这个博客的内容将会是包含文字，图片，还有链接条目。鉴于这篇教程的目的适用于介绍
MongoEngine的，我们将注意力集中在这个博客应用的数据模型化这个方向上，省去了用户接口。


开始
=========

   在我们开始之前，确保MongoDB运行在一个可以访问的主机上面，运行在本地相对来说会更加容易，但是如果
实在是没有办法了，也可以运行在远端服务器上面。如果你还没有安装MongoEngine，像下面这样子轻松地使用
`pip`来安装：

```bash
$ pip install mongoengine
```

   在我们开始使用MongoEngine之前，我们需要做的就是如何连接程序`mongod`服务器。这可以使用
`mongoengine.connect()`函数来连接。如果本地运行成功，只需要我们提供的参数就是我们要MongoDB
使用的数据库名字：

```python
from mongoengine import *
connect('tumblelog')

```

连接MongoDB还有很选项，想了解更多关于它们的信息，可以阅读[Connecting to MongoDB](http://docs.mongoengine.org/guide/connecting.html#guide-connecting)。

定义文档
===========

   MongoDB 是*无模式*的，意味着数据库不强制使用任何模式 --- 我们可以随意添加和移除字段，
并且MongoDB都会照做。
   这使得你的开发过程更加容易，尤其是当数据模型中发生变化的时候。然而，当涉及错误类型或者字段丢失的
时候，定义文档的模式有助于查找bug，并且还允许使用和传统的`ORMs (Object-Relational Mappers)`一样的
方式定义关于文档的实用方法。

   在我们的轻博客应用中，需要存储几种不同类型的信息。我们需要**users**集合(collection)，为
的是我们能够关联**posts(博文)**到一个用户。我们还需要在数据库中存储不同类型的**posts(博文)**
(例如： 文本，图片，还有链接)。为了生成这个轻博客的导航，博文需要用**tags(标签)**和它们相关联起来，
为的是，向用户展示的博文列表可以限定为指定的标签。最后，如果可以为对博文进行进行**comments(评论)**将会更好。
我们以**用户(users)**文档为起点，接着再慢慢地引入其他文档模型。

用户(Users)
----------
   就好像我们通过ORM使用关系数据库一样，我们需要定义一个类(class)：`User`有哪些字段，并且它们可能
存储的数据是什么类型的：

```python
class User(Document):
    email = StringField(required=True)
    first_name = StringField(max_length=50)
    last_name = StringField(max_length=50)
```

   这看起来像是在正常的ORM中定义一个表的结构。关键不同的地方在于，这个模式(schema)将不会传递给
MongoDB --- 这个只是在应用程序层级上强制使用的一种数据组织方式，使得未来发生改变的时候，能够容易更改。
当然，用户(User)文档将会以MongoDB**集合(collection)**而不是表的形式进行存储。

博文(Posts), 评论(Comments)和标签(Tags)
-------------------------------------

   现在我们将开始考虑如果存储剩下的信息。如果我们使用关系数据库的话，我们最有可能分别有**博文(posts)**，
**评论(Comments)**,**标签(Tags)**等三个数据表。为了将评论和每一篇博文联系起来，我们会在评论
数据表中增加一列来存储博文数据表的外键。
我们还需要一个连接表在博文和标签之间提供多对多(many-to-many)的关系。然后，我们需要处理存储特殊
的博文类型(text, image, link)。这里有几种方式可以达到这样子的目的，但是，它们都存在各自的问题，
并不都是很好的解决方式。

博文(Posts)
^^^^^^^^^^^

   值得高兴的是，MongoDB并不是一个关系数据库，所以我们不会用那种方式来处理。最终的结果是，我们可以
使用MongoDB的无模式(schemaless)这个特性，提供给我们一个更好的解决方式。我们将存储所有的博文在
**一个集合(one collection)**中，并且每篇博文类型只存储它所需要的字段。
如果以后我们想要增加关于视频的博文，我们也没必要再修改集合(collection)，我们只是需要*使用*新的
字段来支持关于视频的博文。这样子能够很好地体现出面向对象的继承特性。我们可以考虑将一个类名为`Post`
作为基类，并且将类名分别为`TextPost`，`ImagePost`和`LinkPost`这三个类型作为`Post`类型的子类。

事实上，MongoEngine支持这种类型的数据模型 --- 你需要做的就是在属性`meta`中设置`allow_inheritance`为`True`就行了：

```python
class Post(Document)
    title = StringField(max_length=120, required=True)
    author = ReferenceField(User)

    meta = {'allow_inheritance': True}

class TextPost(Post):
    content = StringField()

class ImagePost(Post):
    image_path = StringField()

class LinkPost(Post):
    link_Url = StringField()
```

   我们使用一个属于`~mongoengine.fields.ReferenceField`类的对象来存储博文作者的引用。这个和
传统的ORMs使用外键字段相似，并且在进行被存储的时候会自动的转成引用(references)，当被加载(loaded)
的时候，自动地解引用(dereferenced)。

标签(Tags)
^^^^^^^^^

   如今我们已经创建出了博文(Post)模型，现在是时候考虑如何将标签(Tags)与博文进行关联？MongoDB允许我们直接
在每篇博文中直接使用一个列表来存储那些标签。那么为了效率与简洁，我们将在每篇博文中以字符串的形式直接
存储那些标签，而不是在单独的集合(collection)中存储那些标签的引用。尤其是当标签非常短(相比每个文档的id来说，甚至更短)，
这样子的非规范化的设计对于数据库的大小不会不会产生重大的影响。现在来看我们更改后`Post`类：

```python
class Post(Document):
    title = StringField(max_length=120, required=True)
    author = ReferenceField(User)
    tags = ListField(StringField(max_length=30))
```

   使用类`~mongoengine.fields.ListField`的对象来定义博文的标签，这个类将一个字段对象作为它的
第一个参数，这个意味这你可以放任意类型(包括列表)的字段的列表。

需要**注意**的是：我们不需要修改特定类型的博文类型，因为它们都继承类`Post`。

评论(Comments)
^^^^^^^^^^^^^

   每次评论都是与一片博文进行关联。在关系数据库中，为了显示一篇博文的所有的评论，我们会从数据库中获
取博文，然后再次从数据中查询与这篇博文相关联的评论。这样子做确实能正常工作，但是毫无理由将和文章
相关联的评论进行分开存储，除非是正在解决关系模型问题。

   使用MongoDB，我们可以使用一个**嵌入式文档(embbeded documents)**的列表将评论直接存储在
每篇博文文档中。单个嵌入式的文档应该和普通的文档不同。数据库中没有它们自己(所有评论)的集合(collection)。
    使用MongoEngine，我们可以定义嵌入式文档的结构，以及实用方法，用我们定义普通文档时使用的方法一样：

```python
class Comment(EmbeddedDocument):
    content = StringField()
    name = StringField(max_length=120)
```
然后就可以在我们每篇博文文档中存储评论列表了：

```python
class Post(Document):
    title = StringField(max_length=120, required=True)
    author = ReferenceField(User)
    tags = ListField(StringField(max_length=30))
    comments = ListField(EmbeddedDocument(Comment))
```

处理引用被删除情况
(handling deletions of references)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

   如果引用会被删除，类`~mongoengine.fields.ReferenceField`对象得携带一个关键字`reverse_delete_rule`来应对
执行删除规则的操作。如果一个用户会被删除，并且删除他的所有博文，需要像下面一样设置规则：

```python
class Post(Document):
    title = StringField(max_length=120, required=True)
    author = ReferenceField(User, reverse_delete_rule=CASCADE)
    tags = ListField(StringField(max_length=30))
    comments = ListField(EmbeddedDocument(Comment))
```

阅读类`mongoengine.fields.ReferenceField`来获取更多的信息。
需要**注意**的是： MapField 和 DictFields目前还没有支持自动处理引用被删除的情况。

给轻博客添加数据
==============

如今已经定义了文档的结构，现在开始想数据库中添加一些文档。首先，需要创建一个`User`类对象：

```python
ross = User(email='ross@example.com', first_name='Ross', last_name='Lawley')
```

**注意**
  我们也可以使用属性的语法来定义用户：
  ```pythonn
  ross = User(email='ross@example.com')
  ross.first_name = 'Ross'
  ross.last_name = 'Lawley'
  ross.save()
  ```

定义一个名为``john``的变量名生成另外一用户，就像我们上面定义``ross``变量一样。

现在，在数据库中已经存在用户了，那么让我们来添加几篇博文：

```python
    post1 = TextPost(title='Fun with MongoEngine', author=john)
    post1.content = 'Took a look at MongoEngine today, looks pretty cool.'
    post1.tags = ['mongodb', 'mongoengine']
    post1.save()

    post2 = LinkPost(title='MongoEngine Documentation', author=ross)
    post2 = link_url = 'http://docs.mongoengine.com/'
    post2.tags = ['mongoengine']
    post2.save()
```

**注意**：
  如果改变已经保存了的对象中的一个字段并且还调用了`save`方法，那么这个文档就会被更新。

读取数据
=======

   现在在数据库中已经有几篇博文了，那么，我们如何显示它们呢？每个文档类（即，任何类直接集成或非
直接继承类`~mongoengine.Document`)都有一个名为`objects`的属性，这个属性是用来在数据库集合
中读取和那个类相关联的文档的。

```python
for post in Post.objects:
    print(post.title)
```

获取特定类型的信息
---------------

这将会输出我们博文的标题，每行一个标题。然而，倘若我们想要读取特定类型的数据(link_url, content, etc.),
又该怎么办呢？其中一种方式就是简单地使用`Post`的子类的`objects`属性了：

```python
for post in TextPost.objects:
    print(post.content)
```

使用`TextPost`类的`objects`属性只会返回使用类`TextPost`创建的文档。实际上，这里还有更多通
用规则：
任何`~mongoengine.Document`的子类的`objects`属性只会查询创建文档时所使用的那个子类或者是
所有子类中一个子类。

   那么我们如何展示出我们所有的博客文章，或者只展示每篇博文特定类型的信息呢？这里有比只是单独使用
子类属性更好的方式。当我们使用之前`Post`类型的`objects`属性的时候， 返回的的对象实际上并不是
`Post`类型的实例 --- 这些返回的对象是`Post`类的子类的实例，这些子类是匹配了博文所属的类行的。
让我们来看一下实际中是如何工作的：

```python
for post in Post.objects:
    print(post.title)
    print('=' * len(post.title))

    if isinstance(post, TextPost):
        print(post.content)
    
    if isinstance(post, LinkPost):
        print('Link: {}'.format(post.link_url))
```
这将会打印每篇博客中的文章的标题，紧接着，倘若是文本博文，就是分别会是是文章的内容，如果是链接博
文，则是"Link: <url>"。

根据标签查找博文
-------------

   实际上，`~mongoengine.Document`类的`objects`属性是`~mongoengine.queryset.QuerySet`
的一个对象。这是延迟(lazily)进行数据库查询，只有当你需要从数据库获取数据的时候。这些数据有可能
被过滤，进而缩小查询结果。现在调整查询，为了只返回含有"mongodb"标签的的博文：

```python
    for post in Post.objects(tags='mongodb'):
        print(post.title)
```

   `~mongoengine.queryset.QuerySet`类的对象还有其他方法可用来返回不同的查询结果，例如，
调用`objects`属性的`first`方法，将会返回单独的一个文档，第一个文档是根据提供的查询而返回的。
还可以在`~mongoengine.queryset.QuerySet`对象上调用聚集(aggregation)函数：

```python
num_posts = Post.objects(tags='mongodb').count()
print('Found {} posts with tag "mongodb"'.format(num_posts))
```

更进一步学习关于MongoEngine
-------------------------

如果你学到了这里，已经是一个很好的开始了，所以干得漂亮，继续加油！接下来你将要学习MongoEngine
的`full user guide <guide/index.html>`, 那里将可以更深入地学习到如何使用MongoEngine和
MongoDB。
