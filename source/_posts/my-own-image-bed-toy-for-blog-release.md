---
title: '[picbed]my-own-image-bed-toy-for-blog-release'
date: 2018-09-21 12:09:52
category: Flask
tags: [flask, picbed, imgbed, 图床]
---

此前开的一个坑，这次填上了。迫于穷，域名又不想备案，vps流量每个月500GB自用也就是十几G这样子而已，所以放着剩下的流量不用简直浪费。加上自己正想实践(梭)一把docker部署.

[用flask做个图床吧](https://quantuminit.com/%E7%94%A8flask%E5%81%9A%E4%B8%80%E4%B8%AA%E5%9B%BE%E5%BA%8A%E5%90%A7/)

<!-- more -->

![9959d97e729d41e3.png](https://i.quantuminit.com/9959d97e729d41e3.png)
![660b0825915b4dad.png](https://i.quantuminit.com/660b0825915b4dad.png)


解决问题过程中的链接

- [the-input-device-is-not-a-tty](https://stackoverflow.com/questions/43099116/error-the-input-device-is-not-a-tty)

   出现这个问题的初衷是自己使用docker已经build好后的imgage，但是当这个镜像跑起来之后，`container`里面的文档数据库除了必须的数据之外没有其他数据了，而使用`docker-compose`跑起相应的服务之后，代码中有数据库连接要验证用户的，即使刚开始的不用，但是后续要进行数据操作的时候，就会用到了。

  如`mongoengine`的[文档](http://docs.mongoengine.org/projects/flask-mongoengine/en/latest/)这么说道：
  >By default flask-mongoengine open the connection when extension is instanciated but you can configure it to open connection only on first database access by setting the MONGODB_SETTINGS['connect'] parameter or its MONGODB_CONNECT flat equivalent to False


- [no python application found, check your startup logs for errors](https://segmentfault.com/q/1010000004906014)

  **但我这里并不是地址被占用什么的，就只是启动uwsgi了，但是并没有load app进来。**

奇怪的事是，使用uwsgi跑这个图传代码的时候，不能使用uwsgi的thread选项，即使是使用了enable-threds也是不可以的，如果启用，项目跑起来后，浏览器中输入地址也是一直在转圈圈，但是如果是`django`项目的话，是可以启用这个thread选项,并且浏览器输入地址是可以访问。

关于这个thread option的一个[issue](https://github.com/unbit/uwsgi/issues/844#issuecomment-77080515)以及讨论，当时自己也是开启了这个选项，项目跑起来之后，使用`ctrl+c`结束进程的时候，得等后一会儿才会kill掉。

[uwsgi-docs - things to know](https://uwsgi-docs.readthedocs.io/en/latest/ThingsToKnow.html)
>By default the Python plugin does not initialize the GIL. This means your app-generated threads will not run. If you need threads, remember to enable them with enable-threads. Running uWSGI in multithreading mode (with the threads options) will automatically enable threading support. This “strange” default behaviour is for performance reasons, no shame in that.

在使用`docker`过程中涉及到的文档:

- [使用 Docker 部署 Django 应用的最佳实践是什么？
](https://www.v2ex.com/t/368982#r_4434176)
- [docker-compose](https://docs.docker.com/compose/django/)
- [dockerfile](https://docs.docker.com/engine/reference/builder/)
- [docker-compose-file](https://docs.docker.com/compose/compose-file/)