---
title: Sams-Teach-yourself-TCP-IP-in-24-Hours
date: 2019-07-29 17:30:48
categories: TCP/IP
tags: [notes, TCP/IP, Networks]
---

## 什么是TCP/IP？

Transmission Control Protocol/Internet Protocol(TCP/IP) 是一个协议系统——支持网络通信的协议集合。

那么问题就来了，回答什么是协议之前，先谈谈什么是网络？


网络及协议
网络是通过通用的传输媒介能够通信的类计算机或计算机的集合。传输媒介可以是计算机之间携带电子脉冲的绝缘导线，
也可以是是一根电话线，或甚至根本就没有线，比如无线网络。

![a432e83c02a14484.png](https://i.quantuminit.com/a432e83c02a14484.png)

一个计算机通过一个或多个执行特殊任务并且管理通信过程的应用来与世界进行交互。

网络协议是一系列帮助定义网络通信复杂过程的通用规则。协议指导发送来自计算机上的应用的数据到另一台计算机上的应用的过程。

![9ba90a2f5b8541d0.png](https://i.quantuminit.com/9ba90a2f5b8541d0.png)

TCP/IP协议定义了网络通信过程并且，更重要的是定义了一个数据单元的查看方式并且应该包含什么数据，这样数据的接收的电脑能够正确地读取这些信息。
TCP/IP以及其相关的协议构成了一个定义数据在TCP/IP网络上如何被处理，传输以及接收的方式的计算机系统。一个相关连的协议系统，被称为协议套装。

TCP/IP标准是定义在TCP/IP网络上通信的规则系统。
TCP/IP实现是执行将一台计算机加入到TCP/IP网络中的功能的软件。

TCP/IP标准的目的是确保所有的TCP/IP实现能够互相兼容，而不用在意版本或是硬件厂商。

不是由TCP/IP模型提供服务，而是定义应该提供的服务。事实上是由实现TCP/IP的厂商软件提供服务。

TCP/IP的两个重要特性：
- 端节点验证：两台互相通信的计算机都处在消息传输链上的两端，因此称之为端节点，有责任告知收到并且验证传输。
- 动态路由：结点通过多路径连接，并且路由器基于现在的情况选择一个路径传输数据。

随后个人电脑的普及带来了`LAN`(local area network)，接着为了让处于LAN的计算机能够连接上基于TCP/IP的网络，此时负责协议翻译的特殊网关(gateways)应运而生。

## TCP/IP 特性

专注于TCP/IP解决如下问题的方式：
- Logical addressing
- Routing
- Name resolution
- Error control and flow control
- Application support

这些问题是TCP/IP的核心部分。

### Logical addressing
网络适配器都有一个唯一的物理地址。以太网中，物理地址（有时候成为Media Access Control[MAC]）在工厂中被分配好。在一个LAN上，底层硬件协议使用适配器的物理地址通过物理网络传送数据。有多种网络类型，并且每一种都有不同的传送数据的方式。
举个例子，在基本的以太网上，一台计算机直接通过传输媒介发送消息。

在大型网络中，需要划分子网并且使用层级设计，目的是消息能够有效地传输到达目的。通过logical addressing，`TCP/IP`提供了子网划分的能力。**logical address**是一个可以通过网络软件进行配置的的地址。在`TCP/IP`中，一台计算机的的logical address 被称为**IP address**。一个IP address能够包含：
- 能用于识别网络的网络ID号码
- 能识别网络上子网的子网ID号码
- 能识别子网上这台计算机的主机ID号码

如果要将自己的网络作为互联网的一部分，ICANN将分配一个网络ID到你的网络中，这个网络ID构成IP address的第一部分。
一项有意思的开发是一个称为Network Address Translation(NAT)的系统，这一系统使得本地使用私有的不能够在互联网上路由的IP地址范围。
NAT设备翻译本地地址成正式的网络即用型地址来进行互联网通信。
在TCP/IP中，使用Address Resolution Protocol(ARP) 和 Reverse ARP(RARP)，可以解析出logical address与硬件的物理地址。

A: logical address

B: physical address
```
|-----|          |-----|
|     |   <---   |     |
|  A  |          |  B  |
|     |   --->   |     |
|-----|          |-----|
```
