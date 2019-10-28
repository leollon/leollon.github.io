---
title: HTTP, HTML & WWW
date: 2019-10-14 22:30:28
categories: Sams-Teach-yourself-TCP/IP-in-24-hours
tags: [notes, TCP/IP, Networks]
---

本文旨在：

- 展示 WWW 如何工作
- 使用文本以及HTML标签建立一个基本的网页
- 讨论 HTTP 协议以及描述它是如何工作的
- 解释语义化网页的目的

## What is the World Wide Web？

通过浏览器窗口看到的网页是浏览器和网页服务器进行会话的结果。该会话使用的语言是超文本传输协议（Hypertext Transfer Protocol，HTTP）。从服务器传输到客户端的数据通过超文本标记语言（Hypertext Markup Language，HTML）渲染成一个统一的文档。World Wide Web 是在1989年时由 Tim Berners-Lee 在 CERN 研究机构创造出来的。他创造了一个能够将在当时已经处在开发中的三种技术结合在一起的精巧且强有力的信息系统：

- Markup language: 一个指令与格式化嵌在文本中的代码的系统
- Hypertext: 一种用于在文本中嵌入链接到文档，图片以及其他元素的方式
- The Internet: 通过 TCP/IP，一个客户端请求服务以及服务器提供服务的全球计算机网路。

标记语言作为一种早期计算机用于往简单文本中添加格式化以及排版的代码的方式。早些时候开发的标记语言（例如如今仍在使用的 TeX ）被科学家用于格式化以及排版数学表达式。

超文本（在文本中一个能够切换到该链接指向的文档的链接）的概念在1960s年代得到了发展。Berners-Lee通过开发统一资源定位符（URL）或统一资源标识符（URI）给互联网带来了超文本的概念。HTML 文档能够组合成统一的页面和链接系统。

![https://i.quantuminit.com/7d6c0268e79f472b.svg](https://i.quantuminit.com/7d6c0268e79f472b.svg)

从[上一篇](/The-Internet-A-Closer-Look)文章可以知道，URL 大多与 Web 相关连：

https://quantuminit.com

也能常常见到在 URL 的后面跟着一个路径以及文件：

https://tools.ietf.org/rfc/rfc2616.txt

浏览器通过 URL 来进行导航。在浏览器窗口中的地址栏中输入页面的URL来访问一个网页。总结这个简单的介绍，一个基本的 HTML 文档包含一些组合：

- 头部信息（与文档相关的元数据）
- 文本
- 图形
- 文本格式化代码（字体以及布局信息）
- 对图形文件等的辅助文件的引用
- 当前文档中其他HTML文档或其他闻之的链接

浏览网页的简单步骤：浏览器中地址栏输入 URL ==> 浏览器初始化一个连接到 URL 中指定的服务器的连接 ==> 服务器通过网络发送 HTML 数据到浏览器 ==> 浏览器为了创建在浏览器中显示的网页而去解释 HTML 数据。

## Understanding HTML

HTML 是通过 HTTP传 输的数据。当使用比如 Notepad 或 Unix 的 vi 等文本处理应用程序查看一个基本的 HTML 文档的内容的时候，将会发现这个文档实际上是一个普通的文本文件。这个文件包含任何出现在页面上的文本，以及还包含有许多称之为**标签**的特殊的 HTML 代码。标签是浏览器的指令。HTML 标签提同所有的格式化，文件应用，以及和网页相关联的链接。

一些中的 HTML 标签：

|标签|描述|
|:----|:----|
|`<html>`|标记文件中 HTML 内容的开头以及结尾|
|`<head>`|标记 header 区域的开头以及结尾|
|`<body>`|标记 body 部分的开头以及结尾部分，该部分用于描述出现浏览器窗口的的文本|
|`<h1>`, `<h2>`, `<h3>`, `<h4>`, `<h5>`, `<h6>`|标记标题的开头以及结尾。每个标题标签代表不懂的标题等级。`<h1>`是最高等级|
|`<b>`|标记一部分加粗文本的开头以及结尾|
|`<u>`|标记下划线文本部分的开头以及结尾|
|`<i>`|标记斜体文本部分的的开头以及结尾|
|`<font>`|标记特殊字体符号部分的开头以及结尾。现在的实践中避免使用这个标签以及以及表示字体信息，而是使用CSS。HTML5中比支持`<font>`标签。|
|`<a>`|定义一个锚点——通常用于标记一个链接。链接的目的 URL 作为 `href` 属性的值出现在第一个 `<a>` 标签的内部|
|`<img>`|指定一个应该出现在文本中的的图片文件|

当然还有更多的 HTML 标签。许多标签用于一个块级文本。如果这样子，标签出现在块的开头以及结尾。在结尾的标签包括一个斜杠（/），用来指出这是一个结尾标签。例如，如下的一个 H1 标题：

`<h1>大标题</h1>`

一个 HTML 文档应该以一个 `<!DOCTYPE>` 声明开始。`!DOCTYPE` 定义了这个文档使用的 HTML 版本。例如，HTML 5 的 `!DOCTYPE` 命令如下：

`<!DOCTYPE html>`

（使用特殊的浏览器扩展的网页可能指定不同的文档类型。）

跟在 `!DOCTYPE` 后面的是 `<html>` 标签。文档的剩余部分被包含在 `<html>` 和对应的 `</html>` 之间。在 `<html>` 标签的开头和结尾之间，这个文档被分割成两部分：

- 头部（head，被包含在 `<head>` 和 `</head>` 标签之间）含有关于这个文档的信息。出现在头部中的信息不会出现在网页中，但是 `<title>` 标签指定显示在浏览器窗口上的标题。`<title>` 标签是一个需要的元素。头部部分中的其他元素是可选的，例如 `<style>` 标签是关于文档样式的信息。
- 主体（body，被包含在`<body>`和`</body>`标签之间）是实际出现在网页上的的文本以及包含了任何与那些文本相关的 HTML 标签。

一份简单的 HTML 文档：

```HTML
<!DOCTYPE html>
<html>
<head>
<title> Ooh This is Easy </title>
</head>
<body>
Easy!
</body>
</html>
```

`<font>`标签在 HTML 4 中得到支持，但是在 HTML 5 中已经找不到。

HTML <font> 标签属性：

|属性|描述|
|:----|:----|
|size|相对字体大小设置。其值的范围从1到7:`<font size =7>`|
|lang|说明文本的语言的语言代码|
|face|字体设置：`<font face ="Arial">`|
|color|文本颜色：`<font color ="RED">`|

**Capitalization**

经典的 HTML 标签，大写不是很重要。然而，比如可扩展标记语言（XML）以及 XHTML 更注重大小写。XML 是大小写敏感的，并且 XHTML 要求小写元素以及属性名字。

## Cascading Style Sheets

层叠样式表（CSS）是将样式信息放入到一个单独文档的的一种方式。以此来代替将所有的样式放入到 HTML 文档中。这样做是为了让 HTML更 像是一个现代的文字处理机或从一个预先定义的字符或段落风格列表中选出的桌面发布工具。这个样式定义了一系列的参数，比如字体大小，颜色，间隔，填充，以及页边距。

使用 CSS 将文档内容从表现层信息分隔开。这样子是使得 HTML 更容易读，并且影响文档中风格上的一致性。

例如，一个标题风格可能像下方定义：

```css
h1 {
    Color: black;
    Font-family: Arial, sans-serif;
    Margin: 0 4px 0 0;
}
```

HTML 文件可以像下方一样引用 CSS 文件：

```html
<link href="path/to/filename.css" rel="stylesheet">
```

## Understanding HTTP

Web 服务器和浏览器使用超文本传输协议进行通信。HTTP 1.1 在1997年伴随着RFC 2068到来，目前被定义在 RFC 7230 - 7235中。2015年出来了一个称为 HTTP/2 的新版本，这个版本基于 Google 的 SPDY 协议，主要提供性能上的加强并且其目的不是用于替代 HTTP 的语义以及状态码。

HTTP 的目的是为了支持 HTML 文档的传输，它是一个应用层的协议。HTTP 客户端以及服务器应用程序使用可靠的 TCP 传输层协议来建立一个连接。

HTTP 做事情如下：

- 在浏览器（客户端）和服务器之间建立一个连接
- 协商会话的设置以及确定参数
- 有序地传输 HTML 内容
- 关闭与服务器的连接

为了提高性能表现，HTTP/2 添加了额外的应用到传输的过程中。

当输入一个 URL 到浏览器窗口中时，浏览器首先检查 URL 的 scheme 来决定协议。（大部分的浏览器支持除了 HTTP之 外的协议。）如果浏览器决定这个 URL 指向 HTTP 站点上的一份资源，则它从 URL 中提取出 DNS 名字并且初始化名字解析过程。客户端计算机向一个名字服务器发送 DNS 查询请求并且接收服务器 IP 地址。然后浏览器使用服务器 IP 地址来初始化一个与服务器通信的连接。

在建立 TCP 连接之后哦，浏览器使用 HTTP GET 命令来请求来自服务器的网页。GET 命令含有浏览器请求的资源的 URL 以及浏览器想要用于事务处理的 HTTP 的版本。大多数情况下，浏览器使用 GET 请求发送相对的 URL，而不是完整的 URL，因为已经与服务器建立了连接：

GET /HTTP-HTML-WWW/ HTTP/1.1

一些其他的可选的 field:value 对可能跟在 GET 命令的后面，指定比如语言，浏览器类型，以及接受的文件类型等设置。

服务器响应由一个 header 以及更在 header 后面被请求的文档。响应 header 的格式如下：

HTTP/1.1 status_code reason-phase
field:value
field:value...

状态码是一个描述请求状态的三位数数字。状态码最左边的的数字标识大致的分类。100s表示通知，200s表示成功，300s指定重定向，400s表示客户端错误，500s表示服务器错误。

常见的 HTTP 状态码：

![https://i.quantuminit.com/fca3ee943d6c48fd.png](https://i.quantuminit.com/fca3ee943d6c48fd.png)

HTTP Header 字段例子：

![https://i.quantuminit.com/22cbfcb558824cc3.png](https://i.quantuminit.com/22cbfcb558824cc3.png)

早期的 HTTP 1.0 版本，每个请求/相应周期需要一个新的 TCP 连接。客户端打开一个连接并且初始化一个请求。服务器完成请求，然后关闭连接。在那种情况下，当服务器已经停止发送数据时，客户端能够知道，因为服务器关闭了 TCP 连接。这个过程需要频繁地打开和关闭连接。

HTTP 1.1 允许客户端和服务器维护比单次传输更长的连接。在那种情况下，客户读爱酱你需要某种方式来知道单次相应的关闭的时间。*Content-Length* 字段指定和 HTML 响应相关联的HTML 对象的长度。
如果服务器不知道发送的对象的长度——伴随着动态 HTML 不断常见的情况——服务器发送 header 字段 *Connection:close* 来通知浏览器服务器通过关闭链接来指定数据发送结束。 
HTTP 也支持服务器与客户端在一个协商阶段中达成确定的格式以及偏好选项的常见设置。

HTTP/2 的优点：

- 在一个连接中并行处理： HTTP 1.1 让浏览器打开多个同时的连接，但是类个连接一次只能响应一个请求。HTTP/2 允许服务器通过同样的连接立刻服务多个请求。
- Header 压缩：在新连接开始时传输 header 信息能够极大地减缓响应时间。压缩减少 header 数据的总大小。
- 服务器“推送”响应：服务器能够在等待一个请求之前给客户端提供内容。随着网站变得更加复杂，一个页面可能含有许多不同的对象，比如图形文件。服务器能够提前感知那些额外对象的请求并且直接推送内容，而不用等待每一项的请求。

HTTP/2 不需要加密但是强烈支持它并且给 TLS 加密提供了一个标准配置。

## Scripting

服务端脚本（server-side scripting）以及客户端脚本（client-side scripting）是自动生成Web代码的两种基本的技术。

### Server-Side Scripting

服务端脚本让服务器接收来自客户端的输入并且在后台处理该输入。这个过程如下描述：

1. 用户浏览一个含有用于购买商品或输入浏览者信息的表单。
2. 服务器基于用户选择生成表单并且传输表单到浏览器。
3. 用户网表单中输入必要的信息，并且浏览器传输表单会服务端。（注意 HTML 表单特性反转通常的过程。浏览器根据服务器要求服务器发送内容。）
4. 服务器接收来自浏览器的数据并且使用一个编程接口将数据传到能够处理用户信息的程序。如果用户是购买一个商品，后台的程序可能检查信用卡信息或者发送一个发货单到邮箱。如果用户是添加他/她的名字到一个邮件列表或加入一个有限制的在线站点，程序可能添加用户到数据库中。

![https://i.quantuminit.com/1c985e4326af40c5.svg](https://i.quantuminit.com/1c985e4326af40c5.svg)

多种编程语言以及环境已经能够帮助到开发者建立基于服务器的 web 应用程序。一种用于接入一个含有 web 页面的程序或脚本是通过通用网关接口（Common Gateway Interface，CGI）。CGI 是开发用于接收来自 web 用户的基于表单的输入，处理该输入，然后以 HTML 的形式生成输出。CGI 脚本通常使用 Perl 语言编写，但是 CGI 也能够与其他语言兼容，包括 C 语言。

PHP 作为 web 开发语言也变得日趋流行起来。一个简单的 PHP 脚本经常嵌入在一个 HTML 标签中：

`<?PHP code here... ?>`

或者使用 `<script language>` 标签：

`<script language="php"> code here ... </script>`

支持 PHP 解析以及执行尖括号之间代码的 web 服务器，插入 PHP 命令的输出来代替标签做为发送给客户端的页面。

Microsoft 的 Active Server Page (ASP) 以及后来的 ASP.NET 技术也是受到欢迎的服务端 web 技术。与定制的，服务端的应用程序的 web 交互的概念引导了整个的编程范式，又称为 web 服务环境。

### Client-Side Scripting

另一种与 web 环境集成脚本的方式是在客户短短上（也就是说，在运行 web 浏览器应用程序的本地客户机系统上）。然而服务端脚本在服务器上面执行并且脚本的输出嵌入在页面中，一个客户端的解决方案是将嵌入的脚本与余下的 HTML 代码以及文本一起传输到客户端，并且脚本由浏览器执行。

JavaScript 以及 VBScript 是两种常见的客户端脚本技术。脚本文件经常通过 HTML 来引用：

`<script src="/script.js" type="text/javascript"></script>`

这个标签引用实际的指令，或它能够引用一个包含代码指令的外部文件。在这两种情况中，这个技术只有当客户机支持在代码中引用的脚本语言才能正常工作。

对于特定应用程序类型，客户端脚本是一种更高效的选择。执行客户端上的脚本的解释器能够更详细地知道本地环境。还有，将交互式的元素放到客户端能够减少网络流量以及提高性能。

AJAX（有时候被视为 Asynchronous JavaScript and XML 的缩写）是一个使用客户端脚本来无缝更新 web 内容而不需要刷新整个页面的技术集合。另一种常见的客户端技术为用户提供交互选项，调用动画或其他多媒体特效，或是响应关于客户端系统状态的信息。

## Web Browsers

HTML 以及 HTTP 标准的目的是确保任何标准兼容的浏览器能够和任何标准兼容的服务器进行通信。一个具有完整特性的浏览器也必须提供数字签名以及证书的支持来允许用户参与到安全事务中，并且通过 Secure Sockets Layer/Transport Layer Security (SSL/TLS) 与 Hypertext Transfer Protocol Secure (HTTPS) 进行通信。

### The Semantic Web

深受 World Wide Web 创造者 Tim Berners-Lee 支持以及拥戴的 semantic web 被设计成使用真正的语义，就是说人类可理解的方式连接 web 数据的一种通用技术。换句话说，它的目标是用某种容易地访问的以及被计算机处理的方式编码 web 信息的含义。

### Resource Description Framework

Semantic Web 技术仍在实验性阶段。在 Web 社区中得到一定特定的关注的一个语义工具被称为 Resource Description Framework (RDF)。RDF 是一个用于表达关系的框架，这个关系能够暗示出含义。

RDF 的基本单元是一个由称为 a triple 的三部分构成的语句。A triple 像一个有一个主题，一个谓语以及一个对象的基本句子一样进行构造。

例如，在句子 "The play has the title A Streetcar Nmaed Desire"中，主题是 "The play"，对象是 "A Streetcar Named Desire"，以及谓语是 "has the title"。

RDF 能够承担几种形式，但是思想是每个元素都在一个 URI 中得到表达，并且 URIs 被连接在一个以冒号分隔的列表中。

### Microformats

RDFs 对于向文本添加含义是一个功能强大的工具。另一个可选的能工具，对于 从事 web 职业来说更加可管理和可使用的是 microformats 。

不像 RDFs，microformats 不会尝试去表示完整的句子结构以及语法结构。microformat 的目标是标记一个区域的文本来将其与一个预先定义的含义关联在一起，这样，一个浏览器或其他 web 应用程序浏览器站点时，会获取到关于该段文本的目的的线索。

一个 microformats 是一个特殊的 name/value 对的词汇，用于提供特定的意图。

更多的信息可以通过 [microformats.org](http://microformats.org/) 获取。随着 HTML5 的到来，一个相似的称为 `microdata` 的概念出现在正式的互联网字典中。

## XHTML

XHTML 标准致力于在过时的 HTML 以及基于 XML 的 web 环境之间搭建桥梁。XHTML 本质上是一个为了适应 XML 语法的 HTML 功能的表述。
像 XML schema 一样表示 HTML 的目的是为了给开发者建立脚本和能生成以及解释代码的其他程序提供灵活性。XHTML 对于小屏幕的移动设备来说，
收到 XHTML 页面的客户端应用程序通过更快地修改页面中的文本来适应更小的屏幕，从而使得不能在小屏幕上显示标准的 HTML 页面，通过 XHTML 显示出来。

## HTML5

HTML5 是一个新的 HTML 标准。许多人相信 HTML5 的出现能够降低 XHTML 的重要性。HTML5 标准含有许多设计用于支持移动设备浏览网页的特性。

HTML5 新的重要特性：

- 本地存储以及离线应用程序支持
- 绘图
- 音频以及视频嵌入
- 地理定位
- 语义标签元素

HTML5 也包含有其他改善的地方，例如托放 API 和更好的支持表单。开发者可以使用 HTML5 来开发出跨平台的跑在 Android，iPhone 以及其他平台并且仅需要小量地更改程序代码的移动端应用程序。、

### HTML5 Local Storage and Offline Application Support

Cookie 是 web 服务器存储在远端客户端机系统上的一小部分持久化数据。Web 服务器使用 cookies 来恢复之前的应用程序的状态或存储关于之前活动的信息。cookies 也是有限制的。
仅仅4 KB 大小的 cookie 不足以用于存储用户以及会话历史的数据。HTML5 存储（又称为 web 存储或 DOM 存储）让浏览器存储用于在 web 应用程序中定义的设置的值。本地存储提供了许多好处。例如，在客户端上执行脚本可以在存储区隐藏暂时的结果，因而通过减少与服务器通过网络进行通信的需要，从而提高了性能。存储空间也能够更加完整地存储当前会话状态的试图，
这样当服务器崩溃或者连接断开时，基于 web 的游戏 或其他交互式应用程序能够得以回复（在某些情况下，保持短暂的运行）。

HTML5 本地存储的能力引出了另一个重要的改善之处：离线应用程序支持。支持离线应用程序特性的一个 web 浏览器能够在网络连接断开时保持运行。支持离线处理的 web 应用程序含有命名文件以及用于离线运行的其他资源的 cache manifest。当网络连接恢复时，浏览器系统自动更新在本地系统的文件并且将所有的离线更改发送回服务器进行保存。

HTML5 离线存储的特性也有一些缺点。cookies 给广告商及内容提供商提供了跟踪用户习惯以及活动的方式。离线存储也能够导致额外的隐私担忧。

### HTML5 绘图

在 HTML5 版本之前，向网页中添加用户浏览网页时可以打开动画，需要通过第三方附件工具，比如 Adobe Flash。HTML5 通过两个重要的新元素来提供自己的绘图能力：用于位图图片的`<canvas>`，以及用于标量矢量图形（SVG）图片的`<svg>`。

`<canvas>`元素简单定义屏幕的一个区域做为可绘画的区域：

```html
<canvas id="picture1" width="350" height="250"></canvas>
```

web 开发者使用由 JavaScript 编写的绘图命令在这个 canvas 上进行绘画。定义中使用的 ID 为 JavaScript 通过 DOM 或 Document Object Model 定义关联的 canvas 定义创建一个标识符。DOM 被 web 程序员用来管理 website 中的对象。

标量图形是一种使用形状，线段以及几何元素而不是格子上的点来绘画的方式。HTML5 通过 `<svg>`元素提供标量图形。
因为标量图形图片由图形以及其他与限定的元素来构建，web 开发者以及浏览器厂商需要一个常见图形定义集合以及影响它们的形状和方向的参数。

`<svg>` 标签需要引用一个在绘画中使用到的图形元素的 XML 命名空间。World Wide Web 协会已经在 http://www.w3.org/2000/svg 中定义了自己的命名空间，其目的是用来做为 HTML5 标量图片的引用：


```html
<svg xmlns="http://www.w3.org/2000/svg"></svg>
```
关于图片的图形，大小，颜色以及方向的信息也能够放在`<svg>`元素的括号内。 浏览[标量矢量图片页面](http://www.w3.org/Graphics/SVG/)获取更多信息。

### HTML5 Embedded Audio and Video

在 HTML5 之前，标准的 web 规范并不提供嵌入视频的支持。HTML5 创造出了`<video>`标签来告诉浏览器这个标签引用的文件是是一个视频文件。
与此同时，HTML5 为引用的视频以及音频编码译码器提供直接的支持。（一个编码译码器基本上提供了用于解码一个多媒体文件的方法。）
如果浏览器知道文件是一个视频文件，并且访问了用于播放该文件的必要解码器，整个过程通过浏览器自身发生而不需要额外的第三方应用程序。


### HTML5 Geolocation

HTML5 的地理定位 API 为应用程序查询设备获取地理定位数据提供了标准的方式。程序可一使用地理定位 API 来决定设备的位置以及处理其他地理定位数据来标注坐标或找到附近服务的道路。

### HTML5 Semantics

HTML5 使用语义化的一种方式是通过一系列预先定义的给文本提供含义的 HTML 元素来实现。可以使用这些元素来标记用于特殊意图的文本。

HTML5 语义标签

|Element|Description|
|:----|:----|
|`<article>`|A section of text that can be broken off as a self-contained article|
|`<aside>`|A sidebar or other boxout with content that is related but separable from the main discussion|
|`<footer>`|Basic information on a section, such as the author name and related links|
|`<header>`|Introductory information and navigational information, such as a table of contents|
|`<hgroup>`|Heading for a section|
|`<mark>`|Text marked for reference|
|`<nav>`|A block of the page designed as a navigation tool, with general links to other parts of the document|
|`<section>`|A chapter or other thematic division of the document|
|`<time>`|A reference for a date or time|

其他包含在 HTML5 规范中重要的语义概念就是 **microdata**。Microdata 是 microformat 概念的扩展。不像 microformats，microdata 使用一系列的 name/value 对的形式。

## 小小的总结

- HTML5 的新特性：提供地理定位 (API)，语义标签，音频以及视频的嵌入标签 (audio/video标签)，本地存储以及离线应用程序支持 (cache manifest)，绘图 （canvas/svg）。
- HTML 中的语义化三种技术：RDF，microformat，microdata。
- 语义化网页是为了某些日子帮助 web 开发者编码语义信息，目的是自动化的过程会知道这个页面是关于什么事件/事物。
