---
title: Classic Tools
date: 2019-09-16 17:42:59
categories: Sams-Teach-yourself-TCP/IP-in-24-hours
tags: [notes, TCP/IP, Networks]
---


阅读完本文之后，将可以：

- 标识并且描述常见的TCP/IP连接工具
- 使用连接工具排查问题
- 解释SSH和Telnet的目的
- 描述一些常见的网络管理协议

## 连接问题

具有代表性的前四个网络连接问题变化：

- 协议功能障碍或配置错误：协议软件不工作或（无论什么原因）未正确配置地运行在网络上。
- 线路问题：网线未插入或不正常工作。集线器，路由器或交换机未正常工作。
- 错误的名字解析：域名系统（DNS）或NetBIOS名字不能被解析。资源能够通过IP地址进行访问但是不能通过主机名或DNS名字（quantuminit.com）。
- 流量过大（Excessive traffic）：网络是工作状态，但是工作很慢。

下面分别讨论解决这些常见连接问题的工具及技术。

## Protocol Dysfunction and Misconfiguration
  
TCP/IP协议组提供许多有用的工具帮助确定TCP/IP是否功能正常并且正确配置，比如

- **ping**：该工具是初始化一个简单的网络连接测试的诊断工具并且能够报告其他计算机或网络设备是否响应。
- **Configuration information utilities**：每一个OS厂商提供某有种显示TCP/IP配置信息的工具并且可以用于检查是否IP地址，子网掩码，DNS服务器和其他参数是否正确配置。
- **arp**：这个工具可以用于查看并且配置地址解析协议（ARP）缓存表的内容，缓存表中将IP地址和物理（MAC）地址关联起来。

### Ping

`ping`工具能够初始化最小的网络连接测试。它发送一则消息给另外一台计算机，说"Are you there"并且等待其他计算机响应。

`ping`命令的基本形式：

```bash
ping IP_address  # IP_address是想要连接的那台计算机的地址。
```

ping工具的命令行选项根据实现和系统不同而不同。*ping*工具使用互联网控制消息协议（[ICMP](The-Internet-Layer/#Internet-Control-Message-Protocol)）`Echo Request`命令发送一则消息到接收计算机。如果接收计算机存在并且运行，它会使用`Echo Reply`消息进行响应。

ping是一个最小的应用。它只需要TCP/IP协议栈中的网络接入层，该层与OSI模型中的底部三层相关。ICMP是一个OSI的三层协议。可能问题发生在传输控制协议（TCP），用户数据报（UDP），或是更上层的应用层并且ping依然能够正常运行。

Ping提供许多特别有用的选项用于网络问题排查。可以这么做有如下步骤：

- 使用称为loopback地址（127.0.0.1）的特殊IP地址ping本地IP软件。如果命令`ping 127.0.0.1`成功，则TCP/IP协议软件能够正常运行。
- ping自己的IP地址。如果能够ping通分配给网络适配器（网卡）的IP地址，就可以知道网卡已经正确配置并且正常地与TCP/IP软件进行交互。
- 通过主机名来ping。大部分系统允许使用一个主机名替换ping命令中的IP地址。如果能够通过IP地址ping通一台计算机，但是不能通过它的主机名ping同样的计算机，就可以知道问题是和名字解析有关系。
