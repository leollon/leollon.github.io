---
title: Subnetting-and-CIDR
date: 2019-08-07 14:25:39
categories: Sams-Teach-yourself-TCP/IP-in-24-hours
tags: [notes, TCP/IP, Networks]
---

## Subnets

子网划分将网络分成更小的单位，称之为子网。CIDR不再需要强调网络地址类。

## Dividing the Network

通过识别IP地址中的network ID，主机可以将数据发送到正确的网络。但是通过A类，B类以及C类地址的网络ID来识别网络段是
有局限性的。在网络层级下，不提供任何的逻辑分区。

一个A类网络：

[A类网络地址](/The-Internet-Layer/#Internet-Protocol)是8 bit的 network ID，24 bit 的 host ID。
那么一个确定network ID 的A类网络下的主机数量达到2<sup>24</sup> - 2 个，减2 是减去了host ID全0以及全1的情
况，全0是网络本身，全1是广播地址。由此可见，主机数量及其庞大。

图示：

![505959fc7fa2453b.svg](https://i.quantuminit.com/505959fc7fa2453b.svg)

将物理网络分段扩大了网络的整体容量，并且因此使得网络能够使用更大一部分的地址空间。在这个常见的场景中，在地址空间范
围内，分割网络段的路由器需要一些往哪个网络传输数据的暗示。因为发送的每个数据报都有相同的network ID(99.0.0.0)，
因此，不能使用network ID。

划分更小的网络段：

![9d25ba35007a4548.svg](https://i.quantuminit.com/9d25ba35007a4548.svg)

子网掩码的使用，子网掩码用来告诉有多少地址能够用于子网 ID以及还剩下多少实际的host ID。CIDR已经大部分替代了子网掩
码。但它们是说同一种事物的两种方式。

## The Old Way: Subnet Mask

一个子网掩码是一个32 bit 二进制的数字。子网掩码的位被放置在可以表明IP地址的子网ID与那个掩码相关联的模式中。

在Linux上，可以通过`ifconfig`命令查看这台主机上配置的子网掩码(mask)。

子网掩码中每一个bit位代表IP地址中的一bit位。子网掩码给IP地址是network ID或子网ID的部分的每一位填上1。

IP地址/子网掩码对图示。

![0ec96e5925d64d33.svg](https://i.quantuminit.com/0ec96e5925d64d33.svg)

未划分子网的网络以及划分子网的网络图示：

![a3ce55e07b254e29.svg](https://i.quantuminit.com/a3ce55e07b254e29.svg)

路由使用的路由表以及子网上的主机含有子网掩码关联的每一个IP地址的信息。
根据数据报的network ID转发数据报到相应的网络，然后根据该网络的subnet ID将数据路由到适当的子网，接着根据host
ID，将数据发送到正确的计算机上。

数据报在划分子网中的转发过程：

![10b12bd0e16c44d1.svg](https://i.quantuminit.com/10b12bd0e16c44d1.svg)

配置子网掩码，手动方式是在配置TCP/IP的同时也配置它，而如果IP地址是通过DHCP获取的，DHCP服务器分配IP地址的同时也
一起分配一个子网掩码，而如果从ISP那里得到一个静态IP地址，与此同时也会有一个和这个IP地址一起使用的子网掩码。所有子
网内的主机应该有同样的子网ID以及子网掩码。

**子网掩码中表示IP地址的network ID以及subnet ID的都是1，表示host ID的都是0。8位字节中，有部分为1，另一部分
为0，部分为1的个数n表明可划分子网的个数是2<sup>n</sup> - 2，另一部分为0的表明该子网内的host个数是2<sup>8-n</sup> - 2。**

一个使用B类地址的第三个8位字节作为子网号的例子，将129.100.0.0划分出四个子网，因此需要第三个8位字节的前四位用来做
为子网ID：

- 子网 #1: 10000001 01100100 10000000 00000000, 即129.100.128.0
  
  - Network ID: 129.100.0.0
  - Subnet ID: 0.0.128.0.0

- 子网 #2: 10000001 01100100 11000000 00000001, 即129.100.192.0  

  - Network ID: 129.100.0.0
  - Subnet ID: 0.0.192.0.0

- 子网 #3: 10000001 01100100 11100000 00000001, 即129.100.224.0
  
  - Network ID: 129.100.0.0
  - Subnet ID: 0.0.224.0.0

- 子网 #4: 10000001 01100100 11110000 00000001, 即129.100.240.0
  
  - Network ID: 129.100.0.0
  - Subnet ID: 0.0.240.0.0

![6fd5abeab32c43c9.svg](https://i.quantuminit.com/6fd5abeab32c43c9.svg)

全0或全1的Network/subnet 以及host IDS都不能分配，全0时，是网络本身，全1时是广播地址。因此如果使用B类地址的第
三个8位字节作为子网ID时，可划分出2<sup>8</sup> - 2 个子网，每个子网内可以含有2<sup>8</sup> - 2 个地址。

因为子网掩码中的表示network ID以及subnet ID都是1，还有A，B，C三类地址中网络地址分别要求0，10，110开头，并且它
们的网络地址范围的最大值是分别是126（127开头的是保留地址），191，223，所以它们的有效子网掩码分别从255.0.0.0，
255.255.0.0 和255.255.255.0开始。

## The New way: CIDR

Classless Inter-Domain Routing (CIDR) 是一种可以在路由表中定义地址块更灵活的技术。CIRDR系统不依赖预先定义
的8,16,24的network ID。而是使用一个称为CIDR前缀的数字指定在作为network/subnet ID的地址内的bit位数。CIDR注
记使用后面跟着一个十进制的数字的`/`来指定在IP地址中的network地址部分占用多少bit位。
例如，205.123.196.183/25，则指定了其中的25 bit位用于网络地址，相应的32 bit子网掩码前25位都是1，后面都是0。

CIDR优点：
支持划分网络的同时，还允许ISP或者管理员聚集或者整合连续的C类地址成一个整体。通过简化路由表延长了IPv4互联网的生命
周期。在这个情况中，CIDR前缀充当了**supernet mask**。

## 小小的总结

- 子网划分中的subnet ID的位是从host ID借过来的。
- 子网掩码中，全为1的位表示网络ID以及子网ID位，全为0的那部分是host ID位，其中如果有部分为1，另一部分为0的，部分
为1的个数n表示可划分的子网个数为2<sup>n</sup> - 2，该子网可用有的host个数是2<sup>8 - n</sup> - 2。
- CIDR使用/n，n为一个整数，表示网络ID以及子网ID中bit位占n位，剩余的bit位为host ID占用的个数。

[上一篇](/The-Internet-Layer)

[下一篇](/The-Transport-Layer)
