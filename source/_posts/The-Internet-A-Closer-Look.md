---
title: 'The Internet: A Closer Look'
date: 2019-10-08 15:58:39
categories: Sams-Teach-yourself-TCP/IP-in-24-hours
tags: [notes, TCP/IP, Networks]
---

本文的最终目的：

- 简单描述互联网的的结构
- 认识并且描述统一资源标识符的组成

## How the Internet Looks

如今的互联网是一个大部分私有网络分享以及售卖其他网络访问的宽大集合。在系统的中心是一个称之为**Tier 1 networks**的圆。Verizon，Sprint，AT&T，以及Qwest都运营这Tier 1 网络。每个Tier 1 网络有一个为来自其他Tier 1网络的流量提供自由传输的对等结点安排。由巨大的Tier 1 网络架设的大量地址空间给互联网提供了大象的全球连接，但是Tier 1只是其中的一部分。

Tier 2 网络系统围绕Tier 1 网络边缘运行。Tier 2 网络可能租约访问一个Tier 1 提供商，但也可能和其他Tier 2 提供商是对等关系来构成一个区域骨干并且为下游的客户端提供冗余的路径。按照定义，一个Tier 2 网络运营传输对等和租约IP传输的组合。Tier 2 network通过向**Tier 3 networks**提供租约互联网接入来盈利。

一个Tier 3 网络就是大多数人能知道的互联网服务提供上（ISP）。Tier 3 ISPs出租所谓的**point of presence(POP)**连接来给它们线路上的订阅用户访问更广大的互联网。

![https://i.quantuminit.com/911437ac01064775.svg](https://i.quantuminit.com/911437ac01064775.svg)

贯穿互联网的Tier 1 以及 Tier 2网络（还有一些ISPs）在称之为互联网交换点（Internet exchange points，IXPs）的交换基础设中相交汇。IXPs是大型的基础设施。许多，或者有时候成百上千的多个网络能够在单独一个交换点进行连接。IXP不提供路由服务。而是由成员网络在IXP基础设施可用的一个安全的空间中提供并且维护它们自己的路由器。IXP基础设置本身是一个本地网络，并且流量经过本地网络，这个本地网络在IXP基础设施内，而这个基础设施在通过运行在网络接入层（OSI的数据链路层）的交换机来管理的成员网络间充当接口。

事实上，互联网是一个单独的内具的实体不是因为它的物理连接而是因为它：

- 有共同的规则集合
- 通过共同的组织集合来管理以及维护
- 讲共同的语言

## What Happens on the Internet

特别要记住的是参与在互联网上或任何其他网络上的网络活动的所有计算机都有一个共同的特点：它们运行着参与这些网络活动的软件。不仅仅是发生网络通信。它需要协议软件，也需要在用于和彼此之间进行通信的连接两端的应用程序。如下图所示，互联网上大部分计算机能够被分成要么是客户端（请求服务的计算机）要么是服务器（提供服务的计算机）。

![https://i.quantuminit.com/b4205a93a8e54fa8.svg](https://i.quantuminit.com/b4205a93a8e54fa8.svg)

编写出来的客户端计算机上的一个客户端应用程序被用于和服务端计算机上服务端应用程序进行交互。编写出来的服务端应用程序用于监听来自客户端的请求以及相应这个请求。

一个对等网络上的结点像

## URIs and URLs

IP 地址或域名能够定位到一个主机。端口号能够指向运行在主机上的一个服务。但是客户端请求什么？服务器应该做什么？客户端请求输出是否存在输入？

对于互联网用户来说最熟悉的请求格式是统一资源定位符（URL）。一个URL实际上是一个又称为统一资源标识符（URI）的更通用的格式的一个特殊例子。对于同茶馆你的例子来说，术语*标识符*相比*定位符*要更好，因为每个请求实际上不会指向一个位置。

在world wide web上看到的URLs的基本格式如下：

scheme://authority/path?query#fragment

**scheme**用于标识一个系统如何解释请求。scheme字段经常与一个协议关联起来。

URI Schemes

|Scheme|Description|Reference|
|:----:|:----:|:----:|
|file|A file on the host system|RFC 1738|
|ftp|File Transfer Protocol|RFC 1738|
|gopher|The Gopher protocol|RFC 4266|
|http|Hypertext Transfer Protocol|RFC 7230|
|https|Hypertext Transfer Protocol Secure|RFC 2818|
|im|Instant Messaging|RFC 3860|
|ldap|Lightweight Directory Access Protocol|RFC 4516|
|mailto|Electronic mail address|RFC 6068|
|nfs|Network File System protocol|RFC 2224|
|pop|Post Office Protocol v3|RFC 2384|
|telnet|Telnet Interactive session|RFC 4248|

以双斜杠开始的**authority**为访问URL中的指定的信息提供必要的信息。这部分内容包含：

- Username (optional): 用于访问服务的用户帐号
- Password (optional): 与用户名字一同指定
- Host: 被请求的提供服务或资源的计算机。可以使用一个IP 地址或DNS名字指定主机。IPv6地址使用[]括号括起来。
- Port: 接收地址的服务端口号

如果服务使用的端口是固定分配给该服务的，则端口号可以省略。例如HTTP服务默认使用80端口。为了读取资源，如果需要用户提供凭证，用户名和密码是必需的。
在通常的构成形式中，authority部分的URL看起来像下面这样子：

//username:password@host:port_number

一些例子：

//www.google.com

//blog.quantuminit.com

//ftpuser:userp4s5word@google.com

//quantuminit.com:8080

即使访问需要提供凭证的资源，可能不用一开始就在URL中指定用户。在初始化请求之后，许多服务会提示用户输入用户ID以及密码。

