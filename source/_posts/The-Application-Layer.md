---
title: The-Application-Layer
date: 2019-08-14 11:02:28
categories: Sams-Teach-yourself-TCP/IP-in-24-hours
tags: [notes, TCP/IP, Networks]
---

## 本文目的

- 描述应用层
- 描述一些应用层的网络服务
- 列出一些TCP/IP重要的程序

## What Is the Application Layer

TCP/IP的应用层是各种网络感知软件组件，用于向TCP和UDP端口发送信息和从TCP端口接收信息。在逻辑相似或等同的意义上，这些应用层组件不是并行的。

## The TPC/IP Application Layer and OSI

TCP/IP的应用层与OSI的应用层，表现层以及会话层相关。

![bb3dcf7b5d06401e.svg](https://i.quantuminit.com/bb3dcf7b5d06401e.svg)

与TCP/IP应用层相关的OSI分层描述：

- Application layer: OSI 应用层有提供服务给用户应用以及支持网络连接的组件。
- Presentation layer: 表现层将数据翻译成平台中立的格式并且处理加密以及数据压缩。
- Session layer: 会话层管理网络上计算机应用之间的通信。

对于多有的应用程序以及实现，所有的这些服务都不是必需。在TCP/IP模型中，应用层的实现不要求遵循OSI细分的分层，但是总体来说，OSI的应用层，表现层以及会话层定义的职责都在TCP/IP应用层负责的范围内。

## Network Services

在此层之前，协议系统的一层为系统中其他层提供服务。这些服务协议系统定义好的，完整的一部分。在应用层，不是所有的服务都被需要用于协议软件的运行并且更有可能根据用户的直接利益来提供，或者与本地操作系统进行网络连接。应用层含有支持用户体验的各种各样的网络服务：文件服务，远程访问服务，邮件服务和HTTP web 服务。

应用层更重要的活动：

- [文件与打印服务](/#File-and-Print-Services)
- [域名解析服务](/#Name-Resolution-Services)
- [远程访问服务](/#Remote-Access)
- [Web服务](/#Web-Services)。

一些应用层协议的表：

|Protocol|Description|
|:----|:----|
|BitTorrent|A peer-to-peer file sharing protocol often used for fast download of large files on the Internet|
|Common Internet File System (CIFS)|An enhanced version of the SMB file service protocol|
|Domain Name System (DNS)|A hierarchical system for mapping Internet names to IP addresses|
|Dynamic Host Configuration Protocol(DHCP)|A protocol used for automatically assigning IP addresses and other network configuration parameters|
|File Transfer Protocol (FTP)|A popular protocol for uploading and downloading files|
|Finger|A protocol used for viewing and requesting user information|
|Hypertext Transfer Protocol (HTTP)|The communication protocol of the World Wide Web|
|Internet Message Access Protocol (IMAP)|A common protocol for accessing email messages|
|Lightweight Directory Access Protocol (LDAP)|A protocol used for implementing and managing information directory services|
|Network File System (NFS)|A protocol that provides a remote user with access to file resources|
|Network Time Protocol (NTP)|A protocol used for synchronizing clocks and other time sources over a TCP/IP network|
|Post Office Protocol (POP)|A protocol used for downloading email from a mail server|
|Remote Procedure Call (RPC)|A protocol that lets a program on one computer call a subroutine or procedure on another computer|
|Server Message Block (SMB)|A file and print service protocol|
|Simple Network Management Protocol (SNMP)|A protocol for managing network devices|

## File and Print Services

一个打印[服务器](/The-Transport-Layer/#TCP-Connections)操作一个打印机并且在那个打印机上完成打印文件的请求。
一个文件服务器操作一个数据存储设备，例如，硬盘，并且完成读取或写入数据到那个设备的请求。文件服务与打印服务是非常见
的网络活动，所以他们总是经常放到一起考虑。

诸如UNIX/Linux Network File System (NFS)，Microsoft的Common Internet File System (CIFS)以及Server
Message Block)都是在应用层运行，还有经典的文件传输工具File Transport Protocol (FTP) 以及 Trivial File 
Transfer Protocol (TFTP)。

文件服务场景示意图：

![fd822f26bd8d43c9.svg](https://i.quantuminit.com/fd822f26bd8d43c9.svg)

## Name Resolution Services

[Name resolution](/What-is-TCP-IP/#Name-Resolution)是映射预订义，用户友好的字母数字名字到IP地址的过程。Domain Name System (DNS)服务给互联网（Internet）和隔离的TCP/IP网络提供域名解析。DNS使用**name servers**来解析DNS域名查询。一个name server服务运行在name server计算机上的应用层并且与其他name server交换域名解析信息。还有其他比如NIS，NetBIOS以及一些与LDAP关联的域名服务的域名解析系统存在。

## Remote Access

应用就是让用户初始化从一台计算机到另一台计算机交互连接的技术集合之家。例如[SSH](/Classic-Services)让用户能够登
录到远程系统并且通过网络发送命令。为了将本地环境和网络集成在一起，一些操作系统使用一种称为**redictor**有时又称
requester的服务。Redirector在本地计算机中拦截服务请求并且检查是否这个请求应该在本地完成或是转发到网络上的另外一
台计算机。

![0ae5007c0d38412b.svg](https://i.quantuminit.com/0ae5007c0d38412b.svg)

例如，远程硬盘能够以本地硬盘出现在客户计算机上。

## Web Services

Hypertext Transfer Protocol (HTTP) 是我们所知的万维网生态系统的心脏的一部分。

## APIs and the Application Layer

一个application programming interface (API) 是一个应用用来访问其他部分操作环境的一个编程组件预订义集合。


