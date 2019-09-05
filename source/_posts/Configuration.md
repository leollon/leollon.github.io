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
