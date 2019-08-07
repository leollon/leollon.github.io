---
title: Subnetting-and-CIDR
date: 2019-08-07 14:25:39
categories: Sams-Teach-yourself-TCP/IP-in-24-hours
tags: [notes, TCP/IP, Networks]
---

## Subnets

子网划分将网络分成更小的单位，称之为子网。CIDR不再需要强调网络地址类。

## Dividing the Network

通过识别IP地址中的network ID，主机可以将数据发送到正确的网络。但是通过A类，B类以及C类地址的网络ID来识别网络段是有局限性的。在网络层级下，不提供任何的逻辑分区。

一个A类网络：
[A类网络地址](/The-Internet-Layer/#Internet-Protocol)是8 bit的 network ID，24 bit 的 host ID。
那么一个确定network ID 的A类网络下的主机数量达到2<sup>24</sup> - 2 个，减2 是减去了host ID全0以及全1的情况，全0是网络本身，全1是广播地址。由此可见，主机数量及其庞大。
图示：

![505959fc7fa2453b.svg](https://i.quantuminit.com/505959fc7fa2453b.svg)

将物理网络分段扩大了网络的整体容量，并且因此使得网络能够使用更大一部分的地址空间。在这个常见的场景中，在地址空间范围内，分割网络段的路由器需要一些往哪个网络传输数据的暗示。因为发送的每个数据报都有相同的network ID(99.0.0.0)，因此，不能使用network ID。

划分更小的网络段：

![9d25ba35007a4548.svg](https://i.quantuminit.com/9d25ba35007a4548.svg)

子网掩码的使用，子网掩码用来告诉有多少地址能够用于子网 ID以及还剩下多少实际的host ID。CIDR已经大部分替代了子网掩码。但它们是说同一种事物的两种方式。

## The Old Way: Subnet Mask

一个子网掩码是一个32 bit 二进制的数字。子网掩码的位被放置在可以表明IP地址的子网ID与那个掩码相关联的模式中。

在Linux上，可以通过`ifconfig`命令查看这台主机上配置的子网掩码(mask)。

子网掩码中每一个bit位代表IP地址中的一bit位。子网掩码给IP地址是network ID或子网ID的部分的每一位填上1。
IP地址/子网掩码对图示。
![0ec96e5925d64d33.svg](https://i.quantuminit.com/0ec96e5925d64d33.svg)

未划分子网的网络以及划分子网的网络图示：

![a3ce55e07b254e29.svg](https://i.quantuminit.com/a3ce55e07b254e29.svg)

路由使用的路由表以及子网上的主机含有子网掩码关联的每一个IP地址的信息。
根据数据报的network ID转发数据报到相应的网络，然后根据该网络的subnet ID将数据路由到适当的子网，接着根据host ID，将数据发送到正确的计算上。

数据报在划分子网中的转发过程：

![10b12bd0e16c44d1.svg](https://i.quantuminit.com/10b12bd0e16c44d1.svg)

配置子网掩码，手动方式是在配置TCP/IP的同时也配置它，而如果IP地址是通过DHCP获取的，DHCP服务器分配IP地址的同时也一起分配一个子网掩码，而如果从ISP那里得到一个静态IP地址，与此同时也会有一个和这个IP地址一起使用的子网掩码。
所有子网内的主机应该有同样的子网ID以及子网掩码。



[上一篇](/The-Internet-Layer)

[下一篇](/The-Transport-Layer)
