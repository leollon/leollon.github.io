---
title: IPv6-Next-Generation
date: 2019-09-11 21:40:32
categories: Sams-Teach-yourself-TCP/IP-in-24-hours
tags: [notes, TCP/IP, Networks]
---

本文目的在于：

- 讨论为什么必须需要一个新的IP地址系统的原因
- 描述IPv6头部的字段
- 写下并且简化IPv6地址所应用的约定
- 将存在的IPv4地址映射到IPv6地址空间
- 理解IPv6多广播以及邻居发现
- 描述一些深受欢迎的IPv6隧道选项。

## Why a New Ip

原因是32-bit IP地址即将耗尽。IPv4能够继续控制互联网的原因是[NAT](//Configuration/#Network-Address-Translation)的开发。IPv6的IP地址格式需要128-bit。该地址空间有足够的空间来适应在IPv4地址和IPv6地址之间的一些兼容性。

IPv6的一些目标：

- **扩充地址分配能力**：IPv6不仅仅是提供更多的地址，而且给IP地址分配提供其他的改善。例如IPv6支持更多的分层地址结构。IPv6还改善了自动配置能力并且给任播地址分配（**anycast addressing）**提供更好的支持，它使得一个收到的数据报到达一组可能的目标中的“最近”或是“最好”的目的地。
- **更简单的头部格式**：省略了一些IPv4中的头部字段。其他字段变成可选的。
- **更好地支持扩展性和选项**：IPv6在可选扩展头部中包含一些头部信息。这个方法在不浪费主要头部空间的情况下增加了可能的信息字段的范围。大多数例子中，这些扩展头部不会被路由器处理。提高传输过程中的效率。
- **流标签**：IPv6数据报能够被标记为特定流标签。流标签是一类需要特殊处理方法的数据报。例如，实时服务的流标签可能不同于邮件信息的流标签。流标签设置能够对于确保传输过程中的最小服务质量起到作用。
- **改善后的验证以及隐私**：IPv6扩展支持验证，保密以及数据完整性技术。

### IPv6 头部格式

相比IPv4头部，IPv6头部要更简单的部分原因是更详细的信息被归属入跟在主要头部后的特殊的扩展头部中。

![https://i.quantuminit.com/2c59efb426604e60.svg](https://i.quantuminit.com/2c59efb426604e60.svg)

字段说明：

- Version（4-bit）：版本。标识IP版本号（这个例子中是6）。
- Traffic Class（8-bit）：流量类型。标识包装在数据报中数据类型。
- Flow Label（20-bit）：流标签。指定流标签。
- Payload Length（16-bit）：数据负载长度。确定数据（在头部后面的数据报部分）的长度。
- Next header（8-bit）：下一个头部。定义紧跟在当前头部后的头部类型。
- Hop Limit（8-bit）：路由跳限制。暗示数据报允许的剩余多少跳（hop）数。没经过一跳，这个值减一。如果这个值到达0,则丢弃数据报。
- Source Address（128-bit）：数据报源地址。标识发送数据报的计算机的IP地址。
- Destination Address（128-bit）：数据报目的地址。标识接收数据报的计算机的IP地址。

IPv6在主要头部和数据之间的单独的扩展头部中提供一组可选信息。这些扩展头部提供特殊情况时的信息的同时允许主要头部仍然很小并且容易管理。

IPv6规范定义如下扩展头部：

- Hop-by-Hop 选项
- Destination 选项
- Routing
- Fragment
- Authentication
- Encrypted Security Payload

每个头部类型与一个8-bit标识符关联。在主要头部或在扩展头部中的“Next Header”定义链中下一个头部的标识符。

![https://i.quantuminit.com/90d3a419361c4cd5.svg](https://i.quantuminit.com/90d3a419361c4cd5.svg)

上面的列表中描述的扩展头部中，只有Hop-by-Hop Options 和 Routing被传输路径上中间结点处理。路由器没必要处理其他扩展头部；他们只需要传递就可以了。

#### Hop-by-Hop 选项头部

Hop-by-Hop选项头部的目的是为传输路径上的路由器关联可选的信息。被包含在规范中规范中的Hop-by-Hop选项头部在很大程度是为了给工业上开发未来选项提供的一种格式和机制。

规范含有一个选项类型命名和一些用于对齐数据的对齐选项。在规范中被明显定义的一个选项是**jumbo payload**，该选项被用来传输长度大于65535字节的数据负载。

#### Destination 选项头部

Destination 选项头部用于将可选信息和目的结点关联起来。像Hop-by-Hop选项一样，这个选项头部最初作为开发未来选项的一个框架而包含进来。

#### Routing 头部

Routing头部被用于指定一个或多个在去往目的地路上数据报路由经过的路由器。

路由头部格式图：

![https://i.quantuminit.com/d35c7ad5ad29414e.svg](https://i.quantuminit.com/d35c7ad5ad29414e.svg)

数据字段说明：

- Next Header： 标识跟着这个头部的下一个头部的头部类型。
- Header Length（8-bit）：按字节指定头部的长度（不包含下一个头部字段）。
- Routing Type（8-bit）：标识路由头部类型。不同的路由头部类型被设计用于特定的情况。
- Segment Left：在目的地之前表明明确定义的路由段的数量。
- Type-Specific Data：给路由类型字段中给定的特定的路由类型标识数据字段。

##### Fragment Header

沿着消息路径上的每个路由器都有一个用于设置最大传输单元（maxium transmission unit，MTU）。MTU设置表明路由器能够传输的数据的最大单元。IPv6中，源节点能够发现**path MTU**——传输路径中任何设备的最小MTU设置。path MTU表示数据能够通过路径传输的最大单元。如果数据报的大小大于这个最大单元，数据报被分割成能够通过传输的更小的块。这个Fragment 头部含有 用于重组分片数据报的必要信息。

##### Authentication Header

验证头部提供安全和验证信息。验证信息提供一种用于决定数据报在传输的过程中是否被修改的方式。

##### Encrypted Security Payload Header

加密安全负载头部（Encrypted Security Payload header，ESP）提供加密和保密性。使用IPv6的ESP能力，一些或所有被传输的数据能够被加密。使用tunnel-mode ESP（为VPN 隧道所设置），一个被加密的完整的IP数据报被放置在一个外部的，未加密的数据报中。在传输模式中，只有数据负载（payload）以及ESP尾部被加密。

## IPv6 地址分配

|Address Type|Binary Prefix|IPv6 Notation|Description|
|:-----|:-----|:-----|:-----|
|Unspecified|0....00(all 0s)|::/128|禁止被分配。表明不存在该地址|
|Loppback|0...01(127 0s)|::1/128|主机用于给自己发送一个包的诊断地址|
|Mapped IPv4|0...0:FFFF(80 0s then 16 1s)|::FFFF/96|等价于现有的IPv4的IPv6地址|
|Multicast|11111111|FF00::/8|标识一组主机|
|Linklocal unicast|1111111010|FE80::/10|用于自动地址配置|
|Global unicast|(Everything else)|||

无论怎么表示IPv6地址，想要128-bit 的IPv6地址都挺伤脑筋的。一个IPv6地址一般用四个由8个冒号分隔开来的十六进制数字组成，0开头可以省略：

2001:DB8:0:0:8:800:200C:417A

一个小小的技巧是忽略多个连续0的块并且用一个双冒号替换它们。则前面的地址可以如下表示：

2001:DB8::8:800:200C:417A

在任何IPv6地址中只允许存在一个双冒号。例如，地址FF01:0:0:0:0:0:0:101可以简单地写成

FF01:101.

表示一个IPv6网络地址，比如一个60-bit的网络前缀20010DB80000CD3,可以像这样子写：2001:0DB8:0000:CD30:0000:0000:0000:0000/60 或者这样子 2001:0DB8:0:CD30::/60。

最后，IPv6网络配置软件将会允许用户定义一个默认的网络前缀，目的之在客户端手动配置时，将只需要考虑主机部分的地址即可。

## Subnetting

与IPv4一样，IPv6也使用CIDR风格的助记符来标记和网络地址关联的二进制位。一个128 bits 的IPv6
地址给网络和主机地址留下了大量的地址空间。在IPv6中，子网划分应该发生在头地址的前64 bits，剩下的64（或更多）可用作子网中的主机IDs的范围。
