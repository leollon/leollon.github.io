---
title: Getting Connected
date: 2019-08-19 15:26:15
categories: Sams-Teach-yourself-TCP/IP-in-24-hours
tags: [notes, TCP/IP, Networks]
---

本文目的：

- 理解光缆宽带的基本知识
- 讨论DSL定义的特性
- 描述无线网络的拓扑以及比如WEP和WPA2的无线网络安全方案的要素和功能
- 描述计算机如何通过电话线拨号上网进行通信

这里引入一些其他可以在TCP/IP网络中常见的连接设备，比如交换机，集线器以及网桥。

## Cable Broadband

一个典型的线缆调制解调器链接图示：

![b6aee1fb6923494c.svg](https://i.quantuminit.com/b6aee1fb6923494c.svg)

*modem*是modulator/demodulator的缩写。一个像电话调制解调器的线缆调制解调器，调制数字网络传输成为模拟形式并且从模拟形式到有效地通过线缆连接传递数据。
**线缆猫终端系统（CMTS）**接收来自线缆猫的信号并且将其转回在线缆提供商的网络接口处的数字形式。反过来，提供商从上游互联网服务提供商（ISP）租用带宽，并且提供商网络上的路由器连接用户和余下的网络。提供商可能也提供其他的支持服务，比如动态主机配置协议（DHCP）被用来给网络上的用户分配一个动态的IP地址。线缆猫实际上不是一个路由器，而更像是一个网桥。线缆猫通过网络接入层的媒体接入控制（MAC）地址来过滤流量。**Data Over Cable Service Interface Spedification(DOCSIS)是用于线缆猫的网络的标准。

## Digital Subscriber Line

其他用于家庭宽带传输媒介的候选是电话网络。Digital Subscriber Line的缩写就是DSL。事实上，在电话网络中使用的双绞线比起用于语音通话时具有更多的容量。DSL收发器充当从本地网络到电话网络的接口，它运行在不与线上语音通话干扰的频率中。DSL网络需要一个在电话线另一端接收信号以及通过提供商的网络和互联网交互的设备。称为数据用户线访问复用器（DSLAM）的设备用作DSL连接的另一端。不想线缆网络，被网络段上的用户共享传输媒介。没DSL顾客从收发器到DSLAM都有专线，这意味着随着流量的增加，线路的性能很少受影响以至于降低。

![769151e41b214515.svg](https://i.quantuminit.com/769151e41b214515.svg)

还有几种DSL版本，例如ADSL（asyncchronous DSL, 最受小办公室以及家庭欢迎），HDSL(high bit-rate DSL)，VDSL（very high bit-rate DSL），SDSL（symmetric DSL，它的上行和下行的带宽相等）以及IDSL（ISDN over DSL）。
一些DSL设备将交换机和路由器集成在一起。其他设备充当网桥（与线缆调制解调器相似），在网络接入层通过MAC地址过滤流量。DSL设备经常使用Point-to-Point协议，封装数据，比如PPP。所谓的通过以太网协议（PPPoE）的PPP是DSL最受欢迎选项。

## Wide Are Networks

WAN技术提供快速，高带宽网络覆盖长距离。尽管WAN性能不能和LAN的性能一样快速，但是比起通过开放的互联网使用标准的网络技术连接远程网络要更快并更安全。WAN风格的连接提出一种用于互联网访问高容量团体网络的方式。

一些许多WAN选项：

- 帧中继（Frame relay）
- 集成服务数字网络（ISDN)
- 高层级的数据链路控制（HDLC）
- 异步传输模式（ATM）

WAN协议几乎总是聚焦在OSI模型上，因此要记住的是Network Access layer等同于OSI的Physical and Data Link layers，也叫一层和二层。

一个典型的WAN场景：

![b4b9276675534e3b.svg](https://i.quantuminit.com/b4b9276675534e3b.svg)

供应商保证特定带宽以及从分界点开始的服务等级。
