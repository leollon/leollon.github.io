---
title: Configuration
date: 2019-09-05 17:38:04
categories: Sams-Teach-yourself-TCPIP-in-24-hours
tags: [notes, TCPIP, Networks]
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

即使如果你的计算机没有成功的配置一个静态或动态的IP地址，一些系统依然能够通过零配置技术来执行一类最基本的TCPIP网络。

## The Case for ServerSupplied IP Address

在TCPIP网络运行每台计算机必须要有一个IP地址。静态地址的局限：

- More configuration：每个客户端必须进行独立配置。地址空间或某些参数上的改变（比如DNS服务器地址）都意味着每个客户端必须重新独立配置。
- More addresses：每台计算机使用一个IP地址不管当前是否在网络上。
- Reduced flexibility：如果被分配到不同的子网，一台计算机必须手动重新配置。

为了解决这一局限，另一个IP地址系统使用DHCP来分配IP地址。DHCP从早期用于启动无盘计算机的**BOOTP**协议开发而来。无盘计算机随着它启动的时候通过网络接收一个完整的操作系统。

## What Is DHCP

DHCP是一个用于自动为计算机分配TCPIP配置参数的协议。一个DHCP服务器可以提供给一个DHCP客户端许多TCPIP设置，比如一个IP地址，一个子网掩码，以及一个DNS服务器的地址。

因为DHCP服务器负责分配IP地址，所以理论上只有DHCP服务器必须配置静态地址信息。对于用户工作站，笔记本以及其他网络客户端系统，对于客户端来说，唯一需要配置网络参数选项的是发送IP地址信息的DHCP服务器。剩下的TCPIP配置通过网络传输。这样子以来，如果网络上TCPIP配置某些方面发生改变，网络管理员仅仅只需要更新DHCP服务器，而不是手动更新每个客户端。

更重要的时候，每个客户端接收一个有租约时间限制的IP地址。DHCP租约特性的优点是网络没必要和含有的客户端一样需要很多的IP地址。

## How DHCP Works

当一个DHCP客户端计算机启动时，TCPIP软件载入内存中并开始运行。然而，由于TCPIP栈还没有IP地址，因此它不能够发送或接收定向的数据报。但是，计算机可以发送并且监听广播。通过广播通信的能力是DHCP能够工作的基本。从DHCP服务器租用一个IP地址过程的四个步骤：

1. DHCPDISCOVER：DHCP客户端通过广播一个发往端口67（BOOTP以及DHCP服务器使用）的数据报来初始化这个过程。第一个数据报又称为DHCP发现消息，它是一个请求任何DHCP服务器接收用于配置信息的数据报。DHCP发现数据报中含有许多字段，但是最重要的一个是含有DHCP客户端物理地址的字段。
2. DHCPOFFER：DHCP服务器为驻留在网络上的客户端计算机构造一个称为DHCP offer的响应数据报并且通过广播将数据报发到发起DHCP discover的计算机上。这个广播被发往UDP端口68并且含有DHCP客户端物理地址。同时包含在DHCP offer中的还有DHCP服务器的物理地址以及IP地址，分配给DHCP客户端的IP地址以及子网掩码。如果有不只一个能够给DHCP客户端提供IP地址的DHCP服务器，此时DHCP客户端有可能接收到几个DHCP offers。大多数情况下，DHCP客户端接收第一个到达的DHCP offer。
3. DHCPREQUEST：客户端选择一个offer并且构造以及广播DHCP请求数据报。DHCP请求数据报包含有能发出offer的服务器的IP地址以及DHCP客户端的地址。DHCP请求执行两个基本任务。第一告诉DHCP服务器客户端请求给DHCP客户端分配一个IP地址（以及其他配置设置）。第二，通知所有其他有更好的offer的DHCP服务器它们的offers不会被接受。
4. DHCPACK：当被选择的来自DHCP服务器的offer的服务器接受DHCP请求报文是，服务器构造最终的租用过程的数据报。这个数据报又称为**DHCP ack**（*acknowledgment的缩写)。DHCPack包含分配给DHCP客户端的一个IP地址以及子网掩码。可选地，DHCP客户端经常也被配置默认网关，几个DNS服务器和可能一或两个WINS服务器的IP地址。除了IP地址之外，DHCP客户端能够接收其他比如NetBIOS结点类型的信息，该信息可能改变BetBIOS名字解析的顺序。

包含在DHCPack中的三个其他关键的字段，每一个都暗示时间段，一个说明租约长度的字段，两个其他时间字段，又称为T1和T2，当客户端尝试重新更新租约时，将会使用到。

### Relay Agents

如果DHCP客户端和DHCP服务器在同一网络段上，这个过程处理和之前描述的一样。如果DHCP客户端和DHCP服务器在由一个或更多的路由分割开来的不同的网络中，这个过程会变得更复杂。路由器一般不转发广播到其他网络。为了使DHCP工作，必须有一个辅助DHCP过程的中间人。这个中间人可以是同一网络上另外一个作为DHCP客户端的主机，但是经常是路由器本身。在任何情况，执行这个中间人功能的过程被称为**BOOTP relay agent**或是**DHCP relay agent**。

一个relay agent被配置一个固定的IP地址并且知道DHCP服务器的IP地址。因为relay agent 已经配置了IP地址，它们总是能够发送并且接收给DHCP服务器的定向的数据报。因为relay agent与DHCP客户端处在同样的网络上，所以能够通过广播和DHCP客户端进行通信。

![https://i.cthee.cyou/08620e0f601c4ed0.svg](https://i.cthee.cyou/08620e0f601c4ed0.svg)

### DHCP Time Fields

DHCP客户端从DHCP服务器租用一个固定时间的IP地址。实际的租约时间长度一般在DHCP服务器上面定义。与DHCP ack消息发送的T1和T2时间的值在租约重新更新过程中用到。T1值暗示客户端什么时候开始重新更新租约过程。T1一般设置为实际租约时间的二分之一。更新租约的时候，DHCP request 和 ack两个数据报不是广播，而是作为定向的数据报发送。因为两台计算机此时都含有有效的IP地址，所以这是有可能的。

当DHCP客户端在实际租约时间二分之一时发起第一个更新租约请求，如果DHCP服务器不可用，客户端等待并在租约时间的4/3时进行尝试更新租约。如果请求也还是失败，DHCP客户端在7/8的租约时间时第三次进行尝试更新。如果DHCP客户端不能在总的租约时间的87.5%更新它的租约，T2时间生效。T2时间允许DHCP客户端向任何DHCP服务器发起广播请求。如果DHCP客户端不能更新租约或在租约过期时从另外一个DHCP服务器获取到新的租约，客户端必须停止使用该IP地址和停止使用TCPIP用于普通的网络操作。

### DHCP Server Configuration

除非你是在中型或大型网络上的一个系统管理员，否则你不可能有机会配置一台计算来充当一个DHCP服务器。Windows提同一个图形界面的工具来配置DHCP服务器。Linux系统通过dhcpd（DHCP守护程序）提供DHCP服务。根据不同的厂商，安装dhcpd的命令也不一样。DHCP配置信息存储在/etc/dhcpd.conf配置文件中。

/etc/dhcpd.conf文件包含DHCP守护程序将分配给客户端的IP地址配置信息和可选的比如广播地址，域名，DNS服务器地址以及路由器地址等设置。

/etc/dhcpd.conf配置文件实例：

default-lease-time 600;
max-lease-time 7200;
option domain-name "macmillan.com";
option subnet-mask 255.255.255.0;
option broadcast-address 185.142.13.255;
subnet 185.142.13.0 netmask 255.255.255.0 {
range 185.142.13.10 185.142.13.50;
range 185.142.13.100 185.142.13.200;
}

正如前面文章提到的一样，DHCP服务经常通过比如路由器/防火墙系统等网络设备来提供。一个路由器设备提供一个网页DHCP配置接口。

![https://i.cthee.cyou/bc30a85b56d74bf5.svg](https://i.cthee.cyou/bc30a85b56d74bf5.svg)

一些路由器提供一个成为IP预留的特性，用来将一个IP地址和物理地址（MAC）关联起来。通过这一特性能够确保设备总是收到同样的IP地址。

## Network Address Translation

只要路由器本身有一个即用型的网络地址，它就能够充当网络上客户端代理——接收来自客户端的请求并且转换来自和去到互联网地址空间的请求。如今许多路由器/DHCP也执行一个称为网络地址转换（NAT）的服务。

NAT设备隐藏本地网络的所有细节，事实上，隐藏了本地网络的存在。

![https://i.cthee.cyou/681d766cc3954b41.svg](https://i.cthee.cyou/681d766cc3954b41.svg)

从[The-Internet-Layer](/The-Internet-Layer)中得知，预留了一小部分的IP地址用于“私有”网络：

- 10.0.0.0 到 10.255.255.255
- 172.16.0.0 到 172.16.255.255
- 192.168.0.0 到 192.168.255.255

后面也有不能路由的169.254.0.0 到 169.254.255.255地址范围，该地址范围用于配置链路本地（link-local）地址以及不是被NAT所使用。NAT设备一般分配一个来自私有地址范围内的IP地址。这些地址在传统意义上是不可路由的，所以到达NAT客户端计算机唯一的方式是通过地址转换过程。

## Zero Configuration

一些操作系统厂商探索出了用于让本地网络的计算机在没有配置静态IP地址或基于DHCP的动态IP地址配置的情况下进行连接。像NetBEUI（Windows 系统）和AppleTalk（Apple 网络）这两个早先的LAN协议提供开箱即用的无配置连接，并且供应商已经寻找到一种通过TCP / IP返回它的方法。

迈出这条路的第一步就是一个称为链路本地地址寻址（IPv4LL）。链路本地地址自从OS 9开始已经是Apple系统的一部分，并且从Windows 98开始，也已经包含在内了。微软称Windows版本的IPv4LL为自动私有IP寻址（Automatic Private IP Addressing，APIPA）。大部分的Windows版本含有一个用于关闭APIPA的注册表键。得通过查阅Window文档才能得知。

APIPA确实造成一些在问题排查过程中的问题。比如，如果网络中的其他计算机配置正常并且某一台不可达，那么得检查看是否这台计算机丢失了DHCP服务器的连接并且给它自己分配了一个不能与本地地址空间兼容的APIPA地址。

功能更强大以及完整的无配置环境的**Zeroconf**扩展了IPv4LL的原理来给小型本地网络提供一个大而全的网络环境的可能性。Apple Macintosh 系统中的**Bonjour**。Linx和UNIX系统中与Apple版本相似的Avahi。

zero-configuration环境中的三个重要的部分：

- **链路本地地址寻址**：计算机给它们分配一个在*169.254.0.0* 到 *169.254.255.255*私有地址范围内的IP地址。
- **多址通信DNS**：在没有一个服务器或是一个预配置的hosts文件情况下的DNS名字解析。通过查询一个指定的IP地址和端口，名字被解析为IP地址（和地址被解析成名字）。其他设备监听发送到这个地址的请求并且响应信息。
- **DNS服务发现**：客户端用于知道网络上有可用服务的一种方式。

这些部分的相互作用创造出了一个环境，在这个环境中，一台计算机能够没有任何TCPIP配置的情况下启动，而又能够接收到一个本地兼容的，不可路由的IP地址以及和网络上的其他计算机注册它的主机名并且通过像网络邻居那样的，点击式的文件浏览器的轻松访问方式来浏览可用的网络服务（比如文件与打印服务器）。

一台支持multicast DNS（mDNS）的计算机保存它自己的内部的DNS资源记录表并使用这张表来将名字解析到IP地址。

![https://i.cthee.cyou/4d19f42114644bf7.svg](https://i.cthee.cyou/4d19f42114644bf7.svg)

DNS服务发现（DNS-SD）为计算机以及设备提供一种通过DNS公布服务的方式。DNS-SD依赖于查询SRV资源记录，该记录标识域内提供的服务。例如，在传统的NDS网络上，一个域的一个SRV记录可能保存有一个FTP服务器或Active Directory 域控制器的主机名和端口号。DNS-SD扩展这一能力到更小的范围并且有策略地利用其他记录类型来完成这个过程。首先，DNS PTR指针记录（反向查询中使用到）的变体指向网络上运行着的可用的服务实例。查询结果可能返回如下信息：

- Instance：一个指定的服务实例。（网络上有可能有几个服务器提供同样的服务。）
- Service：服务的名字。（保存在www.dns-sd.org的DNS-SD服务类型的主要登记库。）
- Domain：该服务所在的域。

通过收集查询的响应，DNS-SD客户端创建出一个网络上可用的服务以及服务实例的浏览列表。

当用户或客户端应用程序选择浏览列表中的一个指定的服务实例的时候，一个关联的SRV记录的查询返回用于访问网络上的服务必要的主机名和端口号。DNS-SD还使用TXT资源记录来返回关于服务的额外信息。

DNS服务发现设计用于与multicast DNS一同工作来提供完整，零配置的DNS环境，但是DNS-SD也能够与具有最小预备的配置的传统的DNS服务一同工作。

微软定义了另一种称之为链路本地多址通信名字解析（LLMNR）。微软的简单服务发现协议（SSDP）提供服务发现。SSDP基于HTTP而不是传统的DNS，这与更加强调基于URL的服务的发展趋势相匹配，但是与传统的DNS基础设施之间存在一些不连续性。

提供类似与DNS-SD的通用即插即玩（Universal Plug and Play，UPnp）协议系统依赖于SSDP。

另一个称为Service Location Protocol（SLP）的服务发现选项在HP打印机和许多其他设备中有使用到。

## Configuring TCPIP

在/etc/network/interfaces文件中，eth0接口（第一块以太网网卡）的静态地址配置的如下：

```text
iface eth0 inet static
address 203.121.14.13
netmask 255.255.255.0
gateway 203.121.14.1
```

/etc/network/interfaces中一个网络接口被配置成DHCP的条目如下：

```text
auto eth0
iface eth0 inet dhcp
```

/etc/network/interfaces文件可以含有许多其定义配置的设置。不过得需要查看Linux文档。

Ubuntu无线网络问题排查[向导](https://help.ubuntu.com/community/WifiDocs/WirelessTroubleShootingGuide)以及[无线维基](http://wireless.wiki.kernel.org/)是了解Linux中关于无线的大概信息的好去处。

## 小小的总结

- DHCP客户端与DHCP服务器刚开始时通过广播以及接收广播来与彼此进行通信。
- 一个DHCP客户端从另外一个网络上的DHCP服务器租用一个IP地址需要一个DHCP中继代理，中继代理费负责发送和接收给DHCP服务器的定向的数据报，中继代理与DHCP客户端处在相同的网络上，可以通过广播与DHCP客户端进行通信。

[上一篇](/TCP-IP-Security)
[下一篇](/IPv6-Next-Generation)
