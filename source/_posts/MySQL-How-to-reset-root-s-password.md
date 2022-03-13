---
title: MySQL - How to reset root's password
date: 2018-01-16 22:13:40
categories: MySQL
tags: [notes, 笔记, MySQL]
---
刚开始的时候方法：

### 结束掉所有的mysql进程
```bash
$ ps -ef | grep mysql | awk '{print $2}' | xargs sudo kill -9
# 或者
$ sudo killall mysqld
```
### 重新以安全模式运行MySQL守护进程
```bash
$ sudo mysqld_safe --skip-grant-tables
```
因为到这里我使用上述命令命令并没有能成功运行MySQL守护进程

所以就有了下面的步骤
### 创建mysql service 目录
```bash
$ sudo mkdir /var/run/mysqld
# 使该目录属于MySQL用户和组
$ sudo chown mysql:mysql /var/run/mysqld
```
### 使用上面结束MySQL进程的方法结束进程
> 不在赘述

### 使用root用户登录Debian 8
根据官方给的方法是
### 创建一个包含更改MySQL root新密码的MySQL 语句的文件
文件内容如下：
**MySQL 5.7.6 及 以上**：
```mysql
ALTER USER 'root'@'localhost' IDENTIFIED BY 'YourNewPassword';
```
那么我将文件的内容命名为`pass`，你可随便取名。
**[MySQL 5.7.5 及以下，参考这里](https://dev.mysql.com/doc/refman/5.7/en/resetting-permissions.html)**
### 启动MySQL 服务器
```bash
$ mysqld --init-file=/path/to/pass &
# 但是并不能有什么作用，它会告诉你去RTFD(Read The Fxxking Documents) 233
# 简单详尽的意思就是，叫你去读MySQL `security` section，找出用系统的root用户运行`mysqld`的步骤。
# 到这里时，已经做了前两个步骤了，下面是第三步。
$ mysqld_safe --init-file=/path/to/pass &
```
到这里，已经设置好了新的MySQL root用户的密码了。

再次结束MySQL进程，然后使用普通的方式来启动MySQL 服务器
```bash
$ sudo service mysql start
$ sudo systemctl start mysql.service
```

## 曾经 MySQL 重新设置root密码的方式，我是像下面这样子做的
```bash
$ sudo killall mysqld
$  sudo /usr/sbin/mysqld -u root --skip-grant-tables
$ mysql -u root -p  #  另起一个终端执行
mysql > use mysql;
mysql > update user authentication_string=Password('new password') WHERE user='root';
mysql > FLUSH PRIVILEGES;
mysql > exit;
$ sudo killall mysqld
$ sudo service mysql start
```

本文(完)

## 参考过以下文章
[Reset the MySQL 5.7 root password in Ubuntu 16.04 LTS](https://coderwall.com/p/j9btlg/reset-the-mysql-5-7-root-password-in-ubuntu-16-04-lts)
[How to Reset the Root Password](https://dev.mysql.com/doc/refman/5.7/en/resetting-permissions.html)






