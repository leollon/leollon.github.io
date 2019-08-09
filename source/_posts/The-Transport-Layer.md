---
title: The-Transport-Layer
date: 2019-08-09 11:07:30
categories: Sams-Teach-yourself-TCP/IP-in-24-hours
tags: [notes, TCP/IP, Networks]
---

Transport layer用途：

- 给网络应用提供API
- 用于网络传输的可选的错误检查，流控制以及验证

本文目的在于：

- 描述传输层的基本职责
- 解释面向连接和无连接协议的异同
- 解释传输层如何通过ports以及sockets给网络应用提供接口
- 描述TCP以及UDP的异同
- 识别构成TCP header的字段
- 描述TCP如何打开开启并关闭连接
- 描述TCP如何序列并且通知数据传输
- 识别构成UDP header的四个字段

## Introducing the Transport Layer

TCP/IP开发者知道他们需要在能够与IP协作的网络层（Internet layer）上，通过提供额外的必要特性的另一层。
需要Transport layer protocol提供：

- 给网络应用提供接口：用于网络应用访问网络。需要能够将数据不仅仅是发送给目标计算机，还要将数据发送到目标计算机特定
的应用中。
- 多路复用/反多路复用：多路复用，意思是从不同的应用和计算机接收数据，并且指导数据到达接收计算机上的目标接收应用。
换句话说就是传输层能够不断地支持多个网络应用并且管理到网络层（Internet layer）的数据流。
接收端，传输层必须能够从网络层接受数据并且指导数据到多个应用中去。这一特性，称之为反多路服用，允许一台计算机不断地支持多个网络应用，比如浏览器，邮件客户端和文件分享工具。多路复用/反多路复用的另一方面是一个单独的应用能够连续地维护与多个计算机之间的连接。
- 错误检查，流控制以及验证：需要确保数据在发送和接受机器上传递的完整方案。

**Transmission COntrol Protocol (TCP)**: TCP 提供广泛的错误控制和流控制以及验证数据的成功传递。**TCP是面向连接的协议**。

**User Datagram Protocol (UDP)**: UDP提供及其基本的错误检查并且被设计用于没必要使用TCP广泛的控制特性的时
候。**UDP是一个无连接的协议**。

## Transport Layer Concepts

理解传输层的设计本质的几个重要概念：

- 面向连接和无连接协议
- Ports 和 Sockets
- 多路复用/反多路复用

## 面向连接和无连接协议

- 面向连接协议建立并维护通信计算机之间的连接并且通过传输线路监控连接的状态。换句话说，每个通过网络发送出去的数据包收到一个通知，并且发送机器记录状态信息来确保每个包没有错误地被收到，如果有必要，则重传。传输结束，发送端和接收端优雅地关闭连接。
- 无链接协议发送单向的数据报到目的计算机并且不操心通知目的计算机数据报在路上了。目的计算机接收到数据并且不操心返回
状态信息给源计算机。

面先连接协议的示意图：

![a02769e3bc074551.svg](https://i.quantuminit.com/a02769e3bc074551.svg)

无连接协议的示意图：

![c7237981b1d042f0.svg](https://i.quantuminit.com/c7237981b1d042f0.svg)

## Ports 和 Sockets

传输层在网络应用和网络之间提供一个接口并且提供传递数据到特定应用的方法。**Port**是预定义的内部地址，它作为从应用到
传输层或从传输层到应用的一条路径。例如，一台客户端通过TCP端口21与一个服务器的上FTP应用程序通信：

**Socket**是一个通过连接IP地址和端口(port)号码形成的一个地址。例如socket number 111.121.131.141:21关联到IP地址是111.121.131.141的计算机上的21端口。

计算机之间构成连接时，使用TCP交换socket 信息图示：

![3c677c9d9e2c418b.svg](https://i.quantuminit.com/3c677c9d9e2c418b.svg)

一台计算机通过一个socket访问一台目的计算机上应用的过程：

1. 计算机A通过已知的端口初始化一个连接计算机B上的应用程序的连接。一个**已知的端口**是一个有IANA分配给特定应用程
序的端口号码，IANA目前由ICANN管理。结合IP地址，这个已知端口成为计算机A的目的socket地址。这个请求包含告诉计算机B
当发送信息回给计算机B的时，要使用那个socket number的数据字段。这个就是计算机A的源socket address。

2. 计算机B通过已知端口收到来自计算机A请求并且响应和计算机A源地址列出来的一样的socket。这个socket成为消息从计算机B上应用程序发送到计算机A上应用程序上的目的地址。

已知的TCP端口：

![0a2e2f97a2b54fd3.png](https://i.quantuminit.com/0a2e2f97a2b54fd3.png)
![806adc354e5443a0.png](https://i.quantuminit.com/806adc354e5443a0.png)

已知的UDP端口：

![d10a759ae4254590.png](https://i.quantuminit.com/d10a759ae4254590.png)


[上一篇](/Subnetting-and-CIDR)
[下一篇](/The-Application-Layer)
