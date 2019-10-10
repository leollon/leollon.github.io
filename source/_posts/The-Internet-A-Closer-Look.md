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
