---
title: Name Resolution
date: 2019-08-24 22:20:28
categories: Sams-Teach-yourself-TCP/IP-in-24-hours
tags: [notes, TCP/IP, Networks]
---

本文目的：

- 解释如何进行域名解析
- 解释主机名（hostname），域名和FQDNs的区别
- 描述主机名解析
- 描述DNS名字解析
- 描述在某些遗留微软网络上使用的NetBIOS名字解析

## What is Name Resolution

主机名系统是在TCP/IP历史上早期开发的简单的名字解析技术。系统中，每台计算机被分配给一个称为**主机名**的含有字母数字的名字。如果操作系统希望是一个IP地址的时候却碰上一个字母数字名字，则操作系统会查询一个**hosts文件**。hosts文件是一个包含主机名和IP地址关联的列表。如果字母数字名字在主机名列表上，计算机读取和名字相关联的IP地址。然后计算机使用相关的IP地址替换命令中主机名并且执行命令。

![93625dcb59ed467d.svg](https://i.quantuminit.com/93625dcb59ed467d.svg)


