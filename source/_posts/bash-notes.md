---
title: the art of command line 笔记整理
date: 2019-06-02 12:39:02
categories: Notes
tags: [bash, notes, shell]
---

- `man` 用来阅读命令的官方文档， 比如 `man` `man`

      命令的分区号
      ![分区号](https://i.quantuminit.com/eed7d1556634417e.png)
      - 1 普通命令
      - 5 文件/约定
      - 8 系统管理员命令
- help` 和 `help -d`可得知一个命令是否是可执行程序。`shell`内置命令或者别名，使用`type command`，例如 `fg`, `bg`, `jobs`等。
- `curl cheat.sh/command`会给出如何使用shell命令常用例子的表单。

重定向
  - `>`: 覆盖输出
  - `<`: 输入
  - `|`: 管道
  - `>>`: 末尾添加

一些标点符号
  - `*`：通配符
  - `?`：单个字符匹配符号
  区别：
    - `"`：双引号
    - `'`：单引号

任务管理
  - `&`
  - ctrl-z
  - ctrl-c
  - `jobs`
  - `fg`
  - `bg`
  - `kill`

ssh
  - `ssh-agent`
  - `ssh-add`

基本文件管理
  - `ls -l`, `ls`
  - `less`, `less +F`
  - `head`
  - `tail`, `tail -f`
  - `ln -s`
  - `chown`
  - `chomd`
  - `du`, `du -sh *`, 磁盘使用情况
  - `df`， `mount`， `fdisk`, `mkfs`, `lsblk`,用于文件系统管理，inode相关，`ls -i`，`df -i`。
  - `ip`, `ifconfig`, `dig`, `traceroute`, `route`, 网络管理
  - `git`,版本控制系统
  - `grep`，`egrep`，以及它们的`-i`, `-o`, `-v`, `-A`, `-B`, `-c`选项。
  - `apt-get`, `yum`, `dnf`, `pacman`，各个linux发行版的包管理工具，`pip`，`Python`包管理命令
