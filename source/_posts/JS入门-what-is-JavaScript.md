---
title: JS入门 - what is JavaScript
date: 2017-08-28 23:44:42
categories: JavaScript
tags: [JavaScript, notes]
---

HTML 和 CSS 是标记语言。标记语言用于描述和定义文档中的元素。JavaScript 是编程语言。
编程语言用于向机器发出指令。编程语言可用于控制机器的行为和表达算法。

开发者工具通常用作沙盒，可以随意在上面运行任意代码。

<!--more-->

`console.log`用于向JavaScript控制台显示内容。

```JavaScript
console.log("hiya friend!");
```

将控制台当作沙盒，运行JavaScript代码进行演示

第一段代码： 更改第一个标签中文字的颜色为红色

```JavaScript
document.getElementsByTagName("h1")[0].style.color = "#ff0000";  # 更改h1标签中文字的颜色为红色
```

第二段代码： 鼠标点击页面中的任何个地方，在一个标签下面增加元素

```JavaScript
document.body.addEventListener('click', function () {
     var myParent = document.getElementById("Banner");
     var myImage = document.createElement("img");
     myImage.src = 'https://thecatapi.com/api/images/get?format=src&type=gif';
     myParent.appendChild(myImage);
     myImage.style.marginLeft = "160px";
});
```
