---
title: log - VPS升了个级
date: 2019-07-30 09:51:24
categories: Debian
tags: [vultr, vps, upgrade-vps, buster]
---

原先vultr的vps一直用的`Debian jessie`，后面不知不觉的`jessie`的源在`update`的时候出现`404 Not Found`，这还了得，
但是呢，“别担心，问题不大”。`sed`一下，然后`update`就行了。这不现在都已经出`Debian baster`（误）了。ちがいます！`Debian buster`ですよ！

原本出现`404 Not Found`的时候，使用的源在`/etc/apt/sources.list`是这样子的：
```
deb http://archive.debian.org/debian/ jessie main
deb-src http://archive.debian.org/debian/ jessie main

deb http://security.debian.org/ jessie/updates main
deb-src http://security.debian.org/ jessie/updates main

# jessie-updates, previously known as 'volatile'
# deb http://archive.debian.org/debian/ jessie-updates main
# deb-src http://archive.debian.org/debian/ jessie-updates main

# docker
deb [arch=amd64] https://download.docker.com/linux/debian jessie stable
# deb-src [arch=amd64] https://download.docker.com/linux/debian jessie stable
# deb-src [arch=amd64] https://download.docker.com/linux/debian jessie stable
```
升级之前，先备份一下那些源文件，以防升级操作过程中，出现什么夭蛾子。
```bash
$ mkdir backup && sudo cp -r /etc/apt/* $_
```
完了之后，需要把`jessie`改为`buster`, archive更改为ftp.us，这就是所谓的换源了:
```bash
$ sudo sed -i "s/jessie/buster/g" /etc/apt/sources.list
$ sudo sed -i "s/archive/ftp\.us/g" /etc/apt/sources.list
$ sudo sed -i "s/jessie/buster/g" /etc/apt/sources.list.d/*  # 其他软件源也得换掉
```

[Debian的软件源镜像列表](https://www.debian.org/mirror/list)

到此，可以愉快的升级了，但是还有一些站点数据备份，之前`crontab`已经帮我做好了。
```
$ sudo apt-key net-update  # 更新过期gpg keys
$ sudo apt update && sudo apt upgrade
```
