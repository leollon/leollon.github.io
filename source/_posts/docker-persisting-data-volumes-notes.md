---
title: docker-persisting-data-volumes-notes
date: 2019-07-17 17:52:06
categories: Docker
tags: [Docker, notes, volumes]
---

数据卷是容器使用和生成持久化数据的偏好机制。`bing mounts`依赖主机的数据结构，数据卷完全由Docker管理。
数据卷的优点：
- 比起`bind mounts`，数据卷更容易备份或迁移
- 可以通过使用Docker CLI 命令或者Docker提供的API管理数据卷
- 数据卷都能在Linux和Windows容器中正常工作
- 数据卷更安全在多容器之间进行共享
- 数据卷驱动使得数据卷可以在运程主机或云提供商上进行存储，也可以加密数据卷的内容，或添加其他功能。
- 新数据卷能够被一个容器预填充

除此之外，比起在容器的读写层持久化数据，使用数据卷是更好的选择，因为数据卷不会增大使用它的容器的大小，并且数据的生命周期不依赖于一个容器。
使用[tmpfs mount](https://docs.docker.com/storage/tmpfs/)存储容器生成的非持续化状态的数据，来避免永久存储这些数据在任何其他地方。
通过避免写进容器可写层来提供容器的性能。


当在compose file中要使用数据卷来存持久化数据时，将主机的目录映射容器中的目录时主机上的目录需已经存在，否则，在build image时，在主机上生成的目录是属于root用户，
容器中的应用不能对该目录映射的容器中的目录进行写操作。

