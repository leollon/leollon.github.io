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

