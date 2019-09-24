---
title: Classic Services
date: 2019-09-23 22:19:32
categories: Sams-Teach-yourself-TCP/IP-in-24-hours
tags: [notes, TCP/IP, Networks]
---

本文目的是学习：

- HTTP
- Email
- FTP file transfer
- File and print services
- LDAP
- IRC and IM messaging

## HTTP

在World Wide Web中心的HTTP（Hypertext Transfer Protocol）本质上是用于以超文本标记语言（Hypertext Markup Language， HTML）格式来传输以及请求数据和图片的应用层协议。一个web服务器首先是一个HTTP服务器。

## Email

标准的邮件依赖于一对服务器系统，这也就是为什么需要在邮件阅读器配置界面配置一个“接收服务器”以及一个“发出服务器”的原因。使用简单邮件传输协议（SMTP）的发送服务器从用户接收发送出去的消息并且通过其他SMTP服务器网络转发到目的地址。通常使用POP或IMAP协议的接收服务器接收发送到用户邮箱帐号的信息并且等待用户的邮件阅读器请求连接并且访问消息。

## FTP

广泛使用的文件传输协议（File Transfer Protocol）让用户能够在TCP/IP网络上的计算机之间进行传输文件。用户在一台计算机上运行FTP客户端，并且在另外一台计算机上运行FTP服务器应用程序，比如UNIX/Linux计算机上的ftpd（FTP 守护程序）或其他平台上的FTP服务。FTP最初是用于传输文件，尽管它能够执行其他比如创建目录，删除目录以及列出文件的功能。

FTP使用TCP协议并且因此通过一个在客户端计算机和服务器计算机之间的可靠的，面向连接的会话运行。服务器上标准的FTP守护程序使用TCP端口21监听来自客户端的请求。

在UNIX世界中，一个守护程序使用运行在后台的一个进程并且当请求服务时提供服务。在Windows世界中，一个守护程序被称为一个服务。

当使用FTP传输文件的时，必须向FTP指定将要传输的文件的文件类型。最常见的选项是二进制和ASCII。当传输的文件是简单的文本文件时，使用ASCII。当传输的文件使用程序文件，word文档或一个图形文件时选择二进制。默认的文件传输模式是ASCII。

在本地计算机上的当前目录开始FTP会话时，当前目录是文件传输过程中的默认目录。
