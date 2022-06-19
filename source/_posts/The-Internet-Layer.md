---
title: The Internet Layer
date: 2019-08-05 10:02:41
categories: Sams-Teach-yourself-TCPIP-in-24-hours
tags: [notes, TCPIP, Networks]
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

TCPIP隐藏了physical address，而是围绕一个logical，hierarchical addressing scheme来组织网络。Internet layer的**IP protocol**维护着称之为**IP address**的logical address。还有一个称为**Address Resolution Protocol（ARP）的Internet layer protocol，它维护一张IP addresses 到 physical address 的表。

在一个路由网络上上，TCPIP在网络上发送数据的策略：

1. 如果目的地址作为源计算机在同样的网络段上，源计算机直接发送包到目的地址。使用ARP将IP Address 解析为physical address，并且数据被发送到目的网络适配器。。

2. 如果目的地址与源计算机不是在同一个网络段上，则进行如下步骤：
  
- 数据报被发往网关（gateway）。网关是一个在本地网络段上能够转发数据报到其他网络段的设备。在[What is the TCPIP](/What-is-TCP-IP)中，可以知道网关基本上是一个路由器（router）。网关地址通常通过TCPIP配置来定义。使用[ARP](/What-is-TCP-IP/#Logical-addressing)，将网关地址解析出physical addres，并且数据被发往网关的网络适配器。
- 数据包通过网关路由到更高级的网络段（如图1），这个过程将在那里重复。如果目的地址在新的网络段，数据被发往它的目的地。如果不是，数据报发往新的另一个网关。
- 数据报通过一连串的网关到达目的网络段，在目的网络中使用ARP将目的IP address 映射到 physical address，并且数据被发往目的网络适配器。

网关接收发往其他网络的数据报、图1：

![9a541258502c4fb5.svg](https://i.cthee.cyou/9a541258502c4fb5.svg)

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
![7f71074523bf4ac8.svg](https://i.cthee.cyou/7f71074523bf4ac8.svg)

大型网络必须为大量的主机保留大量的主机bit位数，小型网络不需要太多的bits去给定主机一个唯一的ID。然而大量的小型网意味着
需要更多的IP address的bits来产生network ID。

三类IP地址：

- A类网络地址： 使用IP address 的前8 bits作为network ID, 剩余的24 bits用做host ID。
- B类网络地址： 使用IP address 的前16 bits作为network ID, 剩余的16 bits用做host ID。
- C类网络地址： 使用IP address 的前24 bits作为network ID, 剩余的8 bits用做host ID。

通过子网划分特性来扩展控制区域网络结构的系统。
Classless InterDomain Routing（CIDR）提供了简单，灵活并且无二异性的用于指定多少bit位属于network ID的标记法。
这一标记方案的目的是：将IP address 分割成network ID 和 host ID。

## IP Header Fields

每一个IP数据报以一个IP header开头。IP数据报的源计算机的TCPIP软件构造该 IP header，目的计算机上软件使用这个包含在IP header中的信息处理该数据报。Header包含了大量的信息，其中包含了源计算机和目的计算机的IP 地址，数据报的长度，IP 版本号以及特殊的路由指令。

**IP header的最小值是20 bytes。**

IP header的内容：
![2283d52d20c9421c.svg](https://i.cthee.cyou/2283d52d20c9421c.svg)

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
- Time to Live (TTL): 这个位字段表示数据被丢弃之前能生存或者路由跳跃（hop）的时间（用秒表示）。每个路由器检测并且按至少1为单位或者数据报在路由器中被延迟的秒数来减少这个字段。当这个字段为0时，将数据报丢弃。Hop表示的是数据报前往目的地中经过
的路由器数量。
- Protocol: 8-bit协议字段暗示将收到数据载荷的协议。带有协议标识6（二进制 00000110）的一个数据报被往栈朝上传递
到达TCP模块 。
常见的协议标识值：

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

地址翻译规则：

- 32-bit 二进制地址以0开头，则这个地址是A类地址
- 32-bit 二进制地址以10开头，则这个地址是B类地址
- 32-bit 二进制地址以110开头，则这个地址是C类地址

三类网络地址的范围表：

|地址类|二进制位地址开头必须是|第一个点号开头的十进制数字|包含的地址|
|:----|:----|:----|:----|
|A|0|0 到 127|10.0.0.0 到 10.255.255.255,127.0.0.0 到 127.255.255.255|
|B|10| 128 到 191| 172.16.0.0 到 172.31.255.255|
|C|110|199 到 223| 199.168.0.0 到 199.168.255.255|

特殊用途的D类和E类地址
D类地址用于多播（multicasting），多播是给一个网络的子集发送一条单独的信息，而不是被本地网络上的所有结点处理的广播。D类
网络地址的二进制的最左边前四位是1110，通过计算得到它的范围是224 到 239。

E类网络地址是被当作实验性的并且不会在生产环境中被使用。E类网络地址的二进制的最左边前五位是11110，通过计算可以得到范围是240 到 247。

使用代理服务器和Network Address Translation(NAT)使得未注册并且非独一无二的地址可以访问互联网。

十进制IP地址转换成八位二进制的两种方式，比如数字199，在二进制中使用0和1来表示一个十进制的数字。

- 方式一
  每次按位来求十进制数字在八位二进制中的1或0。
  1. 因为要按位求，那么用一位二进制数字在八位二进制中能表示的最大的数值是128(10000000)，而199 > 128，被转换的数字
  （199）在八位二进制中第八位为1，199和128的差值是71。
  2. 求第七位，那么此时又用一个1在7位二进制中能表示的最大值是64(1000000)，而71 > 64，被转换的数字
  （199）在八位二进制中第七位为1，71和64的差值为7.
  3. 求第六位，六位二进制中用一个1表示的最大的十进制的值是32，而7 < 32，被转换的数字（199）在八位二进制中第六位为0，
  此时用7减去0得差值7。
  4. 求第五位，五位二进制中用一个1表示的最大的十进制值是16，而 7 < 16，被转换的数字（199）在八位二进制中第五位为0，此时用7减去0得插值7。
  5. 求第四位，四位二进制中用一个1表示的最大的十进制值是8，而 7 < 8，被转换的数字（199）在八位二进制中第四位为0，此时用7减去0得差值7。
  6. 求第三位，三位二进制中用一个1表示的最大的十进制值是4，而 7 > 4，被转换的数字（199）在八位二进制中第三位为1，此时用7减去4得差值3。
  7. 求第二位，二位二进制中用一个1表示的最大的十进制值是2，3 > 2，被转换的数字（199）在八位二进制中第二位为0，此时用3减去2得差值1。
  8. 求第一位，一位二进制中用一个1表示的最大的十进制值是1, 1 == 1，被转换的数字（199）在八位二进制中第一位为0，此时用1减去1得差值0。
  差值为0，终止这个过程。
  
  到此，八位二进制中的各位都算出来完了，因此199的八位二进制是11000111。

- 方式二
  除二取余

## Special IP Address

全部是0的host ID是网络本身，全部是1的是广播地址。广播是发送给网络上所有主机的一条消息。255.255.255.255也可以用于网络上的广播。127开头的网络是loopback address。后来更新为 RFC 1918的RFC 1597保留一些IP地址范围用于私有网络。假设这写私有地址不会连接互联网，所以这些地址没必要是唯一的。这些私有地址经常用于位于NAT设备后的受保护的网络。

- 10.0.0.0 到 10.255.255.255
- 172.16.0.0 到 172.31.255.255
- 192.168.0.0 到 192.168.255.255

169.254.0.0 到 169.255.255.255被保留用于自动配置。

## Address Resolution protocol

ARP 能够将IP address 映射到 physical address。网络段上的每个主机都在内存中维护着称之为ARP table或者ARP cache的表。当主机需要发送数据给网络段上另外一台主机时，主机将检查这个表来决定数据接收端的物理地址（physical address）。如果要接收的物理地址不存在表中，主机将发送一个ARP请求帧的广播。该请求帧包含有未解析的IP地址和发送请求帧的主机的IP地址以及物理地址。

ARP 映射图例：
![2a21b232a5684d55.svg](https://i.cthee.cyou/2a21b232a5684d55.svg)

ARP 缓存表中的条目在过了一段时间会失效，失效的条目将会被移除，解析过程将在下一次发送数据的到过期条目的IP地址时进行。

## Reverse ARP

Reverse ARP 与 ARP相反，知道物理地址，但是不知道IP地址时，将使用RARP。RARP总是被经常用来和**BOOTP**协议协同启动无盘
工作站。

**BOOTP** (Boot PROM)
许多网络适配器有一个可以用来插入一个集成电路（boot PROM)的插槽。boot PROM一旦计算机开机，它也跟这启动。将从一个网络服务器加载一个操作系统到这台计算机中，而不是通过本地磁盘。操作系统载入到这个预先配置好指定IP地址的BOOTP设备中。

## Internet Control Message Protocol

routers使用 Internet Control Message（ICMP）消息来通知源IP，发送数据过程中出现的问题。

最常见的ICMP消息：

- Echo Request and Echo Reply: ICMP 经常在测试期间被使用。例如`ping`命令用来检测网络的连通性。`ping`命令往目的IP地址发送一个数据报并且请求目的计算机在发送的响应数据报中返回数据。
- Source Quench: 假设一台超快的计算机往远程电脑发送大量的数据，这些数据量可能超过路由器的处理能力。此时路由器使用ICMP
给源IP发送*Source Quench*消息，请求源IP降低发送数据的频率。如有必要，额外的Source Quench消息能够被发送到源IP。
- Destination Unreachable: 如果一个路由器收到不能传输的数据报，ICMP返回Destination Unreachable 消息给数据报源IP。
- Time Exceeded: 由于TTL为0了，如果数据报被丢弃，ICMP发送这个消息给数据报源IP。暗示目的地有太多路由要hop，以至于达到
当前的TTL，或着意味着路由表问题导致数据报不断地通过同样的路由器。当数据报不断地循环并且从没到达目的地址时，产生**路由循环**。
当网络管理员将静态路由条目放进路由表的时候，有时候会发生路由循环。
- Fragment Needed: 如果收到的数据报中没有设置**Don't Fragment**位，并且如果路由器需要将数据报进行分片转发到下一个路由其或者目的时，ICMP发送这个消息。

## Other Internet Layer Protocols

比如，IPsec protocols在IPv4中是可选的，但确是IPv6整体的一部分，提供安全的加密通信。其他Internet layer 协议协助完成任务，比如多播。

## 小小的总结

- IP header的最小值是20 bytes。
- IP Header中的TTL用于说明数据报在被丢弃之前存活的时间，或者路由跳跃的时间。
- A类地址网络ID必须是以0开头的8位二进制，主机ID是24位二进制。
- 最常见的ICMP消息: Echo Request and Echo Reply, Source Quench, Destination Unreachable, Time Exceeded, Fragment Needed.
- 0开头的8 bit 的 network ID 以及 24 bit 的 host ID 的A类地址。
- 10开头的16 bit 的 network ID 以及 16 bit host ID 的B类地址。
- 110开头的24 bit 的 network ID 以及 8 bit host ID 的C类地址。
- 1110开头的32 bit 的用于多播的D类地址。
- 11110开头的32 bit 的用于使用实验性但是没在生产环境中使用的E类地址。
- ARP通过IP地址解析出MAC地址，RARP通过MAC地址访问远程主机设置本地的IP地址。
- 数据包的目的地址不存在路由表中时，将向网络上的所有结点发送广播，发送广播的时，也一同发送该结点的物理地址以及IP地址，含有该目的地址的结点通过该物理地址发送该数据目的地址给发送广播的主机并更新主机的路由表。
- 路由表中的条目有过期的时候，过期的条目将从路由表中移除，下一次更新路由表是在发送数据到过期条目的IP地址时进行。

[上一篇](/The-Network-Access-Layer)

[下一篇](/Subnetting-and-CIDR)
