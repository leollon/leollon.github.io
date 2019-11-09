---
title: Web Services
date: 2019-11-01 21:52:16
categories: Sams-Teach-yourself-TCP/IP-in-24-hours
tags: [notes, TCP/IP, Networks]
---

本文的最终目的是：

- 讨论博客，维基，以及社交网络站点
- 理解端到端网络的工作方式
- 讨论 Web 服务架构
- 理解 Web 服务中的 XML，SOAP，WSDL 以及 REST 的角色
- 描述电子商务网站如何处理现金业务

## Content Management System

CMS 本质上是 web 服务器的一个扩展。通常跑在 web 服务器机器上并且来自远程客户工作站通过一个 web 接口和这个机器进行交互。通过 CMS 管理的 web 内容通过可扩展标记语言或一些后端数据库的形式作为系统的属性值被存储以及管理。

![https://i.quantuminit.com/b921a1e627784f49.svg](https://i.quantuminit.com/b921a1e627784f49.svg)

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

Peer-to-peer实际上借鉴了在本地区域网络（LANs）中的相关配置，eer-to-peer 的形式允许计算机通过网络在数据共享社区中共享数据。换句话说，就是数据不是来源于服务来自多个客户端请求的单个 web 服务器。而是，数据存储在遍布社区的普通的 PCs 上。

Peer-to-peer 网络最好的部分是在软件内部处理请求 IP 地址的细节以及网络连接的建立。

![https://i.quantuminit.com/25f738c9e0124250.svg](https://i.quantuminit.com/25f738c9e0124250.svg)


Peer-to-peer 网络会涉及到版权问题。

## Understading Web Services

使用现成的工具，技术以及 web 协议作为创造自定网络应用程序的基本。这种方式，又称为 web 服务架构。

在 web 服务架构后的思想是 web 浏览器，web 服务器，以及 TCP/IP 协议栈处理网络细节，这样，程序员就能够集中精力在应用程序的细节上。

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

![https://i.quantuminit.com/d2b13dee62584647.svg](https://i.quantuminit.com/d2b13dee62584647.svg)

SOAP 消息的结构由一个可选的额头部以及消息主体组成。头部含有给消息通过的路径上的节点上使用的编号，定义以及元信息。主体含有发送给消息接受端的数据。

## WSDL

WSDL (Web Services Description Language) 为了描述和 web 服务相关联的应用程序提供 XML 格式。根据 W3C 的规范，“WSDL 是一个用于描述网络服务作为一系列运行在包含面向文档或面向过程的信息的消息的端点集合的 XML 格式。” WSDL 是一种用于定义通过 SOAP 消息交换信息的服务的格式。

一个 WSDL 文档最初是一个定义集合。在文档中的定义制定了数据上传输的信息，以及和那些数据相关联的操作，还有与数据相关的服务以及服务位置。

WSDL 不局限与 SOAP，也可与其他 web 服务通信协议一起使用。

