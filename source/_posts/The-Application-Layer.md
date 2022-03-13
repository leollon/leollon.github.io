---
title: The Application Layer
date: 2019-08-14 11:02:28
categories: Sams-Teach-yourself-TCPIP-in-24-hours
tags: [notes, TCPIP, Networks]
---

## 本文目的

- 描述应用层
- 描述一些应用层的网络服务
- 列出一些TCPIP重要的程序

## What Is the Application Layer

应用层必须和TCP以及UDP端口一样感知传输层并且根据情况传输数据。TCPIP的应用层是一种用于向TCP和UDP端口发送信息和从TCP端口接收信息网络感知软件组件。在逻辑相似或等同的意义上，这些应用层组件不是并行的。

## The TPC/IP Application Layer and OSI

TCPIP的应用层与OSI的应用层，表现层以及会话层相关。

![bb3dcf7b5d06401e.svg](https://i.quantuminit.com/bb3dcf7b5d06401e.svg)

与TCPIP应用层相关的OSI分层描述：

- Application layer: OSI 应用层有提供服务给用户应用以及支持网络连接的组件。
- Presentation layer: 表现层将数据翻译成平台中立的格式并且处理加密以及数据压缩。
- Session layer: 会话层管理网络上计算机应用之间的通信。

对于多有的应用程序以及实现，所有的这些服务都不是必需。在TCPIP模型中，应用层的实现不要求遵循OSI细分的分层，但是总体来说，OSI的应用层，表现层以及会话层定义的职责都在TCPIP应用层负责的范围内。

## Network Services

在此层之前，协议系统的一层为系统中其他层提供服务。这些服务协议系统定义好的，完整的一部分。在应用层，不是所有的服务都被需要用于协议软件的运行并且更有可能根据用户的直接利益来提供，或者与本地操作系统进行网络连接。应用层含有支持用户体验的各种各样的网络服务：文件服务，远程访问服务，邮件服务和HTTP web 服务。

应用层更重要的活动：

- [文件与打印服务](#File-and-Print-Services)
- [域名解析服务](#Name-Resolution-Services)
- [远程访问服务](#Remote-Access)
- [Web服务](#Web-Services)。

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
|Network Time Protocol (NTP)|A protocol used for synchronizing clocks and other time sources over a TCPIP network|
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

[Name resolution](/What-is-TCP-IP/#Name-Resolution)是映射预订义，用户友好的字母数字名字到IP地址的过程。Domain Name System (DNS)服务给互联网（Internet）和隔离的TCPIP网络提供域名解析。DNS使用**name servers**来解析DNS域名查询。一个name server服务运行在name server计算机上的应用层并且与其他name server交换域名解析信息。还有其他比如NIS，NetBIOS以及一些与LDAP关联的域名服务的域名解析系统存在。

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
程序使用API功能与操作系统进行通信。网络协议栈就是API概念的一个经典的应用。网络API提供从应用到协议栈的接口。应用程序使用来自API的功能打开和关闭连接以及写入数据到网络或从网络中读取数据。Sockets API最初是为BSD UNIX开发的作为应用程序访问TCPIP协议栈的接口。从Window 95开始，微软将TCPIP程序接口直接放进了Window操作系统，而在此之前，时序哟啊用户安装且配置的。

应用程序通过API访问TCPIP协议栈：

![d91fbe6b3a864831.svg](https://i.quantuminit.com/d91fbe6b3a864831.svg)

例如Sockets API的网络API通过[socket](/The-Transport-Layer/#Ports-和-Sockets)接收并传递数据给应用程序。因此那些APIs在应用层运行。

TCPIP实用程序表

|程序名|描述|
|:----|:----|
|ipconfig/ifconfig|A Unix/Linux utility that displays and sets TCPIP configuration settings.(The Windows utility IPConfig is similar.)|
|ping|A utility that tests for network connectivity.|
|arp|A utility that lets you view (and possibly modify) the Address Resolution
Protocol (ARP) cache of a local or remote computer. The ARP cache
contains the physical address to IP address mappings (see Hour 4,
“The Internet Layer”).|
|traceroute|A utility that traces the path of a datagram through the internetwork.|
|route|A utility that lets you view, add, or edit entries in a routing table.[Routing](/Routing)|
|netstat|A utility that displays IP, UDP, TCP, and ICMP statistics.|
|hostname|A utility that returns the hostname of the local host.|
|ftp|A basic file transfer utility that uses TCP.|
|tftp|A basic file transfer utility that uses UDP. Tftp is used for tasks such as downloading code to network devices.|
|finger|A utility that displays user information.|

## 小小的总结

- 应用层必须和TCP以及UDP端口一样感知传输层并且根据情况传输数据。
- DNS是用于映射域名映射到IP地址的服务，HTTP是用于提供web服务的协议。RPC是用于一台计算机上的程序调用另一台计算机
上的子程序或程序。
- ifconfig, ping, traceroute, netstat/ss, telnet
- 获取邮件的的两个应用层协议：IMAP, POP3
- ntp协议用于同步计算机时钟

[上一篇](/The-Transport-Layer)

[下一篇](/Routing)
