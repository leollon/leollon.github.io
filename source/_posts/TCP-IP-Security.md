---
title: TCPIP Security
date: 2019-08-29 19:02:22
categories: Sams-Teach-yourself-TCPIP-in-24-hours
tags: [notes, TCPIP, Networks]
---

本文目的在于：

- 解释防火墙如何工作
- 描述代理服务器以及反向代理服务器
- 讨论一些常见的网络攻击技术以及如何应对它们

## What Is a Firewall

防火墙是一个放置在网络路径中的一个设备，使用这样一种方式，它可以测试，接收，或者拒绝前往这个网络的入站数据包。这可能听起来像是一个路由器；事实上，一个防火墙不必是一个路由器，但是防火墙功能经常是内建到路由器中。重要的不同点是一个传统的路由当它能够转发的时候才转发——一个防火墙当它想要转发包时候才转发。转发的决定不仅仅是单独地根据地址还有网络拥有者配置的规则而不管网络上允许的是什么类型的流量。

早期的防火墙是**包过滤器**。它们检测包来获取关于目的地的线索。许多包过滤器防火墙监视编码在传输层头部中的已知的TCP以及UDP端口号。在防火墙的发展过程中，出现了所谓的状态防火钱个。一个**状态防火墙**不仅仅是独立的检测每个包，还知道包在通信会话序列中属于哪个地方。状态的敏感性有助于状态防火墙小心比如无效包，会话劫持尝试，以及确定拒绝服务攻击。

现代防火墙经常有结合地执行包过滤，状态检测以及应用层过滤。

### Firewall Options

个人防火墙是一个好主意，尤其是那些没有运行在任何形式防火墙背后的系统。在下一个复杂性的级别是防火墙/罗尤其设备哟关于小公司/总公司（SOHO）网络。这种工作会提供动态主机配置协议服务以及网络地址翻译（NAT）。
伴随SOHO防火墙的一个问题是它们不是被设计用于由非专家操作的，所以它们提供很少的配置选项，并且经常不清楚他们使用什么技术来过滤协议流量。另一个问题是防火墙自己就像是一台携带有对于用户来说不是很方便（有时候是不可能）直接访问的操作系统的计算机。另一个选项是使用一个计算机作为一个防火墙/路由器设备来配置一个网络防火墙。还可以使用专业级的防火墙/路由器。

### The DMZ

防火墙提供一个从外部很难访问到内部网络的受保护空间。然而在很多情况下，某个组织可能不想阻止所有资源不能够从外部访问。一个很简单的方式是将可从外部网络访问的服务放到防火墙外。

在小型网络上常见的网络布置：

![https://i.cthee.cyou/77a7c84811714503.svg](https://i.cthee.cyou/77a7c84811714503.svg)

在大型网络中，可以在前面这张图中，再在服务器前面加上一个防火墙。在两道防火墙之间的区域通常又称为**DMZ**（军事术语*demilitarized zone*）。

![https://i.cthee.cyou/064bdb15a7d54a91.svg](https://i.cthee.cyou/064bdb15a7d54a91.svg)

如果防火墙/路由器有三个或更多的接口，它能够连接两个内部网络并且通过单独的接口DMZ
![https://i.cthee.cyou/b5e804c3df8e4d1e.svg](https://i.cthee.cyou/b5e804c3df8e4d1e.svg)

### Firewall Rules

工业级的防火墙工具可以让用户创建一个含有用一系列的命令或定义防火墙行为的防火墙配置文件。这些命令或规则又称为防火墙规则。防火墙规则让网络管理员创建由如下信息构成的关联：

- 源地址或地址范围
- 目的地址范围
- 服务（Telnet，FTP...)
- 行为动作（deny，accept...)

这些参数的组合比起通过端口简单地打开或关闭服务要更灵活。

### Proxy Service

一个**代理服务器**拦截互联网资源的请求并且代表客户端转发请求，充当客户端和请求的目的服务器之间的中间人。虽然不能充分地保护网络，但也经常与防火墙结合在一起使用（尤其是在网络地址翻译（NAT）的环境中）。

![https://i.cthee.cyou/2ebb07f2b6d246fb.svg](https://i.cthee.cyou/2ebb07f2b6d246fb.svg)

很多情况下，代理服务器的最初目的出于性能的考虑而不是安全。代理服务器经常执行一种称为内容缓存的服务。一个内容缓存代理服务器存储它访问的网页的拷贝。代理服务器通常配置为在释放缓存以及请求新的页面之前保存页面一段特定的时间。

### Reverse Proxy

常规的代理服务器充当出去的网络请求的代理。另一种称为反向代理的代理服务器接收来自外部来源的请求并且转发到内部网络。反向代理服务器也提供和常规代理服务器一样的缓存以及内容过滤特性。反向代理隐藏实际完成客户端请求的计算机的细节。反向代理通过缓存大文件或访问页面也能提高性能。反向代理有时候作为一种负载均衡的形式而使用。例如，反向代理接收单个web地址下的请求，然后分发工作负载到上游服务器。

## Attack Techniques

三大类计算机攻击人：

- Adolescent amateurs：仅仅是随便玩玩的孩子。所谓的**脚本小子**经常只有基本的计算机系统知识并且刚开始应用互联网上可用的恶意脚本以及技术。
- Recreational intruders：这一类成熟的攻击人具有广泛的动机。其中的大多数人是为了挑战智商。
- Professionals：这一类危险的攻击人由知道大部分计算机知识的有经验的专家构成。因为他们知道欺骗手段，所以他们很难被追踪到。事实上，他们创造了一些欺骗手段。

### What Do Intruders Want

计算机攻击并且渗透过程是围绕如下步骤组织：

1. 访问系统
2. 获取权限
3. 隐藏
4. 为下一次攻击做准备

攻击人有许多用于获取入口并且隐藏的方法，而且尽管不可能全部描述完所有，但是还是有可能将那些技术分成三种基本类型：

- Credential attacks：这些攻击集中在获取登录凭据来登录系统。必要的时候，甚至在入侵人渗透安全系统之前，先产生攻击。这种技术的变化就是提权，在此情况下，攻击人获取低等级的访问权限，然后通过努力一番，进而获取到更高的权限等级。
- Network-level attacks：攻击人通过找到的开放端口，不安全的服务或是防火墙中的缺口秘密地登录进入。其他网络级别的攻击技术利用TCPIP协议系统的细微差别来获取信息或重新路由连接。
- Application-level attacks：攻击人利用运行在系统上的程序代码中的漏洞，比如web服务器，欺骗应用程序执行任意的命令或是进入某种方式上程序员从未希望发生的行为。

一个全面的网络入侵经常组合使用这些攻击技术。通常情况下，攻击人先使用应用级攻击作为突破口，然后提升权限到管理级别状态，接着安装一个隐藏后门用于无限地访问系统。**后门**是入侵者能够不受检测的登录到系统的一种方式。入侵人经常尝试安装*rootkit*来获取在系统中的立足点并且隐藏入侵。

#### Credential Attacks

访问一台计算机系统最古老的方式是找到密码并登录。找一个密码——任何密码——经常是攻破网络中的第一步。获取密码的方式从高级技术（密码破解字典脚本和解密程序）到低级技术（查找回收站与偷看用户的抽屉）。常见的密码破解方式：

- Looking outside the box
- Trojan horses（木马）
- Guessing（猜测）
- Intercepting（拦截）

##### Looking Outside the Box

密码泄漏的一个主要来源是用于的粗心大意。比如，在废弃的计算机打印资料，将密码告诉其他人或写到一个容易接触得到的地方。

##### Trojans Horses

计算机入侵者的常见的工具称为木马。一般来说，一个木马是一个只做一件事情的计算机程序，但是实际上在后台发生了不可见并且恶意的动作。

![https://i.cthee.cyou/7f675824fc664219.svg](https://i.cthee.cyou/7f675824fc664219.svg)

大多数操作系统有一个用来列出系统上运行的进程，比如Windows中的资源管理器和Linux中的`ps`命令。进程监视器有能力暴露出任何可能代表入侵者运行的秘密进程。有经验的入侵者可能尝试使用忽略任何与入侵者关联的进程并且正常表现的木马版本的进程监视程序替换内置的进程程序。

##### Guessing

某些密码太过于简单或是糟糕的构成以至于它们能被入侵者轻松地猜到。将密码的最小长度设置为6到8个字符，混合使用字母数字并且添加非字母数字字符到密码字符集合中。

##### Intercepting

包探测以及其他监控网络流量的工具能够轻松地捕获以明文（未加密）的形式通过网络传输的密码。在最近的加密技术开发中，比如Secure Sockets Layer(SSL) 以及 IP Security(IPsec)给想要窃听TCPIP来获取例如密码等敏感信息的入侵者提高了相当高的安全级别。

##### What to Do About Credential Attacks

一些导向性的准则：

1. 为组织中的用户提供一个清晰的密码策略。警告他们告诉密码给其他人，将密码写到桌面上的笔记本上以及将密码存储到文件中的危险性。
2. 配置所有的计算机来支持托管的密码策略。设置一个最短的秘密长度（至少六个字符，一些网络如今需要8个字符或更多）。所有的密码应该包含有数字以及字母的组合并且至少含有一个既不在开头也不在结尾的非字母数字字符。为了阻止密码猜测攻击，确保计算机在超过预先定义的登录次数之后能够关闭帐号。
3. 确保密码不以明文的形式通过公用通信线路进行传输。如果可能，在内网也不要以明文的方式进行传输密码，尤其是在大型的网络上。
4. 所有的帐号都不要使用相同的密码。

### Network-Level Attacks

扫描工具比如Nmap和Nessus能自动化查找端口的过程。在网络上运行的其他网络级的攻击策略是拦截并且破坏TCPIP流量。例如**会话（Session hijacking）**是一种利用TCPIP协议中的漏洞的高级技术。Session hijacking要求入侵者窃听TCP会话并且向是TCP会话一部分的流插入包。入侵者能够使用这个技术将命令加入到原始会话的安全上下文中。常见的会话劫持是使系统暴露出或更改密码。一个常用于会话劫持的工具是被称为Juggernaut的自由软件应用。它监听本地网络，维护一个TCP连接数据库。入侵者可以监视TCP流量来重放连接历史或是通过注入随意的命令来劫持一个活跃的会话。最好的防御会话劫持以及其他基于协议的攻击技术是使用虚拟隐私网络（VPN）或其他形式的加密通信。

### Application-Level Attacks

应用层级的攻击技术的一个常见的例子就是缓冲区溢出。当一台计算机通过网络接收（或来自键盘输入的）数据，计算机必须保留足够的内存空间来接收完整的数据集。这个接收空间称为缓冲区。

注入攻击一种新类型的攻击。注入攻击利用使用比如SQL或LDAP服务的程序的问题。

![https://i.cthee.cyou/b19ecfc38a8743f0.svg](https://i.cthee.cyou/b19ecfc38a8743f0.svg)

[CVE(Common Vulnerabilities and Exposures project)](cve.mitr.org)，[SANS](www.sans.org)提供关于securiry threats的邮件新闻列表。如果有机会，不要让网络应用运行时使用root用户或具有管理员权限。（某些情况下，可能会别无选择）。对于那些需要高权限来运行的应用，*jail*或*sandbox*工具集比如UNIX/Linux工具*chroot*可以创建一个有限的安全环境，防止入侵者访问系统的剩余部分。

### Root Access

网络入侵者梦寐以求的东西是能够管理或使用root访问系统。一个具备使用root访问权限的用户可以执行任何命令或查看任意文件。在入侵者登录到系统内部之后，经常第一件事请要做的就是上传*rootkit*。**rootkit**是一个用于在系统中建立更稳固的落脚点的工具集合。

现代的rootkits提供额外的特性。***Key loggers***捕获并且记录键盘输入，等待用户输入密码。所谓的内核rootkits在操作系统中以更高的安全等级运行并且几乎不可能使用常规的检测技术检测出来。

### Go Phishing

一种重要的新的策略是引诱信任的用户通过一个虚假链接，邮件信息或网页作为诱饵发起攻击。这种攻击类型属于钓鱼攻击类型。

钓鱼攻击经常利用链接显示独立于实际URL的事实。最好的策略是别点击在邮件信息中的链接，以及不透露出任何线上的财务信息除非是自己主动参加活动并且知道自己将要去往什么网站。

其他更高级的网络钓鱼技术是更加难于跟踪以及检测。称为跨站脚本的一种策略是使用代码注入来初始一个不容易跟踪到用户查看的页面的恶意脚本来穿过浏览器的安全防护。

比如防火墙等设备最初的目的是阻止源自外部的攻击。通过用户来初始化连接，攻击者能够绕过许多内置于网络安全基础设施的防护措施。浏览器以及防火钱不容易知道这个连接不同于任何连接到外部网站的连接。当连接建立是，攻击者使用一系列的策略来突破不可能从防火墙外突破的安全防护。这种类型的攻击甚至能够从分配给用户系统的一个不可路由的IP地址的NAT（Network Address Translation）中免疫。

![https://i.cthee.cyou/ed862dea07274db6.svg](https://i.cthee.cyou/ed862dea07274db6.svg)

### Denial-of-Service Attacks

DoS（denial-of-service）攻击一旦开始几乎不可能被阻止，因为这并需要攻击人在系统上有任何的特殊权限。DoS攻击的要点是生成大量的请求以至于系统资源被消耗并且性能下降。

最危险的DoS攻击是分布式DoS（distributed DoS）攻击。在DDoS攻击中，攻击人使用多台远程计算机知道其他远程计算机发起协同攻击。DoS攻击经常使用标准的TCPIP连接工具。比如，著名的smurf攻击使用`ping`工具释放对受害方的大量的ping响应。

![https://i.cthee.cyou/8f2297b604ff4d46.svg](https://i.cthee.cyou/8f2297b604ff4d46.svg)

攻击人通过定向广播发送ping请求到整个网络。ping请求的源地址被修改后使得它像是请求来自受害方的IP地址。然后网络上的所有计算机不断的响应ping请求。smurf攻击的影响是来自攻击人的起始ping请求在大型网络上形成多倍的ping请求。

### What to Do

小型网络中，一些提高防御本文讨论的攻击技术的实战经验：

- 使用安全的密码。不同的策略，但是大多数专家推荐含有字母，数字以及标点符号的密码字符的最小长度为8个字。
- 不共享密码。不在明显的地方写下密码并留下来。
- 不点击可疑的或是未经请求的链接。
- 使用最小的权限运行
- 如果使用Windows，确保有某些病毒防护措施。
- 关掉不需要的服务。
- 如果必须访问内网，设置一个VPN用来加密通信
- 使用防火墙。关闭所有的端口，并且除非需要所有的网络服务，否则将它们都关闭掉。
- 在沙盒环境中运行网络服务，目的是为了让入侵者不能够扩大权限。
- 给无线网络使用高质量的加密方式。旧的路由器使用WEP以及最初的WPA。当前标准是使用256 bit加密密钥的WPA2.
- 使用安全的程序设计经验来编写脚本以及其他程序，包含用于防止缓冲区溢出以及注入攻击的类型检查以及输入验证。
- 经常安装安全更新。

## 小小的总结

- 状态防火墙的优点是通过监视连接状态，状态防火墙能够注意到确定的DoS攻击，还有无效的包以及劫持或操纵会话的欺骗手段。
- DMZ的目的是提供一个比内部网络更容易进入但又比开放网络更具安全性的中间安全区域。

[上一篇](/Name-Resolution)
[下一篇](/Configuraion)