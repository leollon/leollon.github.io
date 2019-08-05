---
title: The-Internet-Layer
date: 2019-08-05 10:02:41
categories:
tags:
---

这里将围绕整个互联网使用的32-bit 二进制的的IPv4 address。而如今这个网络世界正过渡到新的128-bit 地址系统，它提供增强的
能力以及更大的地址空间。

## IP Address: A Little Context

使用DHCP自动动态分配IP address给计算机。

## Addressing and Delivering

一个设备比如以太网网卡并不知道上层协议层的任何细节。不知道它的IP address 或者收到的数据是发送给SSH 还是 FTP。
它负责监听发送过来的帧，等待一帧编址为它被发往的物理地址，并且往协议栈向上传递那个帧。
physical address scheme 能够很好地在个人的LAN段中正常工作。除了需要physical address之外，在连续的传输媒介上由几台
计算机构成的网络也能够正常运转。使用和Network Access layer关联的底层协议，数据能够直接从一个网络适配器直接传递到另一个
适配器。

不巧的是，在路由网络上，不可能通过physical address 传输数据。发现过程需要通过physical address来传递并不能通过router interface。即使能成功通过physical address 传输，也会是非常麻烦的，因为被内置永久physical address 的网卡不允许在地址空间上施加逻辑结构。

TCP/IP隐藏了physical address，而是围绕一个logical，hierarchical addressing scheme组织网络。Internet layer的**IP protocol**维护着称之为**IP address**的logical address。还有一个称为**Address Resolution Protocol（ARP）的Internet layer protocol，它维护一张IP addresses 到 physical address 的表。

在一个路由网络上上，TCP/IP在网络上发送数据的策略：

1. 如果目的地址作为源计算机在同样的网络段上，源计算机直接发送包到目的地址。使用ARP将IP Address 解析为physical address，并且数据被发送到目的网络适配器。。

2. 如果目的地址与源计算机不是在同一个网络段上，则进行如下步骤：
  
- 数据报被发往网关（gateway）。网关是一个在本地网络段上能够转发数据报到其他网络段的设备。在[What is the TCP/IP](/What-is-TCP-IP)中，可以知道网关基本上是一个路由器（router）。网关地址通常通过TCP/IP配置来定义。使用[ARP](/What-is-TCP-IP/#Logical-addressing)，将网关地址解析出physical addres，并且数据被发往网关的网络适配器。
- 数据包通过网关路由到更高级的网络段（如图1），这个过程将在那里重复。如果目的地址在新的网络段，数据被发往它的目的地。如果不是，数据报发往新的另一个网关。
- 数据报通过一连串的网关到达目的网络段，在目的网络中使用ARP将目的IP address 映射到 physical address，并且数据被发往目的网络适配器。

网关接收发往其他网络的数据报、图1：

![9a541258502c4fb5.svg](https://i.quantuminit.com/9a541258502c4fb5.svg)

为了在复杂的路由网络中传输数据，Internet layer protocols 必须能够：

- 识别本地网络上的任何计算机
- 提供决定一则消息什么时候必须被发送穿过网关的方式
- 提供识别目的网络段的独立于硬件的方式，为了数据报能够有效地通过路由器（routers）到达正确的网络段。
- 提供转换目的计算机的logical address（IP Address）到physical address 的方式，为的是数据能够被发送到目的计算机的网络适配器。

## Internet Protocol

Internet Protocol（IP）提供层次的，不以来硬件的的地址系统并且在复杂的路由网络上提供用于传输数据需要的服务。
一台计算机充当路由器或者代理服务器时，必须有不只一个的网络适配器，并且因此有不只一个的IP address。*host*总是用于形容一个关联IP address的网络设备。在某些操作系统，有可能给一块单独的网络适配器分配不只一个的IP address。

IP address 被分割成两部分：

- The network ID
- The host ID

通过观察IP address 可以知道主机在那个网络或者子网：
![7f71074523bf4ac8.svg](https://i.quantuminit.com/7f71074523bf4ac8.svg)

大型网络必须为大量的主机保留大量的主机bit位数，小型网络不需要太多的bits去给定主机一个唯一的ID。然而大量的小型网意味着
需要更多的IP address的bits来产生network ID。

三类IP地址：

- A类网络地址： 使用IP address 的前8bits作为network ID。
- B类网络地址： 使用IP address 的前16bits作为network ID。
- C类网络地址： 使用IP address 的前24bits作为network ID。

通过子网划分特性来扩展控制区域网络结构的系统。
Classless InterDomain Routing（CIDR）提供了简单，灵活并且无二异性用于指定多少bit位属于network ID的标记法。
这一标记方案的目的是：将IP address 分割成network ID 和 host ID。

## IP Header Fields

每一个IP数据报以一个IP header开头。IP数据报的源计算机的TCP/IP软件构造该header，目的计算机使用这个包含在
数据报中header处理该数据报。Header包含了大量的信息，其中包含了源计算机和目的计算机的IP 地址，数据报的长度，IP 版本号以
及特殊的路由指令。

**IP header的最小值是20 bytes。**

IP header的内容：
![2283d52d20c9421c.svg](https://i.quantuminit.com/2283d52d20c9421c.svg)

- Vesion: 4-bit表示使用的IP版本。当前的版本是4，用二进制表示就是0100。
- IHL（Internet Header Length）: 4-bit字段给出在32-bit字长中IP header的长度。在32-bit字长中，
最小的header长度是5。用二进制表示是0101。
- Type of Service: 源IP能够制定特别的路由信息。一些路由器忽略这个字段，随着在quality of service（QoS）技术
出现，这个字段引起了更多的关注。这个**8-bit**的字段的最初目的是提供优先等待的数据通过路由器的方式。如今的大部分的
IP实现简单的在这个点段填满0。
- Total Length: 这个16-bit的字段用八进制的数字表示IP数据报的长度。这个长度包含了IP header以及data负载的两个长度。
- Identification: 16-bit的字段是一个被添加到源IP发送的消息中的递增序列数字。当一则消息被发往IP layer时并且消息太大而不能放入一个数据报中，IP 将这个消息分成多个数据报，给所有的数据相同的识别数字。这个数字在接收端被用来重组原消息。
- Flags: Flags 字段表示分片的可能性。第一位（bit）不会被使用并且总是为0。下一位是*DF*（Don't Fragment）标记。这个标记暗示分片是允许的（value=0）还是不允许（value=1）。再接着下一位是*MF*（More Fragments）标记，它它告诉接收方还有更多的分片进来。*MF*为0时，没有更多的分片需要被发送了或者数据报没有被分片。
- Fragment Offset: 这个13-bit的字段是一个被分配给每个连续分片的数值。目的网络层使用这个这个值来重组分片形成合适的顺序。这里找到偏移值一个8-byte单位的数字表示偏移量。
- Time to Live (TTL): 这个位字段表示数据被丢弃之前能生存或者路由跳跃（hop）的时间（用秒表示）。每个路由器检测并且按至少1为单位或者数据报在路由器中被延迟的秒数来减少这个字段。当这个字段为0时，将数据报丢弃。Hop表示的是数据报到目的地之前经过
的路由器数量。
- Protocol: 8-bit协议字段暗示将收到数据载荷的协议。带有协议标识6（二进制 00000110）的一个数据报被往栈朝上传递
到达TCP模块 。
普遍的协议标识值：

|协议名|协议标识值|
|:----|:----|
|ICMP|1|
|TCP|6|
|UDP|17|

- Header Checksum: 这个字段存储16-bit的只用来验证header有效性的计算值。这个字段和TTL字段递减运算一样，在每个路由器
内都会被重新计算。
- Source IP Address: 存储数据报源地址且长度为32-bit的字段。
- Destination IP Address: 长度为32-bit并且被目的IP用来验证正确传输数据报以及存储数据报目的地址的字段。
- IP Options: 这个字段支持一些起初用来测试，调试以及安全相关的可选的头部设置。Options含有严格源路由（数据包应该遵循的路由路径），互联网时间戳（每个路由器的时间戳记录）以及安全约束。
- Padding: IP Option 字段长度可能不一样。Padding字段提供额外的0 bits，以便于总的头部长度确实是32 bits的倍数。
（头部必须在32-bit字长之后结束，因为IHL字段按32-bits字长来计算头部的长度。）
- IP Data Payload: 这个字段包含传递给TCP或UDP（在Transport layer），ICMP，或IGMP的数据，数据量是变化的但是可能
包含上千字节。

## IP Addressing

一个IP address 是一个32-bit的二进制地址。可将这个二进制地址分割成4个8-bit的称之为八位字节的段。
使用带点十进制数来表示32-bit的二进制IP address。每八位二进制（4 × 8 = 32 bits）得出一个相等是十进制数字。
四个十进制值使用点号分开。八位二进制可以表示0到255之间的所有数字。
