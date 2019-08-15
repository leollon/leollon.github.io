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
- 描述TCP如何开启和关闭连接
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

**Transmission Control Protocol (TCP)**: TCP 提供广泛的错误控制和流控制以及验证数据的成功传递。**TCP是面向连接的协议**。

**User Datagram Protocol (UDP)**: UDP提供极其基本的错误检查并且被设计用于没必要使用TCP广泛的控制特性的时
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
当发送信息回计算机A时，要使用那个socket number的数据字段。这个就是计算机A的源socket address。

2. 计算机B通过已知端口收到来自计算机A请求并且响应和计算机A源地址列出来一样的socket。这个socket成为消息从计算机B的应用程序发送到计算机A的应用程序的目的地址。

已知的TCP端口：

![0a2e2f97a2b54fd3.png](https://i.quantuminit.com/0a2e2f97a2b54fd3.png)
![806adc354e5443a0.png](https://i.quantuminit.com/806adc354e5443a0.png)

已知的UDP端口：

![d10a759ae4254590.png](https://i.quantuminit.com/d10a759ae4254590.png)

## 多路复用/反多路复用

多路复用图例：

![418456e06e7f44e0.svg](https://i.quantuminit.com/418456e06e7f44e0.svg)

反多路复用图例：

![6482f7049b1d4d3b.svg](https://i.quantuminit.com/6482f7049b1d4d3b.svg)

多路复用/反多路复用使得TCP/IP协议栈的更底层能够处理数据而不用管是哪个应用初始化的。所有和原始应用的关联都在Transport layer进行设置，并且数据以单独的，应用独立的管道传向或来自Internet Layer。

多路复用和反多路复用的关键是由IP地址和端口（port）号组成的socket地址，它提供了一个识别特定机器上特定应用程序的独
一无二的身份。

## Understanding TCP and UDP

开发网络应用可以选择使用TCP或UDP作为传输协议。UDP更简单的控制机制不应该成为考虑的限制。更少的通信质量保证并不意味
着更低的通信质量。TCP提供的额外的检查以及控制对于很多应用来说完全没必要。开发人员更喜欢在应用自身内部提供错误控制
以及流控制，能够根据特定需求进行自定义，并且更倾向于UDP传输用于网络访问。例如，应用层的Remote Procedure Call
(RPC) 协议能够支持复杂的应用，但是开发者有时候选择使用UDP作为传输层并且通过应用而不是通过降低连接速度的TCP来提供错误与流控制。

## TCP: 面向连接的传输层协议

TCP一些值得注意的一些特性：

- 面向流处理：TCP在流中处理数据。面向流处理意味TCP能够接收数据时一次能够接收一个字节而不是像预格式化的块。TCP格式化数据成能够传递给网络层的变长的段。
- 重新排序：假设目的地收到的数据是乱序的，TCP模块能够将数据进行排序恢复到原来的顺序。
- 流控制：TCP的流控制特性确保数据传输不会超过或超限目的机器能够接收的数据量。
- 先后顺序以及安全：国防部的TCP规范需要有用于设置TCP连接的可选的安全以及优先级等级。
- 优雅的连接关闭：TCP关心连接关闭就像打开连接一样关心。优雅的关闭特性确保所有的段在连接关闭之前，已经被发送并且接
受。

从[How TCP/IP works](/How-TCP-IP-works)中，可以知道分层协议系统比如TCP/IP通过交换在发送计算机给定层和接收
计算机相对应层之间信息来运转。换句话说，发送计算机上的网络接入层和读取数据帧的计算机的网络接入层进行通信。

回想起[What is TCP/IP](/What-is-TCP-IP)中的端结点负责验证TCP/IP上通信（端结点是那些实际上要进行通信的结点——
而不是转发消息的中间结点。）。

典型的网络互联的情况，数据由路由器从源子网传递到目的子网。

![a752a33da6784b02.svg](https://i.quantuminit.com/a752a33da6784b02.svg)

## TCP Data Format

TCP 数据格式图示

![d19083c8683f40c6.svg](https://i.quantuminit.com/d19083c8683f40c6.svg)

字段说明：

- Source Port (16-bit): 在源机器上分配给应用的数据源端口号码
- Destination Port (16 bit): 在目的机器上分配给应用的数据目的端口号码
- Sequence Number (32-bit): 除非*SYN*标记设计为1，否则在特定的段中，它将会是第一个字节的序列号。如果*SYN*设
置为1,序列号字段提供初始的序列数字（ISN），它用来同步序列号。如果*SYN*设置为1，第一个八位字节的序列号比出现在这个
字段中的数字要大（换句话说，就是ISN + 1）。
- Acknowledgment Number (32-bit): ACK号声明收到的段。这个值是接受计算机希望收到的下一个序列号的值。换句话
说，就是最后的一个收到的字节的序列号 + 1。
- Data offset (4 bits): 告诉接收的TCP软件header有多长以及数据从何处开始的字段。这个字段用一个32-bit字长的整数来表示。
- Reserved (6 bits): 为未来的使用而预留的空间。预留字段为适应将来的TCP发展提供空间并且全部是0.
- Control flags (1 bit each): 声明关于段的的特殊信息。
- URG: 值为1的字段，声明该段是紧急的并且Urgent Pointer 是有意义的。
- ACK: 值为1的ACK声明Acknoledgement Number字段是重要的。
- PSH: 值为1的字段告诉TCP软件推送到目前为止通过管道发送的所有数据到接收应用的中。
- RST: 重新设置连接的值为1的字段。
- SYN: 值为1的字段声明被同步的序列号，标记连接开始。[three-way handshake](#Establing-a-Connection)
- FIN: 值为1的字段暗示发送端没有更多的数据要传输了。这个标记被用于关闭一个连接。
- Window (16-bit): 用于流控制的参数。除了发送端自由传送且没有跟进一步通知的最后的acknowledged的序列号之外，
window定义了序列号的范围。
- Checksum (16-bit): 用于检查段的完整性的字段。接收端执行段的checksum计算并且比较这个计算出来的值与存储在字段
中的值。TCP和UDP在checksum计算中包含有一个含有IP地址信息的伪头部。[UDP 伪头部讨论](#UDP-The-Connection-Transport-Protocol)
- Urgent Pointer (16-bit): 指向标记任何紧急信息开头的序列号的偏移指针。
- Options: 指定小部分可选设置中一个。
- Padding: 确保数据起始于32-bit边界的额外的0 bits（根据需要）位。
- Data: 和段一起被传送的数据。

TCP需要所有这些字段来完成管理，通知以及验证网络传输。

### TCP Connections

TCP中的一切都发生在连接的上下文中。TCP支持两种连接打开状态：

- 被动打开：应用进程通知TCP准备通过一个TCP端口来接收到来的连接。因此，从TCP到应用的路径在到来的连接请求之前被打开。
- 主动打开：应用请求TCP初始化与另一台处于被动打开状态的的计算机的连接。（实际上，TCP也能够初始化一个连接到处于主动打开状态的计算机的连接，万一两台计算机都在马上尝试打开一个连接。）

客户端是一台计算机请求或收到来自网络上的另一台计算机的服务，服务器是一台计算机给网络上的其他计算机提供服务。

TCP发送变长的段；数据的每个字节被分配一个序列号。接收端必须为它收到的每个字节发送一个通知。因此，TCP通信是一个传输
以及确认的系统。TCP头部的Sequence Number 以及Acknowledgement Number 字段提供关于传输状态的常规更新给通信的
TCP软件。

分开的序列号不会和每个独立的字节一起被编码。而是，header中的Sequence Number 字段给出段中数据的第一个字节的序列
号。
如果段出现在连接的开头（在[three-way handshake](#Establing-a-Connection)中描述），Sequence Number
字段包含ISN，它的值要比段中数据第一个字节的序列号少1。（也就是第一个字节等于ISN + 1。）
如果成功收到段，接收的计算机使用Acknowledgement Number字段告诉发送的计算机已经收到那个字节了。
acknowledgement 信息中的Acknowledgement Number字段被设置为最后收到的sequence number + 1。换句话说，
Acknowledgement Number 字段定义了接收的计算机下一次接收的sequence number。
如果在指定的时间周期内，未收到一个acknowledgement，发送的计算机重传以最后一个被通知的字节的那个字节为开始的数
据。

### Establing a Connection

计算机B必须要知道计算机A使用什么ISN（初始序列号）来起始序列号。计算机A必须要知道计算机B将会使用什么ISN来为计算机B
传输的任何数据起始序列号。序列号的同步称为一次**three-way handshake**。它总是出现在TCP连接开始时。

![a2b94c3679534b47.svg](https://i.quantuminit.com/a2b94c3679534b47.svg)

在结束three-way handshake之后，连接建立了，并且TCP模块使用sequence 以及acknowledgemen 方案进行传输和接受数
据。

### TCP Flow Control

TCP header中的Window字段为连接提供流控制机制。Window字段的目的是确保发送端别太快地发送太多的数据，这可能因为接
收端的处理速度不能和发送端的发送速度一样尽可能快地处理来到的数据段，从而导致出现数据丢失的情况。TCP使用的流控制方
法称为**滑动窗口**方法。除了发送端认可传输的最后的acknowledged sequence number之外，接收端使用
Window字段（也称之为**缓冲区大小**字段）来定义序列号。

### Closing a Connection

当是时候关闭连接时，计算机初始化关闭操作，计算机A在队列中放置一个*FIN*设置为1的数据段。
应用程序进入到**fin-wait 状态**。在fin-wait状态中，计算机A的TCP软件继续接受数据段并且处理已经在队列中的数据
段，但是不会接收来自应用程序的额外的数据。当计算机B收到*FIN*数据段时，它返回一个acknowledgement来响应*FIN*，并
发送任何剩余的数据段以及通知本地的应用程序，收到了一个*FIN*。计算机B发送一个*FIN*数据段给计算机A，这个数据段计算
机A会确认(acknowledge)并且关闭连接。关闭连接图示：

![85a28dee69f444e8.svg](https://i.quantuminit.com/85a28dee69f444e8.svg)

## UDP: The Connection Transport Protocol

UDP要比TCP更简单。关于UDP，还是有些东西值得关注的。
首先，尽管UDP有时候和描述的一样没有错误检查的能力，事实上，它能够执行基本的错误检查。最好将UDP描述成具有有限的错误
检查能力。UDP数据报包含一个接收端能够用于测试数据完整性的checksum值。（checksum测试是可选的并且能够在接收端被关
闭来加速处理进来的数据。）UDP数据报包含一个含有数据报目的地址的伪头部，因此，也提供了检查错误数据报的方式。并且，
如果接收端模块接收到一个传到未激活或未定义的UDP端口，则返回ICMP消息来通知数据报源计算机，端口不可达。
第二，UDP不提供TCP提供的数据重新排序功能。大型网络上，重新排序最具有重要意义。比如，在互联网中，数据段可能走不同的
路径并且在路由缓冲中经历了较高的延迟。在本地网络中，在UDP中缺少重新排序特性不会导致不可靠的接收。

UDP的简单，无连接设计使得它能够用于网络广播的情况中。广播是子网上所有的计算机都会收到并且处理的一个单独的消息。

UDP协议的主要目的是将数据报公开给应用层。UDP不传输丢失或损坏的数据包，削除重复的数据报，通知数据报的接收，或建立或
终止连接。
UDP header由4个16-bit的字段构成。如下图所示：

![2193b220d298492b.svg](https://i.quantuminit.com/2193b220d298492b.svg)

字段说明：

- Source Port: 这个字段占用UDP头部的头16 bits。它保存应用发送数据包的UDP端口号。输入到这个字段的值被接收端的
应用用来当他准备好发送时，作为响应的返回地址。这个字段可以考虑为可选的，并且不要求发送数据报的应用包含它的端口号
码，希望应用程序能够放16个 0 bits 到这个字段中。显然，如果没有有效的端口，接收数据报的应用将不能发送响应。然而，
万一是一则单向的消息，不期待有响应返回时，这可能会是需要的功能。
- Destination Port: 保存接收端上UDP软件传输这个数据报到的端口地址，它是16-bit的字段。
- Length: 16bit的字段。该字段以UDP数据报的八位字节识别数据报长度。该长度包含UDP头部还有要传输的数据的长度。因为
UDP头部是8个八位字节的长度，所以，这个值至少为8字节。
- Checksum: 用于决定这个数据报在传输过程是否被损坏的16 bit字段。这个字段的值是通过执行特殊计算一串二进制而得到
的结果。在UDP例子中，checksum是基于一个伪头部，UDP头部，UDP数据，还有可能是填充的八位字节的0来构成一个偶数个八
位字节长度的checksum输入值而计算的。发送端生成，接收端用来决定数据报是否已经被损坏。

部分用于checksum计算的数据是从IP header又称伪头部中提取出来的一个字符串。伪头部提供目的地址信息，这样接收端能够
用于决定一个数据报是否已经被错误传输。

### 其他传输层协议

- Datagram Congestion Control以及Stream Control Transmission Protocol提供在传统的TCP和UDP中没有的一些
增强特性
- Real-time Transport Protocol (RTP) 提供用于传输实时音频以及视频的结构。

## Firewalls and Ports

防火墙是一个预防局域网被未经授权的用户从互联网上访问的协议系统。防火墙的一个重要特性是能够阻止访问特定的TCP和UDP
端口。禁止局域网外的用户访问局域网内的SSH 服务器以及局域网内的任何一台计算机。

![8fa1d412400a4570.svg](https://i.quantuminit.com/8fa1d412400a4570.svg)

防火墙能够阻止访问任何或所有可能暴露出安全威胁的端口。

## 小小的总结

- 传输层职责提供API，多路复用/反多路复用，流控制，错误检查以及验证，
- TCP是面向连接且具有更为健全的错误检查，以及使用滑动窗口方法进行流控制，接收端通过比较数据段中checksum以及重新
对Data进行计算出来的checksum来进行验证。
- 通过将port和地址连接起来构成socket，这个socket在系统中是独一无二的，可用于识别网络应用，应用则通过这个
socket，可称为管道，给传输层发送数据。
- TCP与UDP异同
  - 同：都属于传输层，通过socket识别网络应用。
  - 异：TCP面向连接且提供更为健全的错误检查，流控制，数据段重排序，具有可靠传输特性。UDP面向无连接，相比TCP更简
  单且提供简单的错误检查，UDP数据报长度至少为8字节。
- [TCP header字段](#TCP-Data-Format)
- [TCP开启连接](#Establing-a-Connection)
- [TCP关闭连接](#Closing-a-Connection)
- [TCP 如何序列并通知数据传输](#TCP-Connections)
- [UDP header的四个字段](#UDP-The-Connection-Transport-Protocol)：
  - Source Port
  - Destination Port
  - Length
  - Checksum

[上一篇](/Subnetting-and-CIDR)

[下一篇](/The-Application-Layer)
