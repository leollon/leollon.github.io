---
title: Name Resolution
date: 2019-08-24 22:20:28
categories: Sams-Teach-yourself-TCP/IP-in-24-hours
tags: [notes, TCP/IP, Networks]
---

本文目的：

- 解释如何进行域名解析
- 解释主机名（hostname），域名和FQDNs的区别
- 描述主机名解析
- 描述DNS名字解析
- 描述在某些遗留微软网络上使用的NetBIOS名字解析

## What is Name Resolution

主机名系统是在TCP/IP历史上早期开发的简单的名字解析技术。系统中，每台计算机被分配给一个称为**主机名**的含有字母数字的名字。如果操作系统希望是一个IP地址的时候却碰上一个字母数字名字，则操作系统会查询一个**hosts文件**。hosts文件是一个包含主机名和IP地址关联的列表。如果字母数字名字在主机名列表上，计算机读取和名字相关联的IP地址。然后计算机使用相关的IP地址替换命令中主机名并且执行命令。

![93625dcb59ed467d.svg](https://i.quantuminit.com/93625dcb59ed467d.svg)

- 在每组特殊的域名服务器之间分发名字解析的责任。名字解析服务器维护定义了名字到地址关联的表。
- 授权本地名字解析权利给本地管理员。换句话说，就是不维护一份所有名字到地址关联表的中心化，主要版本，让网络A上个一个管理员负责网络A上的名字解析，以及让网络B上的管理员管理网络B上的名字解析。通过那样子的方式，每个单独的网络上的管理员负责网络上的任何更新也要负责确保那些更新反应到名字解析的基础设施。

以上两点优先级主导了域名系统（DNS）的开发。DNS将名字空间分割成称为域名的层次实体。域名可以和主机包含在一起，称之为主机域名全称（FQDN）。例如，主机名是www，且在域名quantuminit.com中，则构成的FQDN是www.quantuminit.com。

## Name Resolution Using Hosts Files

操作系统支持TCP/IP识别hosts文件并且在需要很少或者没有来自用户的干预下用它来进行名字解析。根据实现方式，配置主机名的细节也不相同。步骤大致如下：

1. 给每台计算机分配一个IP地址以及主机名
2. 创建一个映射IP地址到每台计算机的主机名的host文件。host文件经常命名为hosts，尽管有些实现使用的文件名为hosts.txt。
3. 将hosts文件放置到每台计算机设计好的目录中。根据不同的操作系统，目录的位置也不相同。

hosts文件包含有一台计算机需要想要与之通信的主机条目，允许输入和主机名，FQDN或是其他静态地别名相关的IP地址。当然里面还有一个条目用于[loopback 地址](/The-Internet-Layer/#Special-IP-Address)，即127.0.0.1。这个loopbakc地址用于给TCP/IP诊断并且代表当前这台计算机。

一个hosts文件示例：

![c18b44dc38ee4716.svg](https://i.quantuminit.com/c18b44dc38ee4716.svg)

当一台计算机上的一个应用需要解析一个名字成一个IP地址的时候，系统首先比较它自己的名字和被请求的名字。如果不匹配，系统将会进入hosts文件中查找（如果存在）是否有该计算机名在里面。如果能够匹配到一个，返回这个IP地址到本地计算机。通过使用IP以及ARP传输技术两台计算机之间产生通信。

创建或编辑hosts文件时的一些要点：

- IP地址必须左对齐并且使用一个或多个空格将它与主机名分隔开
- 名字必须被至少一个空格分隔开
- 在单行上多余的名字将会是第一个名字的别名。
- 文件从上到下被解析（计算机读取）。使用第一个和IP地址关联的匹配。当匹配成功时，则停止解析。
- 因为是从上到下进行解析，所以应该将常用的名字放置在上方。这样子可以加速解析过程。
- 使用*#*开头后面跟文本来表示注释
- 记住hosts文件是静态的；当IP地址发生更改时，必须手动更改hosts文件。
- 尽管FQDNs被允许以及在存在hosts文件中，但是不鼓励在hosts文件这么使用它们并且导致管理员在诊断时造成问题。控制hosts文件的管理员不能控制远程网络上的地址的分配以及主机名。

## DNS Name Resolution

DNS 存储名字解析在一台或多台特殊的服务器中。DNS服务器为网络提供名字解析服务。

![818cd5af384348c7.svg](https://i.quantuminit.com/818cd5af384348c7.svg)

上图中的DNS服务器的优点之一，为本地网络以及提供单一的DNS配置点以及高校的利用网络资源。然而并未为更大型网络基础设施的提供去中心化管理，以及不能够有效地运行一个含有网络上每个主机的IP地址记录的数据库。即使可以，但是在单台DNS服务器上维护所有互联网上的数据库是禁止的。

更好的方式是然每个办公室或学术机构配置运行一台本地的名字服务器，然后为所有的名字服务器提供能够与彼此之间进行通信的方式。当某个DNS客户端发送名字解析请求到名字解析服务器的时候，名字服务器按如下的方式之一进行工作：

- 如果名字服务器能够在自己的地址数据库中找到请求的地址，则立即发送这个地址给客户端。
- 如果服务器不能在自己的数据库记录中找到地址，则向其他名字服务器查找该地址然后发送该地址给客户端。

![35d10c4a3a6b449c.svg](https://i.quantuminit.com/35d10c4a3a6b449c.svg)

DNS不是严格地与主机名协作，而是与FADNs协作。在DNS树的顶部是一个称为根的单个结点。根有时候又称为句点（.)，尽管根的实际符号是null字符。根的下面是一组称为顶级域名（TLDs）的的域名。

![https://i.quantuminit.com/79848b7aecf24386.svg](https://i.quantuminit.com/79848b7aecf24386.svg)

总的来说，DNS系统支持高达127层域名分级。

![https://i.quantuminit.com/8e38575d227848b7.svg](https://i.quantuminit.com/8e38575d227848b7.svg)

当网络上的一台主机需要一个IP地址的时候，它通常发送一个递归查询到邻近的名字服务器。这个查询告诉名字服务器，要么给出和这个名字相关联的IP地址，否则返回响应表示不能找到。名字查询过程中使用递归查询（主机发送）以及迭代查询（名字服务器发送）。总结出查询的过程就是，客户端发送一个单独的递归查询该名字服务器，然后名字服务器发送一系列的迭代查询到其他名字服务器来解析这个名字。当名字服务器获取到与这个名字相关联的地址时，则使用这个地址响应客户端的查询。

DNS名字解析过程：

1. 主机1发送一个递归查询给名字服务器A询问和域名c.b.a.edu相关联的IP地址。
2. 名字服务器A检查自己的记录是否有这个请求的地址。如果名字服务器A有这个地址，则返回该地址给主机1。
3. 如果名字服务器A没有这个地址。由A初始查找地址的过程。名字服务器A为了获取该地址发送一个迭代请求到名字服务器B，.edu域名的顶级名字服务器询问和c.b.a.edu相关联的地址。
4. 名字服务器B不能提供地址，但是能够发送名字服务器C的地址给名字服务器A，即a.edu的名字服务器。
5. 名字服务器A发送查询该地址的请求给名字服务器C。名字服务器C不能够提供这个地址，但是能够发送名字服务器D的地址，就b.a.edu的名字服务器。
6. 名字服务器A发送获取该IP地址的请求到名字服务器D。名字服务器D查询主机c.b.a.edu的地址并且发送这个地址给名字服务器A。然后名字服务器A发送这个地址给主机1。
7. 主机1初始化一个连接到主机c.b.a.edu的连接。

![https://i.quantuminit.com/7fd227e581c7494f.svg](https://i.quantuminit.com/7fd227e581c7494f.svg)

## Registering a Domain

ICANN有完整域名注册任务的授权但是委托特定的顶级域名的注册到其他组织。VeriSign当前负责维护DNS根区域。其他组织维护顶级域名的系统。例如：

- **.com, org 以及 .net**：许多称为登记处的公司被授权为受欢迎的.com，.org和.net顶级域名提供名字解析服务，还有很少被人们所知的域，例如.info，.museum，.name以及.pro。VeriSign负责授权登记.com和.net域，以及公共互联网注册(PIR) 维护.org。
- **.gov**：给US联邦政府所保留的域。
- National domains：可能你已经注意到许多国家都有它们自己的域名空间。比如.uk，.fr，.de，.ca，.us等国家代码顶级域名（ccTLDs）。

## Name Server Types

在网络上实现DNS，至少需要一个主服务器负责维护域名。从本地文件中获取负责区域的所有信息的主名字服务器，从主名字服务器的**区域文件**获取信息并响应服务请求的备份名字服务器（又称第二名字服务器），响应来自本地网络上的客户端的域名解析请求查询的只缓存服务器。只缓存服务器向其他DNS服务器查询域名信息以及提供比如Web和FTP服务的计算机。当收到来自其他DNS服务器的信息是，将那些信息存储在缓存中以防万一再次请求这些信息。只缓存服务器被本地网络上的客户端计算用来解析名字。互联网上的其他DNS服务器不会知道这些只缓存服务器并且因此不会查询它们。

最受欢迎的DNS实现是BIND，Berkeley Internet Name Domain。

### Domains and Zones

在含有一组通用的DNS服务器的集合配置中的一组DNS主机称为**区域（zone）**。简单网络上，一个区域可能代表了一个完整的DNS域。更复杂的网络上，子域的DNS配置有时候委托到服务这个子域的另一个区域。区域委托让管理员具备更快速地知道管理一个子网络管理那个子网络的DNS配置。
区域（zone）以及域（domain）的区别不仅体现在精细的语义上（域是命名空间的细分并且区域是一个主机集合），区域和域的概念不是平行的。在读这部分内容事，要记住：

- 在子域中的成员资格暗示了在父域中的成员资格。例如，在b.a.edu中的一台主机是a.edu的一部分。相反，如果b.a.edu的域是被委托的，则在b.a.edu的主机不是a.edu区域中的一部分。
- 如果子域不是被特别地委托，则不需要一个单独的区域并且简单地与父域的区域文件包含在一起。

一个区域代表一组DNS服务器以及主机的集合配置，并且DNS管理员为了提高管理效率可选择性地委托部分名字空间到其他区域。

### Zone Files

一个DNS区域是一个表示存在DNS命名空间中的一个计算机集合的管理单元。一个区域的DNS配置被存在一个区域文件中。当响应查询以及初始请求的时候，DNS服务器查询在区域文件中的信息。一个区域文件是一个具有标准化结构的文本文档。区域文件由多个**资源记录**构成。一些资源记录的常见类型：

- SOA：SOA表示Start of Authority。SOA记录指明了这个区域的权威的名字服务器。
- NS：NS表示Name Server。NS记录指明了这个区域的一个名字服务器。一个区域可能含有几个名字服务器（因此，有多个NS记录）但是只能有一个SOA记录用于权威的名字服务器。
- A：一条A记录将一个DNS名字映射到一个IPv4地址。
- AAAA：一条AAAA记录将一个DNS名字映射到一个IPv6地址。
- PTR：一条PTR记录将一个IP地址映射到一个DNS名字。
- CNAME：CNAME是canonical name的缩写。一条CNAME记录映射一个别名到由一条A记录表示的实际的主机名。

因此，这个区域文件告诉DNS服务器一下信息：

- 这个区域的权威的名字服务器。
- 在这个区域的DNS服务器（权威的以及非权威的）。
- 在区域内的主机的别名（备用名字）之内的DNS-name-to-IP-address映射。

一个区域文件示例：
![https://i.quantuminit.com/8b03b3e9906345d6.svg](https://i.quantuminit.com/8b03b3e9906345d6.svg)

SOA记录中包含有几个使用主DNS服务器上的原版区域文件更新备份DNS服务器的参数。一个serial number表示区域文件自己的版本号，其他参数：

- Refresh time：备份DNS服务器查询主DNS服务器来更新区域文件的时间间隔。
- Retry time：区域文件未成功更新，再次尝试更新之前需等待的时间。
- Expiration time：备份DNS服务器无更新再获取一条记录的上限时间。
- Minimum Time to Live (TTL)：导出的区域记录的默认TTL。

### The Reverse Lookup Zone File

DNS名字解析必需的另一种类型的区域文件是反向查询文件。当一个客户端提供一个IP地址并且请求相关的主机名时，将使用到这个文件。在IP地址中，最左边部分是普遍的，最右边部分是特殊的。然而在域名中确实相反的，左边部分是特殊的，右边部分，比如，.com或.edu都普遍的。创建一个反向查询区域文件，必须将网络地址顺序进行逆序，这样普遍的以及特殊的部分会遵循在域名中使用的模式。例如，192.59.66.0网络的区域会有是66.59.192.in-addr.arpa的名字。*in-addr*部分表示反向地址，*arpa*部分表示另一个顶级域名并且是来自发生互联网之前的原本的ARPAnet的预留部分。这个文件与普通的区域文件一样含有SOA以及为区域定义的名字服务器的NS记录，但是反向查询区域文件包含将地址映射到名字的PTR记录，而不是将域名映射到地址的A记录。只有主机地址部分是被包含在地址映射中——网络部分从文件名字中获取：

```text
; zone file for 23.21.181.in-addr.arpa
@ IN SOA boris.cocacola.com. hostmaster.cocacola.com. (
     201.9 ; serial number incremented with each
           ; file update
           ;
     3600 ; refresh time (in seconds)
     1800 ; retry time (in seconds)
     4000000 ; expiration time (in weeks)
     3600) ; minimum TTL
IN NS horace.cocacola.com.
IN NS boris.cocacola.com.
;
; IP address to host mappings
;
4 IN PTR chuck
5 IN PTR amy
```

起初计划用于IPv6反向查询区域文件是以*ip6.int*结尾，但是互联网当前正向*ip6.arpa*过渡。因此可能碰上两中形式之一。和IPv4一样，网络地址部分有文件名反映出来（逆序的）；主机ID是文件中的一个条目（也是逆序的）并且被映射到一个主机名。

### DNS Security Extension （DNSSEC）

拦截DNS查询的攻击人可以发送一个虚假的响应，将客户端重定向到一个能够提供发起攻击方式的秘密的DNS服务器。只要虚假的行应在真实响应到达之前，DNS客户端一点也不聪慧的。

可以通过验证返回DNS数据的源头的方式来解决这个问题。DNS Security Extensions（DNSSEC）为验证DNS数据提供了这个系统。DNSSEC通过一组加密密钥和数字签名来实现。DNSSEC需要定义在RFC 2671中的Extension Mechanisms for DNS（EDNS）的支持。EDNS DO头部位标志一个DNSSEC查询。DNSSEC添加一个验证有效的过程来确保DNS查询的结果是真实可靠的。

为了达到安全验证的目的，DNSSEC添加四个新的DNS资源记录类型：

- DNSKEY：用于签名以及鉴定DNS资源记录集合的公钥。
- DS：指向（并且验证）一个子区域的DNSKEY的资源记录。
- RRSIG：和区域数据关联的数字签名。
- NSEC：下一个权威数据的拥有人的名字。

服务器拥有构成信任链的安全的DNS数据。解析方必须能够独立访问和用于顶层区域的一条DNSKEY记录关联的公钥。这个密钥是分开获取的。公钥鉴定存储在信任点上的数据，包括一条验证以及鉴定与在下一个查找过程步骤相关联的子区域的DS记录。解析方遍历信任链，通过低层级子区域，使用在父区域上的DS key验证子区域上的DNSKEY。最后一步，DNSKEY解密存储在RRSIG资源记录内的数据签名并且将其和由普通的DNS查询过程返回的签名进行比较。如果这个过程成功完成，则用于DNS查询的源得到了验证，并且数据被认为是真实的。

保存在初始入口点的DNS数据包括用于任何子区域的DS记录。例如，.com区域的官方服务器包含了一条famousIT.com的DS记录。DS记录识别以及验证用于子区域的DNSKEY。

![https://i.quantuminit.com/2f3f823b39164d9c.svg](https://i.quantuminit.com/2f3f823b39164d9c.svg)

DNSSEC依赖于在DNSKEY以及DS资源记录之间的一连串交互。父区域可能含有用于多个子区域的DS记录，以及每个DS记录提共必要的信息来验证在子区域中相关的DNSKEY记录是正确的并且表示一个服务器在信任链内。

另一个必要的要素是RRSIG记录包含用于区域数据的签名。为了给一个区域签名，这个区域的管理员生成一个或多个公共/私有的密钥对并且使用密钥为区域内的权威数据进行签名。对于每一个用于在一个区域中创建RRSIG记录的私钥，这个区域应该包含一个含有相关公钥的区域DNSKEY。

RRSIG记录含有比如拥有人名字，一个分类值（a class value），一个TTL值，包含数据的区域的名字以及其他识别记录的数据的信息。

万一名字错误或是当一个用于查找名字的实际匹配不可用时，则使用NSEC记录。

### DNS工具

如果使用资源的IP地址可以连接到资源，但是不能通过一个主机名或是FQDN连接这个资源，有可能的问题是名字解析问题。

#### Checking Name Resolution with Ping

简单且好用的`ping`是一个用于测试DNS配置的工具。

通过IP地址：

```shell
ping 198.1.14.2  # 通过IP地址给这台计算机发送信号，如果命令成功，表示可以通过IP地址连接到远程计算机
```

通过DNS名字：

```shell
ping quantuminit.com
```

#### Checking Nmae Resolution with NSLookup

NSLook工具用处：

- 查询DNS服务器以及查看比如资源记录等信息
- DNS问题排查

运行模式：

- Batch模式：在Batch模式中，运行NSLookup并提供输入参数。NSLookup使用输入的参数执行请求的功能，显示结果然后结束。
- Interactive模式：在Interactive模式中，不提供输入参数而直接运行NSLookup，然后NSLookup提示输入参数。当输入输入参数时，NSLookup执行请求的操作，显示结果，再次提示输入参数。

![https://i.quantuminit.com/6ad2ac608ebb448e.svg](https://i.quantuminit.com/6ad2ac608ebb448e.svg)

nslookup的interactive模式中可用的命令：

- host [server]：使用当前默认的服务器或使用提供的指定的服务器。
- server domain/lserver domain：更改默认服务器到domain。**lserver**使用初始服务器查询关于domain的信息，而**server**使用当前的默认服务器。
- set all：打印当前频繁用于**set**的选项值。包括当前使用的默认服务器和主机信息。

使用`man nslookup`查看更多信息。要注意的是大部分商业DNS服务器以及跟服务器拒绝`ls`命令，因为它们可以生成巨大的流量并且可能引发安全漏洞。

#### Domain Information Grouper (Dig)

最基本的用法：

```bash
dig quantuminit.com
```

在指定DNS服务器进行查询：

```bash
dig @8.8.8.8 quantuminit.com
```

查询指定的资源记录类型：

```bash
dig blog.quantuminit.com NS # 显示与域名相关连的NS记录。
```

查找邮件服务器：

```bash
dig quantuminit.com MX
```

当指定IP地址时，`-x`选项执行反向查询。`-4`选项限制查询为IPv4。`-6`用于IPv6。

## Dynamic DNS

每次计算机启动时，IP地址通过动态主机配置协议（DHCP）进行配置。企业目录系统比如微软的Active Directory使用动态DNS来管理在目录结构内的DHCP客户系统。

主机从一个DHCP服务器获取一个IP地址，并用这个IP地址更新DNS服务器。

![https://i.quantuminit.com/110b21ae66a9498b.svg](https://i.quantuminit.com/110b21ae66a9498b.svg)

## NetBIOS Name Resolution

**NetBIOS**是一个应用编程接口（API）并且最初由IBM开发的名字解析系统成为了微软Windows网络演化起到了重要作用。旧版的Windows系统，NetBIOS名字是分配给Windows计算机的计算机名字。
NetBIOS过去是为那些不使用TCP/IP的网络而开发的。NetBIOS不仅仅是在所有的Windows上有，流行的开源Samba文件服务以及其他独立工具也支持NetBIOS名字解析。

因为NetBIOS通过广播运行，所以在小型网络上的一个用户不需要做任何事情来配置NetBIOS名字解析（除了设置网络以及分配一个计算机名字）。大型网络上，可以通过使用称为Windows互联网名字服务（Windows Internet Name Service，WINS）服务器来解析NetBIOS名字成IP地址。也可以配置一个静态的**LMHosts**文件（和DNS下的hosts文件相似）用于名字解析查询。

NetBIOS名字是单个长度限制为15个字符的名字。NetBIOS不允许网络上有相同的计算机名。技术上来说，一个NetBIOS名字包含16个字符。然后第十六个字符被底层应用所有使用并且一般来说不能由用户直接配置。

## 小小的总结

- 域名是一个用于识别一个网络的名字。主机名是分配给特别的主机并且映射到一个IP地址的单独的名字。
- DNS资源记录是包含在一个DNS区域文件中的条目。不同的资源记录用来识别不同的计算机或服务的类型。
- 处于DDNS 网络中的计算向DHCP获取一个IP地址，再将这个IP地址发送给一个DNS服务器用于更新。
- CNAME资源记录用于给一个A记录进行别名。
- DNSSEC使用一条存储在父区域的DS资源记录来识别和鉴定存储在子区域的DNSKEY资源记录。为了有必要验证查询响应的真实性，保存在父区域的DS记录允许查询遍历信任链。DS资源记录指向（以及验证）子区域的DNSKEY。
