---
title: What is a web server?
date: 2017-08-17 22:04:24
categories: Network
tags: [notes, HTTP, web_server]
---

web server涉及到硬件和软件。

硬件负责存储一个网站的所有内容，即HTML文，图片，视频，语音， CSS， JavaScript，并且还提供操作系统以及网络连接。通过网络域名或者ip地址可以访问这台服务器。

软件负责解析用户通过浏览器发送过来的HTTP请求，解析请求中URLs以及HTTP(浏览器浏览网页使用的协议)。

<!-- more -->



发布一个网站，需要一个静态的服务器或者是一个动态的服务器。

- 静态服务器，或者堆栈，由硬件(计算机)和软件(HTTP 服务器)组成，之所以称为静态的，是因为静态服务器解析url得出对应的文件并通过HTTP发送给用户浏览器的时候和存储在这台服务器上面的文件的内容是一模一样的。比方说，服务器上面存储的文件的文件名是index.html, 该文件内容是"hello world"。那么服务器(HTTP server)发送给用户浏览器的文件名和文件内容分别依旧是index.html和'hello world'。

- 动态服务器，由静态服务器和额外的软件组成，比如数据库，应用服务器。应用服务器负责从数据库中获取数据并进行处理/或者不处理，然后放入到HTML模板中，再由静态服务器通过HTTP响应浏览器的URL请求。

**托管文件**

#### 硬件方面：

> web server也就是一台可以全年每分每秒都在运行的计算机，这台计算机存储着跟一个网站相关的所有文件，这些文件是HTML和图片，视频，CSS样式表，JavaScript文件等等。这台电脑可以是你自己的电脑，但是存储在专门的计算机上面更为方便。专门的计算机有如下优点：

- 时刻开着机并保持运行
- 时刻连接着互联网
- 时刻保持着同一个外网IP地址
- 由第三方服务商进行相关的硬件维护

#### 软件方面：

>使用HTTP进行通信交流

web server支持HTTP(Hypertext Transfer Protocol)。HTTP指定了客户端与服务器之间转移超文本的方式。
一份协议是两台电脑见进行进行通信的规则集合。HTTP是文本类型的，并且是无状态的协议。

**文本类型是什么意思呢？**

  意思就是这协议里面的内容是纯文本的，并且对于我们来说是可读的。比如下面浏览器的Request Headers:

    :authority:blog.quantuminit.com
    :method:GET
    :path:/
    :scheme:https
    accept:text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8
    accept-encoding:gzip, deflate, br
    accept-language:en-US,en;q=0.8
    cache-control:max-age=0
    cookie:__cfduid=dc58427250e06445edbc6017d751350a91499695700; _ga=GA1.2.277865214.1302012244; _gid=GA1.2.532311051.1503029392
    dnt:1
    if-modified-since:Fri, 18 Aug 2017 06:20:43 GMT
    referer:https://blog.quantuminit.com/
    upgrade-insecure-requests:1
    user-agent:Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/60.0.3112.101 Safari/537.36

**无状态又是什么意思呢？**

服务器和客户端都没有记住自己之前的通信。例如，服务器并不会记住你之前输入的密码抑或是在HTTP事务中进行到了哪一步。

HTTP协议关于客户端与服务器之间的通信给出了清晰的通信规则。最基本的规则是这样子的：

- HTTP Requests由客户端发起，服务器只能响应客户端的HTTP Requests。
- 通过HTTP请求一个文件，客户端必须提供文件的URL
- web server必须应答每一个HTTP Request，至少提供一个错误信息。

一个web server，也就是HTTP server 有责任去处理和应答到来的Requests。
   1. 当收到请求，HTTP Server第一件事情就是检查Request URL是否与存在的文件相互匹配
   2. 如果匹配，web server 发送这个文件内容回给浏览器。否则，应用服务器生成必要的文件
   3. 如果以上两种处理都没有得出相应的结果，web server返回给浏览器一个错误消息，最通常的就是"404 Not Found".

静态内容和动态内容：

一个服务器即可以提供静态内容也可以提供动态内容。

静态内容就是文件的内容在没有人为的干预的情况下是保持不变的，从服务器启动开始，这个文件内容是什么样子，提供给浏览器的内容就是什么样子的。

动态内容就是也就是有些数据是经过处理，然后将这些处理后的数据加入到HTML模板中。而这些数据的来源可能是数据库，也可能来自于其他文件，比如存储Json数据的文件。
