---
title: HTTP, HTML & WWW
date: 2019-10-14 22:30:28
categories: Sams-Teach-yourself-TCP/IP-in-24-hours
tags: [notes, TCP/IP, Networks]
---

本文旨在：

- 展示WWW如何工作
- 使用文本以及HTML标签建立一个基本的网页
- 他哦阿伦HTTP协议以及描述它是如何工作的
- 解释语义化网页的目的

## What is the World Wide Web？

通过浏览器窗口看到的网页是浏览器和网页服务器进行会话的结果。该会话使用的语言是超文本传输协议（Hypertext Transfer Protocol，HTTP）。从服务器传输到客户端的数据通过超文本标记语言（Hypertext Markup Language，HTML）渲染成一个统一的文档。World Wide Web是在1989年时由Tim Berners-Lee在CERN研究机构创造出来的。他创造了一个能够将在当时已经处在开发中的三种技术结合在一起的精巧且强有力的信息系统：

- Markup language: 一个指令与格式化嵌在文本中的代码的系统
- Hypertext: 一种用于在文本中嵌入链接到文档，图片以及其他元素的方式
- The Internet: 通过TCP/IP，一个客户端请求服务以及服务器提供服务的全球计算机网路。

标记语言作为一种早期计算机用于往简单文本中添加格式化以及排版的代码的方式。早些时候开发的标记语言（例如如今仍在使用的TeX）被科学家用于格式化以及排版数学表达式。

超文本（在文本中一个能够切换到该链接指向的文档的链接）的概念在1960s年代得到了发展。Berners-Lee通过开发统一资源定位符（URL）或统一资源标识符（URI）给互联网带来了超文本的概念。HTML文档能够组合成统一的页面和链接系统。
![https://i.quantuminit.com/7d6c0268e79f472b.svg](https://i.quantuminit.com/7d6c0268e79f472b.svg)

从[上一篇](/The-Internet-A-Closer-Look)文章可以知道，URL大多与Web相关连：

https://quantuminit.com

也能常常见到在URL的后面跟着一个路径以及文件：

https://tools.ietf.org/rfc/rfc2616.txt

浏览器通过URL来进行导航。在浏览器窗口中的地址栏中输入页面的URL来访问一个网页。总结这个简单的介绍，一个基本的HTML文档包含一些组合：

- 头部信息（与文档相关的元数据）
- 文本
- 图形
- 文本格式化代码（字体以及布局信息）
- 对图形文件等的辅助文件的引用
- 当前文档中其他HTML文档或其他闻之的链接

浏览网页的简单步骤：浏览器中地址栏输入URL ==> 浏览器初始化一个连接到URL中指定的服务器的连接 ==> 服务器通过网络发送HTML数据到浏览器 ==> 浏览器为了创建在浏览器中显示的网页而去解释HTML数据。

