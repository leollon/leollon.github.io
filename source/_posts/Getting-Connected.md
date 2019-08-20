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

## Wireless Networking

### 802.11 Networks

从[The Network Access Layer](/The-Network-Access-Layer/)中得知，物理网络的细节驻留在TCP/IP协议栈的Network Access layer。**IEEE 802.11**规范为网络接入层的无线网络提供了模型。

![489b1c2f1eea41f1.svg](https://i.quantuminit.com/489b1c2f1eea41f1.svg)

因为802.11与IEEE 802.3以太网标准相似且兼容，802.11经常称为无线以太网。
物理层的不同选项表示不同的无线广播格式，frequency-hopping spread spectrum (FHSS)，direct-sequence spread spectrum（DSSS），othogonal frequency-division multiplexing（OFDM）以及 high-rate direct-sequence spread spectrum multiplexing（HR/DSSS）。
无线网络与有线网络不同的一个品质是结点是移动的。换句话说，网络能够响应连接设备的位置改变。

#### 802.11 Family

802.11是一些列标准的集合名字。
|标准|频率范围|传输速度高达|
|:----|:----|:----|
|初始版本802.11|2.4GHz|2Mbps|
|802.11a|5GHz|54Mbps|
|802.11b|2.4Ghz|5.5Mbps&11Mbps|

还有802.11g（2003年采取）802.11n (2009年采取)，还有其他版本。802.11ac作为高速选项不断受到喜爱。

#### Independent and Infrastructure Networks

最简单的无线网络形式有两个或更多携带有无线网卡的设备直接与彼此进行通信。这种类型的网络正式地被称为**独立基础服务集合（independent BSS，或IBSS）**，更常见的名称是自组网络。适用于紧凑空间内少部分计算机之间的通信。Independent BSS网络是有局限性的，因为它依赖参与通信的计算之间的接近，没有基础设施用于管理连接以及没有办法和更大的网络进行连接，比如本地局域网或者互联网。

![ccb646f6530a4edb.svg](https://i.quantuminit.com/ccb646f6530a4edb.svg)

另一种无线网络形式，称为**基础设施基础服务集合（infrastructure BSS)**，在团体网络以及公共机构环境中更常见，并且由于新一代的便宜的无线路由设备，使得它成为家庭以及咖啡店的受欢迎的网络选项。Infrastructure BSS依赖一个称为**接入点（access point，AP）**的固定设备来促成无线设备之间的通信。一个接入点通过无线广播与无线网络进行通信并且通过传统的连接连接到普通的以太网网络。无线设备通过接入点进行通信。无线设备通过发送一个帧到接入点并让接入点传输消息到目的地来完成与同一区域的其他无线设备进行通信。

![878a9878e6014e7c.svg](https://i.quantuminit.com/878a9878e6014e7c.svg)

通过多个接入点服务更大的区域：

![808959b6286540ac.svg](https://i.quantuminit.com/808959b6286540ac.svg)

802.11帧提供四个地址：

- 目的地址：帧到达的目的设备
- 源地址：发送帧的设备
- 接收器地址：应该处理802.11帧的无线设备。如果帧发送给的是一个无线设备，接收器地址和目的地址一样。如果帧发送到的设备超出无线网络，接收器地址是接收并转发帧到以太网分布式网络中的接入点的地址。
- 传输器地址：转发帧到无线网络上的设备的地址。

802.11帧格式中的一些重要字段：

- Frame control： 描述用于翻译帧内容的协议版本，帧类型以及其他必要值的更小字段的集合。
- Duration/ID：提供评估大约传输持续多长时间的字段。这个字段可能也从接入点请求缓存的帧。
- Address fields：48-bit物理地址字段。根据帧的类型，这个字段的使用也不同。第一个字段通常是接收器的并且第二字段通常是传输器的。
- Sequence control：分片编号（用于分片重组）以及帧的序列号。
- Frame body：与帧一起传输的数据。当然，和帧一起传输还包含有上层的协议头部。
- Frame Check Sequence (FCS)：环形冗余校验，用于检查传输错误以及验证帧在传输过程中没有被修改。

![e06f0810ddec4b44.svg](https://i.quantuminit.com/e06f0810ddec4b44.svg)
