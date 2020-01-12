---
title: 用flask做一个图床吧
date: 2017-08-07 10:36:02
categories: picbed
tags: [picbed, 图床, imgbed]
---


本来想用七牛云的作为图床并且支持https外链的，迫于国内域名要备案，才可以在七牛云上面绑定自己的域名，同时自己也不想备案。

现考虑使用flask写一个自用的图床。
<!-- more -->

TODO List

- [x] 图片异步上传
- [x] 显示图片链接相关代码
- [x] 图片https外链使用Let's Encrypt提供证书支持https
- [x] 文件删除

-----


2018-09-21 00:33:15 坑填好了

## [点我体验一下](https://picbed.quantuminit.com/)
## [点我去看一下代码](https://github.com/leollon/yet-another-image-bed)

--EOF--
