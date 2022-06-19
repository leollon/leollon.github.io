---
title: How TCPIP works
date: 2019-08-01 10:47:07
categories: Sams-Teach-yourself-TCPIP-in-24-hours
tags: [notes, TCPIP, Networks]
---

TCPIP是一个协议系统，并且协议是一个规则和过程的系统。

一个协议系统比如TCPIP必须负责如下任务：

- 分割消息成可以通过传输媒介高效传输的可管理的数据块。
- 与网络适配器硬件相配合
- 地址：发送数据的计算机必须能够将数据发往接收电脑。接收计算机必须能够识别应该收到的消息。
- 路由数据到目标计算机的子网，即使源子网和目的子网是不相似的物理网络。
- 执行错误控制，流控制以及通知：为了可靠的通信，发送和接收电脑必须能够识别并且纠正错误的传输以及控制数据流。
- 从一个应用接收数据并将其传递给网络。
- 从网络中接收数据并将其传递给一个应用。

使用模块化设计，将TCPIP协议系统分割成理论上一个模块的功能独立于另外一个模块，每个模块负责通信过程中的一小部分。

描述TCPIP网络的通用模型图例，但不是唯一的模型:

![4cfd4cc419584b0a.svg](https://i.cthee.cyou/4cfd4cc419584b0a.svg)

ARPAnet模型使用三层描述，Network Interface layer, Host-to-Host layer, Process-Level/Application layer.
其他的描述：Application layer, Transport layer, Internet layer, Data link layer, Physical layer.

Internet layer 有时候又可称为Internetwork layer 或 Network layer.

正式的TCPIP协议层以及其功能：

- Network Access layer: 提供物理网络的接口。为传输媒介格式化数据并且基于物理硬件地址为子网发送数据。提供在物理网络传输数据的错误控制。

- Internet layer：提供logical, 硬件独立的地址，目的是数据能够在不同的物理架构的子网中传递。提供路由来减少流量以及支持互联网间的传输。将物理地址（通常在Network Access layer）和 logical address关联起来。

- Tranport layer：提供流控制，错误控制以及互联网间的确认（acknowledgement）服务。给网络应用提供接口。

- Application layer：提供解决网络问题的应用。文件传输，远端控制以及互联网活动。还支持用于为特定操作环境下访问网络的程序的网络应用编程接口（APIs）。

发送段通过网络发送的数据每经过一层，都将该层的信息加入到数据中，这些数据与接收端上的通信层有关。在接收端中，经过每一层往上传输时，都将与该层相关的信息移除。

## OSI 模型

网络工业上的标准七层网络协议架构模型，即OSI模型。严格来讲，TCPIP并不遵循OSI模型，但是两个模型有着相似的目标。

标准四层TCPIP模型与OSI七层模型的关系：

![1789ea1fe6bd4db7.svg](https://i.cthee.cyou/1789ea1fe6bd4db7.svg)

Physical layer：将数据转换成实际上会通过传输媒介的电流或模拟脉冲信号，并且察看数据的传输

Data Link layer： 给网络适配器提供接口；维护子网的逻辑连接

Session layer：维护通信计算机上的通信应用之间的会话

Presentation layer：翻译数据成标准格式；管理加密和数据压缩

其他层与原TCPIP四层模型相似。

其中最相似之处在Transport 和 Internet（在OSI中称为Network) layers。这些层包含了最容易识别并且可区分的协议系统的部分。

## 数据包（data package）

数据包从发送端发送出去时，没经过一层，都将该层相关的信息和实际的数据一起传往下一层，这一信息称之为头部。在接收端，其过程则与发送端的过程相反。
由于每一层负责不一样的功能，因此，基本的数据包的形式在每一层都不一样。

给数据打上每层的头部的过程图例：

![fa4f802018704533.svg](https://i.cthee.cyou/fa4f802018704533.svg)

每一层的的数据包看起来都不一样，且有着不同的名字，在每一层创建的数据包名如下：

- Application layer创建的数据包称为**message**.
- 在封装Application layer message的Transport layer创建出来的数据包称为**segment**，如果数据包来自Transport layer的TCP协议。如果数据包来自Transport layer's 的User Datagram Protocol(UDP)协议，则称之为**datagram**。
- 在封装Transport layer **segment**的网络层的数据包称为datagram。
- 在封装并且可能细分datagram的Network Access layer的数据包被称为**frame**。然后这个**frame**在Network Access layer的最底层变成比特流。

## 快速浏览TCPIP网络

首先，谈论协议层而不是协议会向一个已经很抽象的主题引入一个额外的抽象。
第二，在协议层这个更大的话内，分项列出不同的协议做为子标题可能给出一种错误的印象是所有的协议是同等重要的。
而事实上，大部分的TCPIP系统中的功能就只能就它的小部分最重要的协议而言。

基本的场景有如下：

- 通过TCP或UDP传递来自一个协议，网络服务或是在Application layer的API操作的数据到Transport layer协议的两者之一（TCP 或 UDP）。根据编程的需要，程序能够通过TCP或UDP访问网络：

  - **TCP**是一个面向连接的协议，比起无连接协议，它提供了更可靠的流控制以及错误控制。TCP用了大量的努力来保证数据的
      传输，相比UDP，TCP更可靠，但是错误检查以及流控制使得TCP比UDP更慢。
  - **UDP**是无连接的协议。比起TCP更快，但不可靠。UDP将错误控制交给应用程序来负责。

- 数据段传到IP协议提供logical address 信息以及包装数据成为数据报的网络层。

- IP数据报进入到传递能与物理网络交互的已经设计好的软件组建的Network Access layer。Network Access layer 创建为了进入physical network而设计的一个或更多的数据帧（frame）。在LAN系统的例子中，比如以太网，帧可能含有通过[**ARP**](/What-is-TCP-IP/#Logical-addressing)协议从维护的表获取来的physical address的信息。

- 数据帧被转换成可以通过网络媒介传输的比特流。

基本的TCPIP协议网络系统：

![6b826ab818a64712.svg](https://i.cthee.cyou/6b826ab818a64712.svg)

## 小小的总结

- OSI模型的physical 和 data link layer与TCPIP的Network access layer相对应。

- TCPIP 的 Internet layer 负责将数据从一个网络段route到另外一个网络段。

- UDP 够简单并且更快但是没有TCP的错误检查以及流控制。

- 数据封装意思是在数据被往下传递之前，往数据中附加当前层相关的头部。

- TCP可靠是因为TCP含有错误检查以及通知服务来确保TCP段能够被传输。

[上一篇](/What-is-TCP-IP)

[下一篇](/The-Network-Access-Layer)
