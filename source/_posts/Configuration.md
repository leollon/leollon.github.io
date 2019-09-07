---
title: Configuration
date: 2019-09-05 17:38:04
categories: Sams-Teach-yourself-TCP/IP-in-24-hours
tags: [notes, TCP/IP, Networks]
---

本文目的在于：

- 描述DHCP及其提供的好处
- 描述通过通过DHCP租用一个IP地址的过程
- 描述网络地址翻译（NAT）的目的
- 展示计算机如何使用零配置的协议

## Getting on the Network

尽管不同的系统使用不同的方式配置，但是最基本的选择是根据如下之一来做：

- 配置一个静态IP地址
- 将计算机配置成从DHCP接收一个动态地址

即使如果你的计算机没有成功的配置一个静态或动态的IP地址，一些系统依然能够通过零配置技术来执行一类最基本的TCP/IP网络。

## The Case for ServerSupplied IP Address

在TCP/IP网络运行每台计算机必须要有一个IP地址。静态地址的局限：

- More configuration：每个客户端必须进行独立配置。地址空间或某些参数上的改变（比如DNS服务器地址）都意味着每个客户端必须重新独立配置。
- More addresses：每台计算机使用一个IP地址不管当前是否在网络上。
- Reduced flexibility：如果被分配到不同的子网，一台计算机必须手动重新配置。

为了解决这一局限，另一个IP地址系统使用DHCP来分配IP地址。DHCP从早期用于启动无盘计算机的**BOOTP**协议开发而来。无盘计算机随着它启动的时候通过网络接收一个完整的操作系统。

## What Is DHCP

DHCP是一个用于自动为计算机分配TCP/IP配置参数的协议。一个DHCP服务器可以提供给一个DHCP客户端许多TCP/IP设置，比如一个IP地址，一个子网掩码，以及一个DNS服务器的地址。

因为DHCP服务器负责分配IP地址，所以理论上只有DHCP服务器必须配置静态地址信息。对于用户工作站，笔记本以及其他网络客户端系统，对于客户端来说，唯一需要配置网络参数选项的是发送IP地址信息的DHCP服务器。剩下的TCP/IP配置通过网络传输。这样子以来，如果网络上TCP/IP配置某些方面发生改变，网络管理员仅仅只需要更新DHCP服务器，而不是手动更新每个客户端。

更重要的时候，每个客户端接收一个有租约时间限制的IP地址。DHCP租约特性的优点是网络没必要和含有的客户端一样需要很多的IP地址。

## How DHCP Works

当一个DHCP客户端计算机启动时，TCP/IP软件载入内存中并开始运行。然而，由于TCP/IP栈还没有IP地址，因此它不能够发送或接收定向的数据报。但是，计算机可以发送并且监听广播。通过广播通信的能力是DHCP能够工作的基本。从DHCP服务器租用一个IP地址过程的四个步骤：

1. DHCPDISCOVER：DHCP客户端通过广播一个发往端口67（BOOTP以及DHCP服务器使用）的数据报来初始化这个过程。第一个数据报又称为DHCP发现消息，它是一个请求任何DHCP服务器接收用于配置信息的数据报。DHCP发现数据报中含有许多字段，但是最重要的一个是含有DHCP客户端物理地址的字段。
2. DHCPOFFER：DHCP服务器为驻留在网络上的客户端计算机构造一个称为DHCP offer的响应数据报并且通过广播将数据报发到发起DHCP discover的计算机上。这个广播被发往UDP端口68并且含有DHCP客户端物理地址。同时包含在DHCP offer中的还有DHCP服务器的物理地址以及IP地址，分配给DHCP客户端的IP地址以及子网掩码。如果有不只一个能够给DHCP客户端提供IP地址的DHCP服务器，此时DHCP客户端有可能接收到几个DHCP offers。大多数情况下，DHCP客户端接收第一个到达的DHCP offer。
3. DHCPREQUEST：客户端选择一个offer并且构造以及广播DHCP请求数据报。DHCP请求数据报包含有能发出offer的服务器的IP地址以及DHCP客户端的地址。DHCP请求执行两个基本任务。第一告诉DHCP服务器客户端请求给DHCP客户端分配一个IP地址（以及其他配置设置）。第二，通知所有其他有更好的offer的DHCP服务器它们的offers不会被接受。
4. DHCPACK：当被选择的来自DHCP服务器的offer的服务器接受DHCP请求报文是，服务器构造最终的租用过程的数据报。这个数据报又称为**DHCP ack**（*acknowledgment的缩写)。DHCPack包含分配给DHCP客户端的一个IP地址以及子网掩码。可选地，DHCP客户端经常也被配置默认网关，几个DNS服务器和可能一或两个WINS服务器的IP地址。除了IP地址之外，DHCP客户端能够接收其他比如NetBIOS结点类型的信息，该信息可能改变BetBIOS名字解析的顺序。

包含在DHCPack中的三个其他关键的字段，每一个都暗示时间段，一个说明租约长度的字段，两个其他时间字段，又称为T1和T2，当客户端尝试重新更新租约时，将会使用到。

### Relay Agents

如果DHCP客户端和DHCP服务器在同一网络段上，这个过程处理和之前描述的一样。如果DHCP客户端和DHCP服务器在由一个或更多的路由分割开来的不同的网络，这个过程会变得更复杂。路由器一般不转发广播到其他网络。为了使得DHCP工作，必须有一个辅助DHCP过程的中间人。这个中间人可以是同一网络上另外一个作为DHCP客户端的主机，但是经常是路由器本身。在任何情况，执行这个中间人功能的过程被称为**BOOTP relay agent**或是**DHCP relay agent**。

一个relay agent被配置一个固定的IP地址并且知道DHCP服务器的IP地址。因为relay agent 已经配置了IP地址，它们总是能够发送并且接收给DHCP服务器的定向的数据报。因为relay agent与DHCP客户但处在同样的网络上，所以能够通过广播和DHCP客户端进行通信。

![https://i.quantuminit.com/08620e0f601c4ed0.svg](https://i.quantuminit.com/08620e0f601c4ed0.svg)

### DHCP Time Fields

DHCP客户端从DHCP服务器租用一个固定时间的IP地址。实际的租约时间长度一般在DHCP服务器上面定义。与DHCP ack消息发送的T1和T2时间的值在租约重新更新过程中用到。T1值暗示客户端什么时候开始重新更新租约过程。T1一般设置为实际租约时间的二分之一。更新租约的时候，DHCP request 和 ack两个数据报不是广播，而是作为定向的数据报发送。因为两台计算机此时都含有有效的IP地址，所以这是有可能的。

当DHCP客户端在实际租约时间二分之一时发起第一个更新租约请求，如果DHCP服务器不可用，客户端等待并在租约时间的4/3时进行尝试更新租约。如果请求也还是失败，DHCP客户端在7/8的租约时间时第三次进行尝试更新。如果DHCP客户端不能在总的租约时间的87.5%更新它的租约，T2时间生效。T2时间允许DHCP客户端向任何DHCP服务器发起广播请求。如果DHCP客户端不能更新租约或在租约过期时从另外一个DHCP服务器获取到新的租约，客户端必须停止使用该IP地址和停止使用TCP/IP用于普通的网络操作。

### DHCP Server Configuration

除非你是在中型或大型网络上的一个系统管理员，否则你不可能有机会配置一台计算来充当一个DHCP服务器。Windows提同一个图形界面的工具来配置DHCP服务器。Linux系统通过dhcpd（DHCP守护程序）提供DHCP服务。根据不同的厂商，安装dhcpd的命令也不一样。DHCP配置信息存储在/etc/dhcpd.conf配置文件中。
