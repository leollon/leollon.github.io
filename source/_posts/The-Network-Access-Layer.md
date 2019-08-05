---
title: The-Network-Access-Layer
date: 2019-08-02 15:43:10
categories: Sams-Teach-yourself-TCP/IP-in-24-hours
tags: [notes, TCP/IP, Networks]
---

Network Access layer在TCP/IP协议栈中栈底，是服务以及提供和管理访问网络硬件访问规范的集合。

## Protocols and Hardware

负责工作：

- 与计算机的网络适配器配合
- 使用恰当的访问方法约定协调数据传输
- 将数据转换成一种会被转移成可通过传输媒介的电流或模拟信号的格式。
- 检查收到的数据中的错误
- 往发送出去的数据添加错误检查，为的是接收计算机能够检查数据中错误。

与操作系统和协议软件的关键底层耦合的网络适配器管理了大部分的委托给Network Access layer的任务。

## The Network Access Layer and the OSI Model

TCP/IP 的Network Access layer 大体上与 OSI的**Physical**以及**Data Link layers**相关联。

![ebdf33f8dafd4b61.svg](https://i.quantuminit.com/ebdf33f8dafd4b61.svg)

Data Link 功能划分成两部分：

- Media Access Control (MAC): 给网络适配器提供一个接口。48位的称为MAC地址的物理硬件地址存在的地方。

- Logical Link Control (LLC): 执行往子网分发的数据帧的错误检查并且管理子网通信设备间的连接。

## Network Architecture

网络架构是为了物理网络而设计的并且是在那个物理网络上定义的通信规范集合。其规范需要考虑如下情况：

- Access method: 定义计算机如何共享传输媒介的规则集合。为了避免数据冲突，计算机必须在传输数据的时候遵循这些规则。
- Data frame format: Internet layer的数据报使用预先定义的格式进行封装。被包含在头部中的数据必须提供必要的信息来
传递在物理网络的上数据。
- Cabling type: 用于网络的线缆类型对于其他的设计因素也有影响，比如适配器传输比特流（电信号）的导电性。
- Cabling rules: 传输的协议，线缆类型，以及导电性会影响电缆的最大和最小长度以及线缆连接器的规格。

Network Access layer的架构：

- IEEE802.3 (ethernet): 以太网。使用线缆作为基础的网路。
- IEEE802.11 (wireless networking): 无线局域网技术。
- IEEE802.16 (WiMAX): 用于长距离传输的移动无线连接技术。
- Point-to-Point Protocol (PPP): 用于通过电话线连接的调制解调器（modem）的协议。

TCP/IP也支持其他网络架构。由于Network Access layer封装了传输媒介的细节，协议栈的上层能够独立于硬件而运行。

![c524f9e0b45b40a5.svg](https://i.quantuminit.com/c524f9e0b45b40a5.svg)

## Physical Addressing

**physical address**总是被称为**Media Access Control(MAC) sublayer**。通过LAN发送的数据报必须使用这个physical address 来识别数据源以及目的适配器。以太网中，physical address 的长度是48比特位的，并且不是很容易记得住。
TCP/IP使用Address Resolution Protocol(ARP) 和 Reverse ARP(RARP) 来将IP Address 关联到本地网络的上的网络适配器的physical address。

## Ethernet

在一个经典的以太网网络中，所有的电脑共享共同的传输媒介。以太网使用一个成为 **carrier sense multiple access with collision detection（CSMA/CD）的接入方法来决定一台计算机什么时候可以通过传输媒介自由的传输数据。使用CSMA/CD，所有的电脑监视传输媒介并且等待直到在传输之前线路可用。当两台计算机同时传输数据，此时就会发生冲突，其中的两台电脑就得将发送数据的过程停下来，等上一个随机时间间隔后，再次尝试传输。

在现代互联网上，通过设备例如交换机来管理流量，从而减少冲突的发生，从而使得以太网能够更高效地运行。
早期以太网系统经常使用连续的共轴线缆作为传输媒介，但是目前最普遍的场景是所有计算机连接到一个单独的网络设备。

以太网能够使用各种媒介。传统的基于集线器的10BASE-T以太网最开始被用于10Mbps的基础带宽速度上。如今100Mbps相对普遍，另外，1000Mbps（gigabit）也是可用了。

共轴线缆图例：
![29b71871313549f8.svg](https://i.quantuminit.com/29b71871313549f8.svg)

使用网络设备图例：
![cf164facc2ba49f4.svg](https://i.quantuminit.com/cf164facc2ba49f4.svg)

## Anatomy of an ethernet Frame

Network Access layer 接受来自Internet Layer的数据报并转换成与物理网络规范一致的形式：

![c73d911f32a848c4.svg](https://i.quantuminit.com/c73d911f32a848c4.svg)

数据转换步骤：

1. 如有必要，将Internet layer的数据分割成可以放在在太网帧的数据字段中发送出去的更小的块。以太网帧的总共大小
在64 bytes 到 1518 bytes，不包含preamble。（一些系统支持更大的可达到9000bytes的帧大小。
这是所谓的jumpbo frames 提高了效率，但是引入了一些兼容性问题并且不是普遍地被支持）

2. 打包数据块成帧。每一帧除了包含数据还有以太网需要处理帧时的网络适配器的其他信息。
    一个IEEE 802.3以太网帧包含数据如下：

    - Preamble: 用来标记帧开始的比特序列（8字节，最后的一帧是一字节的起始帧分割符）。
    - Recipient Address: 接收这个帧的网络适配器的6-byte (48-bit) 的physical address。
    - Source Address: 发送帧的网络适配器的的6-byte (48-bit) 的physical address。
    - Optional VLan tag: 在802.1q标准中描述的可选的16-bit字段，被设计用来允许多个虚拟LANs通过相同的交换机进行操作。
    - Length: 2-byte（16-bit）用于描述数据字段大小的2-byte（16-bit）
    - Data: 与帧一起被发送的数据。
    - Frame Check Sequence (FCS): 帧的4-byte（32-bit）的摘要值。**FCS**是验证数据传输的普遍方法。发送的计算机计算
    帧的**cyclical redundancy check (CRC)**值并且在帧中编码CRC值。接收这个帧的电脑重新计算这个帧的CRC，并且检查FCS字段是否匹配。如果不匹配，那么在传输的过程中，一些数据丢失了或发生了更改，这种情况下，更高层的协议可能请求重新传输。

    出来的帧的总和等于 8 + 6 + 6 + 2 + 4 + (bytes of data) + 2(Vlan tag)

3. 传递帧到与OSI的Physical layer相关的更底层的部分，该部分将帧变成比特流并且通过传输媒介发送出去。

## 小小的概括

- Network Access layer 含有服务以及管理访问物理网络过程的规范。
- CRC是有发送帧的计算机进行计算帧而得出，在帧中编码该CRC值。接受帧的计算机重新计算帧的CRC，然后检查帧中的FCS字段是否匹配。
- 以太网的物理地址是6-byte（48-bit）的长度。
- ARP使用Logical address来解析出physical address。
- MAC layer 提供接口给网络适配器。Logical Link Control layer 提供帧的错误检查以及管理子网上网络结点之间连接。

[上一篇](/How-TCP-IP-works)

[下一篇](The-Internet-Layer)
