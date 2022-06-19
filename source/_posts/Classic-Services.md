---
title: Classic Services
date: 2019-09-23 22:19:32
categories: Sams-Teach-yourself-TCPIP-in-24-hours
tags: [notes, TCPIP, Networks]
---

本文目的是学习：

- HTTP
- Email
- FTP file transfer
- File and print services
- LDAP
- IRC and IM messaging

## HTTP

在World Wide Web中心的HTTP（Hypertext Transfer Protocol）本质上是用于以超文本标记语言（Hypertext Markup Language， HTML）格式来传输以及请求数据和图片的应用层协议。一个web服务器首先是一个HTTP服务器。

## Email

标准的邮件依赖于一对服务器系统，这也就是为什么需要在邮件阅读器配置界面配置一个“接收服务器”以及一个“发出服务器”的原因。使用简单邮件传输协议（SMTP）的发送服务器从用户接收发送出去的消息并且通过其他SMTP服务器网络转发到目的地址。通常使用POP或IMAP协议的接收服务器接收发送到用户邮箱帐号的信息并且等待用户的邮件阅读器请求连接并且访问消息。

## FTP

广泛使用的文件传输协议（File Transfer Protocol）让用户能够在TCPIP网络上的计算机之间进行传输文件。用户在一台计算机上运行FTP客户端，并且在另外一台计算机上运行FTP服务器应用程序，比如UNIX/Linux计算机上的ftpd（FTP 守护程序）或其他平台上的FTP服务。FTP最初是用于传输文件，尽管它能够执行其他比如创建目录，删除目录以及列出文件的功能。

FTP使用TCP协议并且因此通过一个在客户端计算机和服务器计算机之间的可靠的，面向连接的会话运行。服务器上标准的FTP守护程序使用TCP端口21监听来自客户端的请求。

在UNIX世界中，一个守护程序使用运行在后台的一个进程并且当请求服务时提供服务。在Windows世界中，一个守护程序被称为一个服务。

当使用FTP传输文件的时，必须向FTP指定将要传输的文件的文件类型。最常见的选项是二进制和ASCII。当传输的文件是简单的文本文件时，使用ASCII。当传输的文件使用程序文件，word文档或一个图形文件时选择二进制。默认的文件传输模式是ASCII。

在本地计算机上的当前目录开始FTP会话时，当前目录是文件传输过程中的默认目录。

常用的FTP命令列表及其解释：

- `ftp`：ftp命令用于运行ftp客户端程序。可以输入只输入ftp或输入他的同时后面跟上空格并且加上IP地址或域名。
- `user`：用于更改当前会话的用户ID以及密码信息。将会提示输入新的用户ID以及密码。这个命令相当于退出当前会话然后又开启一个新的用户。
- `help`：显示在FTP客户端上可用的FTP命令。
- `ls` or `dir`：UNIX/Linux 的`ls` 或 `ls -l`命令或Windows`dir`命令用于列出FTP服务器上当前工作目录的文件名以及目录名称。`ls -l`命令类似与`ls`但是它能够列出额外的细节比如读写权限以及文件创建时间。

![https://i.cthee.cyou/4ca26180261540fa.svg](https://i.cthee.cyou/4ca26180261540fa.svg)

- `pwd`：`pwd`命令打印在远程服务器上当前工作目录的名字。
- `cd`：更换FTP服务器上的当前工作目录。
- `mkdir`：在FTP服务器上的当前工作目录内部创建一个目录。通常匿名的FTP会话不允许使用这个命令。
- `rmdir`：从FTP服务器上的当前工作目录删除一个目录。通常匿名会话不允许使用这个命令。
- `binary`：将FTP客户端使用的默认ASCII传输模式切换成二进制传输模式。二进制模式在传输二进制文件时能够用到。比如使用`get`，`put`，`mget`以及`mput`传输程序以及图形文件时。
- `ascii`：将客户端传输模式从二进制传输模式切换成ASCII传输模式。
- `type`：显示当前的文件传输模式。
- `status`：显示FTP客户端上多个的设置信息。比如客户端设置的传输模式以及是否客户端是否设置显示详细的系统信息。
- `get`：从FPT服务器上获取一个文件。get后面接单个文件名时，将从服务器上复制该文件到本地计算机当前目录中。如果接两个文件名，第二文件名将会是复制的文件在本地计算机上的文件名。
- `mget`：`mget`命令与get相似除了它可以获取多个文件之外。
- `put`：从FTP客户端传输一个文件到FTP服务器中。put后面接一个参数时，在服务器上的文件名和客户端上的一只，接两个参数时，第二个参数是在FTP服务器上的该复制文件的文件名。
- `open`：用于与FTP服务器建立一个新会话。本质上是退出FTP和再次开启的快捷方式。`open`命令能够用于和一个完全不同的FTP服务器打开一个会话或与当前的服务器重新开启一个会话。
- `close`：`close`命令用于关闭与一个FTP服务器的当前会话。FTP客户端程序依旧处于打开状态，并且可以使用`open`命令和服务器打开一个新的会话。
- `bye`或`quit`：关闭当前的FTP会话并且终止FTP客户端运行。

如果想要使用比FTP更安全的文件传输工具，可以使用含有`scp`和`sftp`工具的SSH工具包。

## Trivial File Transfer Protocol

TFTP（Trivial File Transer Protocol）用于在TFTP客户端与TFTP服务器之间传输文件的协议。这个协议使用用户数据报（UDP）作为传输层（TCPIP模型中从上往下的第二层），不像FTP，不需要用户登录来传输文件。TFTP被设计得很小，因此它和UDP都能够在PROM（programmable read-only memory，可编程只读内存）芯片上实现。TFTP只能读写文件，不能列出目录中的内容，创建或删除目录或像FTP一样允许用户登录。TFTP最开始是与RARP和BOOTP协议联合使用来启动无盘工作站以及在某种情况下，上传新系统代码或给路由器或其他网络设备打补丁。它使用称为netascii的ASCII格式或称为octet的二进制格式两者之一来传输文件。

使用tftp传输完文件之后，会话将关闭并终止。tftp命令使用语法格式：

```bash
TFTP [-i] host [get | put] <source filename> [<destination filenae>]
```

阅读[RFC 1350](https://tools.ietf.org/html/rfc1350)，获取学习TFTP协议细节。

## File and Print Service

如`ftp`和`tftp`工具都是运行在TCPIP协议栈的应用层的独立应用程序。介绍两种提供完整的网络文件访问的协议：

- Network File System(NFS)：在UNIX和Linux系统上使用的协议
- Common Internet File System/Server Message Block(CIFS/SMB)：WIndows客户端用于提供远程访问文件的协议

### Network File System

NFS最初由Sun开发，现在在UNIX，Linux和其他系统上都得到支持。NFS允许用户访问（读写，创建以及删除）位于远程计算机上的目录以及文件，就好像这个目录和文件在本地计算机上一样。NFS在本地文件系统和远程文件系统之间都有实现，所以不需要应用程序的任何改变。程序能够通过NFS而不需要任何重新编译或其他改变就能够访问本地文件和远程文件以及目录，就好像它们只存在在本地文件系统上一样。

NFS被设计成独立于操作系统，传输层以及物理网络结构而存在。在客户端计算机和服务器计算机之间使用远程过程调用（Remote Procedure Calls，RPCs）来实现这一独立性。RPC是一个使运行在一台计算机上的一个程序调用运行在另一台计算机上的程序的内部代码段的过程。在NFS例子中，客户端操作系统向服务器上的操作系统发起一个远程过程调用。

在远程文件和目录在NFS系统可以使用之前，得先经过挂载。在成功挂载之后，远程文件和目录存在并可以像在本地文件系统上一样操作。

### Server Message Block and Common Internet File System

服务器消息块（SMB）是支持Windows用户界面的具有网络功能的工具的一个协议，比如，Explorer，网络邻居以及映射网络驱动。SMB是设计运行于各种不同协议系统之上，包括IPX/SPX，NetBEUI，以及TCPIP。

每个SMB的会话以一个初始信息交换而开始，在信息中协商SMB对话规范以及验证一个客户端并登录到服务器上。验证过程的细节根据操作系统以及配置有所变化，但就SMB关注的而言，登录信息被封装在*sesssetupX*SMB中。在SMB协议之下的的协议传输简单称为一个SMB。）

如果登录成功，客户端发送一个指定网络共享名字的SMB给服务器，这个网络共享也是客户端想要访问的。如果共享访问成功，客户端能够打开，关闭，读取，或写入网络资源，并且服务器发送必要的数据来完成客户端的请求。

另一个等同于SMB的版本是公用文件系统（CIFS）。开发者以及其他支持服务器与Window客户端通过SMB交流的操作系统都知晓SMB和CIFS协议的细节。一个称为Samba的开源服务器为UNIX/Linux系统提供SMB文件服务。

当在Windows中配置文件共享时，本质上是配置这台计算机，使其充当一个CIFS服务器。

## Lightweight Directory Access Protocol

在大型网络上，以统一高效地管理资源信息的任务的困难日益增加。当初开发轻量目录访问协议（LDAP）是作为基于TCPIPX.500数据模型的继承人。LDAP是一个**目录服务**。一个LDAP服务器维护一个关于按逻辑且类似树状结构组织管理的网络资源的信息目录。LDAP运行TCPIP应用层 并且监听到达TCP端口389的请求。

也许最著名的基于LDAP的系统就是微软的活跃目录（Active Directory）。在开源世界中，OpenLDAP更受到倾睐。

一个LDAP目录的结构定义在一种模式中。这个模式清楚的说明了定义存在目录中的数据的属性集合。例如一个雇员记录的目录中可能包括雇员的姓名，地址以及用户ID等的属性。

一个LDAP目录被有层次地进行组织管理，就像一个文件目录结构一样。每个条目都有一个定义它在树中位置的distinguished name（DN）。这个DN由一个唯一定义容器中的条目的relative distinguished name（RDN），加上一系列定义条目所在的容器的层次的部件构成。如下图。

dn: cn=Ellen Johnson, ou=employees, dc=pearson,dc=com

![https://i.cthee.cyou/85483ac5df90454a.svg](https://i.cthee.cyou/85483ac5df90454a.svg)

一个DN可能看起来像是这样子的：

dn: cn=Ellen Johnson, ou=employees,dc=pearson,dc=com

注意到与名字值关联的两个字母的属性类型（在等号左边的）。LDAP预先定义一些用于在建立DN中定义的标准属性类型，包括如下：

- Domain component（dc）：在定义目录层次的嵌套容器链中的一个条目。在之前的例子中，*dc*条目提到一个DNS域名（pearson.com)。
- Organization unit（ou）：为了管理方便对条目进行分组的一个容器。一个ou可能定应一些逻辑组，比如一个部门，然而一个*dc*更可能反映网络自身的结构。
- Canonical name（cn）：在容器中唯一的对象的一个可读性友好的名字。

在之前的例子中，cn充当RDN。也有可能使用另一个别的名字，比如一个用户ID或员工工号做为在区别名字内的RDN元素：

dn: userid=ejohnson,ou=Employees,dc=pearson,dc=com

LDAP是一个二进制格式。字母数字表现形式，比如之前的例子实际上是能用于读取并且报告LDAP数据的LDAP Data Interchange Format（LDIF）。

读取目录信息通过采取传递URLs到LDAP服务器的形式。前缀（或这说方案）`ladp`用于将URL和LDAP协议管理起来。如下：

ldap://ldap.pearson.com/userid=ejohnson,ou=employees,dc=pearson,dc=com

读取所有在ldap.pearson.com与下面关联的专有的名字的属性：
userid=ejohnson, ou=employees,dc=pearson,dc=com

指定一个特定的属性，用问号符号包起来：

ldap://ldap.pearson.com/userid=ejohnson,ou=employees,dc=pearson,dc=com?phonenumber?

LDAP有时候回合其他验证工具结合起来为用户提供一种用于验证LDAP数据存储的安全方式。例如，Active Directory 集成Kerberos验证。Unix/Linux管理员可能将LDAP和Kerberos或即插即用验证模块（PAM）系统组合起来一起使用。

除了支持由现成系统比如Active Directory提供标准用户和资源管理服务之外，LDAP也能够容易地适配自定义的应用程序。对于LDAP来说，任何需要从常见的数据存储中进行网络查询，以及从目录风格的数据结构而不是扁平文件或SQL风格的数据库中受益的场景，这些场景LDAP都能够提供参考。LDAP的协议（schema）框架能够容易的轻松地适配面向对象变成方法，并且许多编程环境提供应用编程接口（APIs）和用于支持LDAP查询的其他工具。

## Remote Control

大部分用户不再从shell提示窗口进行操作，而是喜欢通过鼠标在图形接口（GUI）点击来操作。许多远程接入协议以及工具让用户使用平常的键盘与鼠标的桌面操作来控制远程系统。通过GUI提供远程访问的任务有些复杂，但是原理都是一样的。运行在计算机A的应用层软件部分拦截键盘的输入并且通过协议栈重定向到计算机B。来自计算机B的屏幕输出通过网络发送回计算机A。这样结果是计算机A的键盘和鼠标充当计算机B的键盘鼠标，并且计算机A的屏幕显示计算机B桌面。简而言之，用户可以在计算机A通过远程控制可以看到并且操作计算机B。

![https://i.cthee.cyou/6bfab9684bad43d0.svg](https://i.cthee.cyou/6bfab9684bad43d0.svg)

一些GUI-based远程控制工具，Apple使用Apple Remote Desktop工具，Windows使用支持远程桌面协议（RDP）的远程桌面链接工具，Unix/Linux系统总是通过X Server图形环境的基本架构来简单地实现这个功能。然而，最近的工具，比如虚拟网络计算（VNC）以及NoMachine's NX已经增添了便利性并且给端用户提供了远程访问。

## 小小的总结

- 现代网络上LDAP的主要的角色是维护一个能够通过TCPIP轻松访问到的网络和用户信息目录。
- TFTP不能列出目录中的内容，创建或移除目录，或允许用户像FTP一样登录。
- TFTP主要用于和RARP以及BOOTP协议联合起来用于启动无盘工作站，以及在某些情况下，用于上传新系统代码或补丁到路由器或其他网络设备中。
- NFS通过在客户端以及服务器之间使用RPC来完成不依赖于操作系统的特性。
- SMB以及CIFS是能够支持文件共享功能的协议。

[上一篇](/Classic-Tools/)
[下一篇](/The-Internet-A-Closer-Look/)
