---
title: Web Services
date: 2019-11-01 21:52:16
categories: Sams-Teach-yourself-TCPIP-in-24-hours
tags: [notes, TCPIP, Networks]
---

本文的最终目的是：

- 讨论博客，维基，以及社交网络站点
- 理解端到端网络的工作方式
- 讨论 Web 服务架构
- 理解 Web 服务中的 XML，SOAP，WSDL 以及 REST 的角色
- 描述电子商务网站如何处理现金业务

## Content Management System

CMS 本质上是 web 服务器的一个扩展。通常跑在 web 服务器机器上并且来自远程客户工作站通过一个 web 接口和这个机器进行交互。通过 CMS 管理的 web 内容通过可扩展标记语言或一些后端数据库的形式作为系统的属性值被存储以及管理。

![https://i.cthee.cyou/b921a1e627784f49.svg](https://i.cthee.cyou/b921a1e627784f49.svg)

## Social Networking

### Blogs and Wikis

Blog （ weblog 的简写）是电子杂志或在线日志，在这个 blog 上面，新的故事被放在上面并且过去的故事被放在下拉方向上的列表中。大部分的 blogs 基本上是 CMS 的特殊形式，并且许多标准的 CMS 工具提供内置的博客编辑支持。

学习 blog 如何工作的一种方式是查询发送给客户端的源代码。

Wiki 是为了方便协作以及信息共享而提供空间的一个网站。 Wiki 的关键点是给用户发表笔记，文档，以及其他重要信息提供地方。


Wikis 被公司以及其他组织扩展用来作为计划，协同工作，以及组织文档的工具。 Wiki 系统的设计可以多样化，但是可以将 wiki page 或 entry （例如 Wikipedia 中的条目）当作分配给标准属性的值的一个集合。一个 XML schema 或者相似的数据结构可能定义一系列与条目有关联的值，例如：

- Title：条目的标题
- Category：通过话题对条目的层级分类
- Language：使用何种语言编写该条目
- Contents：和条目相关联的完整的 HTML 代码

对文本的修订也能够通过该结构的扩展进行跟踪。当页面被请求时，数据会合布局标签以及其他格式信息合并到一起形成出现在浏览器中的代码。

## Peer-to-Peer

Peer-to-peer实际上借鉴了在本地区域网络（LANs）中的相关配置，peer-to-peer 的形式允许计算机通过网络在数据共享社区中共享数据。换句话说，就是数据不是来源于服务来自多个客户端请求的单个 web 服务器。而是，数据存储在遍布社区的普通的 PCs 上。

Peer-to-peer 网络最好的部分是在软件内部处理请求 IP 地址的细节以及网络连接的建立。

![https://i.cthee.cyou/25f738c9e0124250.svg](https://i.cthee.cyou/25f738c9e0124250.svg)

Peer-to-peer 网络会涉及到版权问题。

## Understading Web Services

使用现成的工具，技术以及 web 协议作为创造自定网络应用程序的基本。这种方式，又称为 web 服务架构。

在 web 服务架构后的思想是 web 浏览器，web 服务器，以及 TCPIP 协议栈处理网络细节，这样，程序员就能够集中精力在应用程序的细节上。

HTTP 传输系统是我们所知的 web 服务中唯一部分。还有重要的是在 web-based 环境中提供现成的类，函数，以及用于工作的编程接口。Web 服务应用程序经常使用在需要一个简单的客户端连接维护存货清单或处理订单的服务器的情况中。

几乎任何一家大公司都有需要能够跟踪预约，订单和存货清单的软件需求。web 服务框架有利于将分隔的服务以及事务整合进一个单独统一的环境中。

数据用标准标记格式——一般是 XML 传输通过 web 服务系统的组成部分，尽管还有第二种选择比如 JSON 正受到开发者的欢迎。

XML 是一种用于给属性赋值高效，通用的方式。Simple Object Access Protocol(SOAP) 提供一种用于在 web 服务之间传递 XML-based 数据的标准方法。SOAP 也描述了如何使用 XML 以及 HTTP 去调用远程程序。SOAP 信息传递进入和传递来自通过 Web Services Description Language (WSDL)定义的网络服务。

其他专家主张回到简单的方式上来，使用设计通过标准的 HTTP 命令运行的一个细致地有结构的系统。REST (Representational Sate Transfer) 架构反映出了这方面的着重点。

## XML

对于 HTML 数据来说，通过一系列预先定义的 HTML 标签有限制地表达了其中的含义以及上下文。比如数据包裹在 `<a>`标签里面，被解释成一个链接。而 XML 让用户可以自定义自己的元素。

XML 格式看起来有点像 HTML，但是确实不是 HTML。可以在 XML 中随意地使用标签，因为不会为某些特定的，具体的预定义应用程序，比如浏览器准备数据。数据就仅仅是数据。思想是无论谁为该文件创建出结构，随之而来地也会创建一个应用程序或样式表读取文件并且理解数据所表达的意思。

一个单独的文档包含有 XML schema （用来格式化以及翻译 XML 数据的路标）。schema 文档的存在使得很容易检查 XML 数据的有效性，并且容易地创建解析以及处理 XML 数据的客户端应用程序。

XML 经常被描述成是一个“用于创造标记语言的标记语言。”

## SOAP

XML 定义了一种用于交换应用程序数据的通用格式。SOAP 是在一种用于交换在 web 服务客户端和服务器之间传输的 XML-based 消息的协议。


SOAP 设计用于支持在所谓的 SOAP 节点之间的通信。SOAP 规范定义了从 SOAP 发送放到 SOAP 接受方的传递的消息结构。

![https://i.cthee.cyou/d2b13dee62584647.svg](https://i.cthee.cyou/d2b13dee62584647.svg)

SOAP 消息的结构由一个可选的头部以及消息主体组成。头部含有给消息通过的路径上的节点上使用的编号，定义以及元信息。主体含有发送给消息接受端的数据。

## WSDL

WSDL (Web Services Description Language) 为了描述和 web 服务相关联的应用程序提供 XML 格式。根据 W3C 的规范，“WSDL 是一个用于描述网络服务作为一系列运行在包含面向文档或面向过程的信息的消息的端点集合的 XML 格式。” WSDL 是一种用于定义通过 SOAP 消息交换信息的服务的格式。

一个 WSDL 文档最初是一个定义集合。在文档中的定义制定了数据上传输的信息，以及和那些数据相关联的操作，还有与数据相关的服务以及服务位置。

WSDL 不局限与 SOAP，也可与其他 web 服务通信协议一起使用。

## Web Service Stacks

类似 TCPIP 一样，由一个组件栈构成一个 web 服务环境。Linux 厂商以及开发者经常谈论 **LAMP** 栈，一个开源组件集合容易被修改用于 web 服务环境。LAMP 是如下主要栈组建的缩写：

- Linux: 支持服务器应用程序运行在服务系统上的一个操作系统
- Apache: 支持服务 XML-based SOAP 消息的一个 web 服务器。
- MySQL: 能够访问后端数据服务的一个数据库系统
- PHP (or Perl or Python): 用于编写自定义 web 服务应用程序细节的一门 Web 即用型编程语言。

## REST

XML 的强大之处以及客户端/服务器模型能够引导出各种分享请求以及传输数据的应用程序。对于一个自定义服务器来说，可能以几乎任意种格式给任意类型自定义客户端应用程序提供几乎任意种类的自定义信息。

不像 SOAP，REST 自身来说不是一个协议。而是，它是一个用于创建简单，整洁以及可移植的 web-based 应用程序的设计原理。REST 系统可以分解成如下几个非常基础的元素：

- Resource: 请求的目标（客户端想要的东西）。这可以是一个网页，一行数据库记录，或其他编程对象
- Resource identifier: 给资源命名的一个 URI。
- Representation: 来自以完整格式传输资源的服务器的响应。要注意的是资源没必要以表征的形式存储而传输给客户端。这个对象可能在服务端动态组合成表征形式而传输给客户端。

除了主要的 REST 的元素（Resource，Resource identifier，以及 representations）之外，还有不同形式的资源和表征元数据和消息一起传输，目的是为了表明数据的本质。

REST 系统重要的部分是客户端不告诉服务器做什么事情；它只说它想要的东西。REST 不使用传统的 API 共识，客户端通过该 API 调用服务端上的进程。而是，客户端简单地发送一个以一个 URI 的形式的标识符标识其想要添加，查看或修改的资源以及在 URI 的主体中提供必要的信息来完成请求。

通过一个基本的 REST 请求指定唯一的动作是标准的 HTTP 方法：

- GET: 从服务器上获取一份资源。
- PUT: 直接地创建或修改一份资源。
- POST: 向服务器提交数据用于修改资源。
- HEAD: 获取与资源相关连的元数据。
- DELETE: 删除资源。

*POST* 和 *PUT* 命令之间的不同点是， *PUT* 替换一份资源的全部内容。 *POST* 向服务器提交的信息在更新资源中用到，在不推断更新将如何进行的情况下。 *PUT* 据说是幂等性的，其意思就是同样的动作导致同样的结果，不管它被执行多少次。 *POST* 则没有不能够保证。 REST 设计原则强调尽可能地使用幂等方法，但是 *POST* 方法被包含在必要的情况中的。在幂等性的操作上的强调定义了 REST
系统的特性。例如，SOAP-based 系统 偏向于扩展使用非幂等性的 *POST* 操作，这实际上就最小化数据传输以及网络带宽来说可以更高效。

数据一般以 XML 格式传进服务器，有时候 REST 服务也支持 JSON还有普通的 HTML 。理想情况下，从服务器返回来的数据是表征状态的形式，通常是 HTML 或其他使用 web 浏览器容易处理的格式。

围绕 REST 模式设计的一个网站，服务，或开发框架被称为 RESTful。

## E-Commerce

一个典型的 web 交易场景如下图所示，其过程如下描述：

1. 一个 Web server 提供一个从 Web 进行访问的在线目录。用户浏览通过互联网有其他地方提供的商品。
2. 用户决定买商品并且点击在网页上的购买商品链接。
3. 服务器与浏览器进行安全的连接。这时候，浏览器有时候显示一则比如 "You are now entering a secure area..."消息。不同的浏览器使用不同的方法来表明安全连接。
4. 建立连接之后，通常接下来是一些验证形式。在一些交易站点，买家与厂商建立某种形式的用于帐号。部分是为了安全的原因以及便理性（目的是用户能够跟踪购买的状态）。用户帐号信息也让厂商跟踪用户行为并且关系用户的个人信息以及购买历史。登录操作需要 web
   服务器与后端数据库服务器进行通信——要么创建一个新的帐号或检查登录到一个存在的帐号的凭证。最近受到欢迎的另一种方式是直接在会话里面提供信用卡信息而不需要登录。
5. 服务器（或一些工作在服务器后端上的应用程序）必须验证信用卡信息并且向监督任务的信用卡机构注册交易，这些任何与执行以及验证信用卡信息相关联（经常又称为支付网关）。信用卡机构通常是一个商业服务。
6. 如果允许交易，购买的通知以及邮件信息被发往厂商的发货部门，并且交易软件参与到最后与用于确认的购买细节中以及更新用于帐号的信息。

## 小小的总结

- P2P 网络的特点是网络中的节点即可以充当客户端又可以充当服务器。
- PUT 方法替换整个资源内容。POST 方法向服务器传输用于更新资源的信息。PUT 方法是幂等的。POST 方法不能保证幂等性。
- REST 强调简单，完整的操作，让系统处在在一个完整的，可预测的状态。
- REST 三个基础要素：Resource，Resource identifier，Representation。

[上一篇](/HTTP-HTML-WWW)
[下一篇](/Encryption-Tracking-Privacy)

