---
title: Classic Tools
date: 2019-09-16 17:42:59
categories: Sams-Teach-yourself-TCP/IP-in-24-hours
tags: [notes, TCP/IP, Networks]
---


阅读完本文之后，将可以：

- 标识并且描述常见的TCP/IP连接工具
- 使用连接工具排查问题
- 解释SSH和Telnet的目的
- 描述一些常见的网络管理协议

## 连接问题

具有代表性的前四个网络连接问题变化：

- 协议功能障碍或配置错误：协议软件不工作或（无论什么原因）未正确配置地运行在网络上。
- 线路问题：网线未插入或不正常工作。集线器，路由器或交换机未正常工作。
- 错误的名字解析：域名系统（DNS）或NetBIOS名字不能被解析。资源能够通过IP地址进行访问但是不能通过主机名或DNS名字（quantuminit.com）。
- 流量过大（Excessive traffic）：网络是工作状态，但是工作很慢。

下面分别讨论解决这些常见连接问题的工具及技术。

## Protocol Dysfunction and Misconfiguration
  
TCP/IP协议组提供许多有用的工具帮助确定TCP/IP是否功能正常并且正确配置，比如

- **ping**：该工具是初始化一个简单的网络连接测试的诊断工具并且能够报告其他计算机或网络设备是否响应。
- **Configuration information utilities**：每一个OS厂商提供某有种显示TCP/IP配置信息的工具并且可以用于检查是否IP地址，子网掩码，DNS服务器和其他参数是否正确配置。
- **arp**：这个工具可以用于查看并且配置地址解析协议（ARP）缓存表的内容，缓存表中将IP地址和物理（MAC）地址关联起来。

### Ping

`ping`工具能够初始化最小的网络连接测试。它发送一则消息给另外一台计算机，说"Are you there"并且等待其他计算机响应。

`ping`命令的基本形式：

```bash
ping IP_address  # IP_address是想要连接的那台计算机的地址。
```

ping工具的命令行选项根据实现和系统不同而不同。*ping*工具使用互联网控制消息协议（[ICMP](The-Internet-Layer/#Internet-Control-Message-Protocol)）`Echo Request`命令发送一则消息到接收计算机。如果接收计算机存在并且运行，它会使用`Echo Reply`消息进行响应。

ping是一个最小的应用。它只需要TCP/IP协议栈中的网络接入层，该层与OSI模型中的底部三层相关。ICMP是一个OSI的三层协议。可能问题发生在传输控制协议（TCP），用户数据报（UDP），或是更上层的应用层并且ping依然能够正常运行。

Ping提供许多特别有用的选项用于网络问题排查。可以这么做有如下步骤：

- 使用称为loopback地址（127.0.0.1）的特殊IP地址ping本地IP软件。如果命令`ping 127.0.0.1`成功，则TCP/IP协议软件能够正常运行。
- ping自己的IP地址。如果能够ping通分配给网络适配器（网卡）的IP地址，就可以知道网卡已经正确配置并且正常地与TCP/IP软件进行交互。
- 通过主机名来ping。大部分系统允许使用一个主机名替换ping命令中的IP地址。如果能够通过IP地址ping通一台计算机，但是不能通过它的主机名ping同样的计算机，就可以知道问题是和名字解析有关系。

在典型的网络问题排查场景中，一个网络管理员会执行如下*ping*命令：

1. ping loopback地址（127.0.0.1）来验证TCP/IP在本地计算机上能够正常工作。
2. ping本地IP地址来验证网络适配器处于运行状态并且配置了本地IP地址。
3. ping默认网关来验证计算机能够与本地子网通信并且验证网关的是处于运行状态的。
4. ping在默认网关外的一个地址来验证网关能够成功地转发超出本地网络段的数据包。
5. 通过主机名ping本地主机和远程主机来验证名字解析功能是正常的。

某些管理员更喜欢反方向按步骤执行这些ping命令。其目的都是一样的：隔离出通信失败的地方。所有的Unix和Linux系统，包括MacOS都含有`ping`命令，旧版本地Windows系统也使用ping。在Windows PowerShell环境中，*Test-NetConnection*命令等同于经典的ping。

ping发送ICMP*Echo Requests*并输出响应，偶尔丢失的数据报不能将其考虑为网络连接失败，因为ICMP不保证传输的可靠性。然而，丢失的响应暗示网络过于拥挤。一些ping工具版本用毫秒来表示从发送*Echo Request*消息到*Echo Reply*消息返回所用的时间。响应时间短表示数据报不必要经过太多的路由器或慢速的网络。如果ping命令返回一个接近于0的Time To Live（TTL），可能表示连接靠近TTL的阈值并且一些数据报丢失了或重新发送了。

### Configuration Information Utilities

UNIX和Linux系统使用`ifconfig`命令来显示地址信息。如果一台计算机含有两个网卡，就会有两个IP地址。ifconfig命令显示和每个网卡相关连的地址信息。

使用`ifconfig`显示IP地址信息：

```bash
ifconfig interface_name
```

interface_name是想要查看地址信息的网卡的名字。（在Unix和Linux中，每个网卡通过定义接口的配置文件分配一个名字并且通过那个名字进行引用。）例如，

```bash
ifconfig eth0  # 显示eth0当前的IP地址和子网掩码等等
```

使用ifconfig给网卡配置IP地址，子网掩码：

```bash
ifconfig eth0 <IP_address> netmask <netmask>
```

*ifconfig up*和*ifconfig down*选项能够用于启用和停用网卡。例如

```bash
ifconfig eth0 up
ifconfig eth0 down
```

使用`man ifconfig`查看更多的关于`ifconfig`更多可用选项。旧版本的Windows系统使用`ipconfig`。在PowerShell环境中，`Get-NetIPConfiguration`命令最接近等同于ipconfig。

更新的并且更加通用的`ip`工具逐渐替换`ifconfig`成为Unix/Linux系统上基本的TCP/IP配置工具。

### Address Resolution Protocol

在TCP/IP网络上每台主机维护一个ARP缓存表——一个用于关联IP地址到物理地址的表。使用`arp`命令查看本地计算机或另外一台计算机的ARP缓存表的内容。大部分情况下，协议软件负责更新ARP缓存表并且使用`arp`命令进行网络故障排障的情况很少。

可以使用`arp`命令来手动设置想要的物理/IP地址对。这个方法有助于减少网络上的流量（尽管在小型网络上没必要这么做）。ARP缓存表中的条目默认是动态变化的：条目自动添加到缓存中无论何时发送数据报并且一条当前条目不存在目的计算机的缓存表中。缓存条目一旦输入即开始过期。缓存条目可以通过执行另一台计算机或路由器的ping来添加。使用`arp`命令查看缓存条目：

- `**arp -a**`：用于查看所有的ARP缓存条目
- `**arp -g**`：用于查看所有的ARP缓存条目
- `**arp -a <IP_address>**`：如果有多张网卡，可以通过使用`arp -a`加上网卡接口的IP地址来查看和这个接口关联的ARP缓存条目
- `**arp -s**`：手动添加一个永久的静态条目到ARP缓存表中。此条目保持有效直到计算机重启并且如果使用手动配置物理地址时产生错误，条目将会被自动更新。例如，`arp -s 192.139。10.1 00-9f-ef-ac-23-79`。
- `**arp -d <IP_address>**`：用来手动删除一条静态条目。例如，`arp -d 192.139.10.1`。

## Line Problems

网络集线器或网线问题实际上不是TCP/IP问题。但是依然可以使用TCP/IP工具比如`ping`来诊断网络线路问题。通常情况下，如果网络过去都是能正常工作的并且突然停止工作，原因可能就是网路线路出故障了。大多数的网卡，集线器，交换机以及路由器都有暗示这个设备处于工作状态并且准备接收数据的显示灯。集线器，路由器或交换机的每个端口都有一个显示是否有活跃的网络连接通过这个端口处于运行状态的连接状态灯。

可以使用`ping`来隔离网络线路问题。如果一台计算机能够ping通自己的地址但是不能ping网络上任何其他地址，问题的可能存在在连接计算机到本地子网中的网络线路中。

## Name Resolution

名字解析问题发生在当发送一则消息给一台主机时主机名不能够在网络上被解析出来。事实上，大多数常见名字解析问题的共性是源计算机能够通过IP地址到达目标计算机，但是不能通过主机名到达目标计算机。作为实际问题，在今天互联网上的资源是通过主机名进行读取的，并且第一次尝试连接一台主机可能是通过名字。如果尝试失败，可以通过[ping](#Ping)的问题排查步骤来解决问题。如果仍然能够通过IP地址连接，那么可能问题出在了名字解析上面。

一些名字解析问题常见的原因：

- hosts文件丢失或不正确
- 名字服务器掉线或不可达
- 在客户端配置中名字服务器没有被正确地引用
- 尝试到达的主机在名字服务器中没有主机名条目
- 在命令中使用的主机名是不正确的

如果在一个使用名字解析的网络上正经历名字解析问题，通过ping名字服务器来确保它在线是一个很棒的做法。如果名字服务器在本地子网之外，通过ping网关来确保名字解析请求能够到达名字服务器。多次检查输入的名字确保它是资源的正确名字。如果这些方法都不能得出一个解决方案，则可能需要使用`nslookup`工具来查询关于指定主机名条目的名字服务器。[nslookup](/Name-Resolution/#Checking-Name-Resolution-with-NSLookup)工具工作模式。PowerShell中等同于`nslookup`的工具是`Resolve-DNSName`。使用`hostanme`查看当前工作计算机的主机名。

### Network Performance Problems

因为TCP/IP协议通常使用TTL设置限制网络上一个数据包的时间，网络性能低能够导致丢包并且因此丢失连接。即使为丢失连接，低效的网络性能可能是一个烦恼并且生产力下降的源头。造成糟糕的网络性能的通常原因是过多的流量。因为网络上有太多的计算机，所以可能网络中流量过于繁重，或者这个原因可能是一个有故障的设备比如一个网卡在网络上创建不必要的流量，在网络上又称为**广播风暴（broadcast storm）**。有时候，造成糟糕的网络的原因是一个宕机的路由器停止转发流量并在网络中某处产生了瓶颈。

#### Traceroute

`traceroute`工具用于跟踪当数据报从本地计算机通过多个网关到达目的地要经过的路径。如果要使用域名系统（DNS），经常可以从响应中确定城市的名字，区域以及通用的运输人。`traceroute`工具利用ICMP协议来定位每个处在客户端计算机和目的计算机之间的路由器。TTL的值告知一个数据包经过的路由器后网关的数量。通过操作在发送出去的ICMP*Echo*信息中的TTL的值，`traceroute`能够找到路径上的每个路由器，如下：

1. 一个ICMP*Echo*消息被发往目的IP地址，TTL的值设置为1。第一个路由器从TTL值中减去1,这造成TTL的值为0.
2. 因为TTL值现在被设置为0,路由器知道不应该做转发数据报的任何尝试并且简单地丢掉它。数据报的TTL值已经过时。路由器发送一个*ICMP TIme Exceeded-TTL Expired In Transit*消息回给客户端计算机。
3. 运行`traceroute`命令的客户端计算机显示这个路由器的名字，然后发送出另外一个含有值为2的TTL的ICMP *Echo*消息。
4. 第一个路由器从TTL值中减去1,并且如果可以，则沿着路径转发数据报到它的下一跳（next hop）。当数据报到达第二个路由器时，TTL值又再次减1,导致TTL的值为0。
5. 第二个路由器，像第一部中一样，简单地丢弃数据包并使用和第一个路由器使用同样的方式返回一个ICMP消息给发送端。
6. 随着`traceroute`不断增加TTL的值并且路由器不断减小这个值直到数据报最终到达它的目的地，这个过程都在在持续地进行。
7. 当目的计算机接受ICMP*Echo*消息时，它发送会一个*ICMP Echo Reply*消息。

除了定位数据报通过的每个路由器或网关，`traceroute`工具还记录到达买个路由器所用的round-trip时间（RTT）。根据实现方式的不同，`traceroute可能实际上发送多于一个*Echo*消息到路由器，目的是为了能够更好地判断round-trip时间。

不应该使用通过`traceroute`获得的round-trip时间值来明确地评判网络的性能。许多路由器会简单地给ICMP流量更低的优先权并消耗大量的处理时间来转发更多重要的数据报。

`traceroute`命令使用例子，命令后面跟IP地址，DNS名字或一个URL：

```bash
traceroute 193.138.240.91  # 跟IP地址
traceroute quantuminit.com
```

旧版本的Window系统使用`tracert`命令作为等同于`traceroute`的命令工具。在Powershell系统上，`TraceRoute`是一个`Test-NetConnection`命令的选项：

```cmd
test-NetConnection www.example.com -TraceRoute
```

### Route

大多数路由器使用特殊的路由协议来交换路由信息并且动态地有周期地更新它们的路由表。在路由器和计算机上可以手动多次地添加路由条目到路由表中。

`route`命令在TCP/IP网络中有许多用得到的地方。可以使用`route`来显示路由表中来自一台主机的数据包不能够被高效地路由的情况。如果`traceroute`命令显示出一个反常的或不高效的路径，可能需要`route`来确定为什么正在使用的那条路径并且可能得配置一条更高效的路由路径。

`route`命令也可以用于手动添加，删除并更改路由条中路由条目。Windows系统上route命令使用例子：

- `route print`：显示路由表中的当前的路由条目。

![https://i.quantuminit.com/7e3f5ca7c9b64dd6.png](https://i.quantuminit.com/7e3f5ca7c9b64dd6.png)

- `route add`添加一条新的路由条目到路由表中。例如，为了指定一个路由到目的网络202.34.17.0是5跳的距离并且首先传进一个带有在本地网络192.59.66.5上的一个IP地址的路由器并且子网掩码是255.255.255.224，输入如下命令：

```cmd
route add 207.34.17.0 mask 255.255.255.224 192.59.66.5 metric 5
```

手动添加的路由条目，在计算机或路由器重启之后会丢失。经常将一系列的`route add`命令放到一个开机启动脚本中，这样每次启动的时候，都能够使用这些路由。

- `route change`：更改路由表中的条目。下面的例子是更改数据路由到一个有更多更直接的three-hop路径到目的地的不同的路由器：

```cmd
route change 207.34.17.0 mask 255.255.255.224 192.59.66.7 metric 3
```

- `route delete`：从路由表中删除一个条目。

```cmd
route delete 207.34.17.0
```

### Netstat

`netstat`工具显示和IP，TCP，UDP以及ICMP协议相关的统计数据。统计数据显示比如发送的数据报，接收的数据报以及各种各样可能已经发生的错误。

计算机有时候会接收到的引发错误，丢弃或是失败的数据报。产生丢弃是因为数据报传输到了错误的位置。如果是一个路由器，当在路由的数据报中的TTL值到达0时，也会丢弃数据报。重组失败发生是在所有的分片数据基于接收的数据报中的TTL值一段时间内不能到达。

`netstat`命令选项描述：

- `netstat -s`：用于分协议显示每个协议的统计数据。
- `netstat -e`：用于显示关于以太网的统计数据。列出来的项目包括总共的字节，错误，丢弃数据，定向数据报的数量以及广播数量。这些统计数据都是发送和接收的数据报。
- `netstat -r`：用于显示与`route print`命令相似的路由表信息。除了活跃的路由之外，还显示当前活跃的连接。
- `netstat -a`：用于显示所有活跃连接，包括建立的连接和那些正在监听的连接请求。
- `netstat -n`：显示所有的活跃的连接
- `netstat -p TCP`：用于显示建立的TCP连接
- `netstat -p UDP`：用于显示建立的UDP连接

![https://i.quantuminit.com/0b1132c475d849a9.svg](https://i.quantuminit.com/0b1132c475d849a9.svg)

Windows PowerShell environments 使用等同于`netstat`的`Get-NetTCPConnection`命令。

## Telnet

**telnet**是一系列提供terminal-like访问一台远程计算机的组件。他曾经是最常见的用于达到使用命令行访问一台远程计算机的方式。然而最近几年，更加安全的SSH协议成为终端访问的标准。

Telnet会话需要一个充当远程终端的Telnet客户端和一个接收连接请求并且允许连接的Telnet服务器。

![https://i.quantuminit.com/ac16e131fdcb42c5.svg](https://i.quantuminit.com/ac16e131fdcb42c5.svg)

Telnet也是一个协议——一个定义在Telnet服务器和客户端之间交互的规则系统。

在Unix系统上，`telnet`命令像下面一样在命令提示符后面输入：

```bash
$ telnet hostname  # hostname是想要连接的计算机的名字。hostanme可以换成IP地址。启动telnet应用程序。
```

telnet也提供一些可以在Telnet会话中使用的特殊命令：

- close：用于关闭连接
- display： 用于显示连接设置，比如，终端模拟器的端口
- envriton：用于设置环境变量。由操作系统使用的环境变量来提供机器指定或用户指定的信息。
- logout：用于登出远程用户和关闭连接
- mode：用于在ACSII或二进制文件传输模式中进行切换。ASCII模式设计用于文本文件的高效传输。二进制模式用于其他的文件类型，比如可执行文件和图形图像。
- open：用于连接一台远程计算机
- quit：用于推出Telnet
- send：用于发送特殊的Telnet协议序列给远程计算机，比如一个终止序列，一个间断序列或一个文件结束（end-of-file）序列
- set：用于设置连接参数
- unset：用于复原连接参数
- ?：用于打印帮助信息

