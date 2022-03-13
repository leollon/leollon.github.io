---
title: fix-hexo-todo-list-checkbox
date: 2017-08-08 22:52:45
categories: blog
tags: [hexo, bug]
---

## TODO List 的checkbox渲染以后，页面的中的复选框可以通过鼠标点击

<!-- more -->

上github上面看了关于这个feature的一圈issue，说是marked不支持这个feature。还是不服，就又继续找。

又搜到这篇[博客](http://zhoupq.com/%E7%94%A8-HTML-%E6%A0%87%E7%AD%BE%E5%AE%9E%E7%8E%B0-MarkDown-Task-List/)的启发，
然后后面直接按照那上面的去嵌入了html的代码，当然渲染出来的结果也是自己想要的了。

嵌入的代码如下面一样：

```html
<input type="checkbox" checked="checked" disabled="disabled"> Done
<input type="checkbox" disabled="disabled"> Undo
```
经过hexo渲染处理后，效果也达到自己想要的了：

<input type="checkbox" checked="checked" disabled="disabled"> Done

<input type="checkbox" disabled="disabled"> Undo

后来，不知道又浏览到如下链接：

[hexo-renderer-marked](https://github.com/litten/hexo-theme-yilia/issues/519#issuecomment-303847742)

自己也按照上面的步骤做了，依旧没有产生应有的效果。

继续查找，当浏览到下面链接的时候：

[Add Markdown List Support](https://github.com/hexojs/hexo-renderer-marked/pull/32)

[hexo-renderer-marked-bug](https://github.com/wkzcn/hexo-renderer-marked/blob/Fix-checklist-issue/lib/renderer.js#L25)

去看了一下源码，偶然见看到如下代码，宛如打开了新世界的大门：

```javascript
text = text.replace(/^\s*(<p>\s*)?\[ \]\s*/i, '<input type="checkbox"></input> ').replace(/^\s*(<p>\s*)?\[x\]\s*/i, '<input type="checkbox" checked></input> ');a
```
这不正是todo list的正则表达式吗？再结合之前通过嵌入html代码的方法。所以我修改了本地的`node_module/hexo-renderer-marked/lib/renderer.js`一点点代码：

```javascript
text = text.replace(/^\s*(<p>\s*)?\[ \]\s*/i, '<input type="checkbox" disabled></input> ').replace(/^\s*(<p>\s*)?\[x\]\s*/i, '<input type="checkbox" checked disabled></input> ');a
```

当我这么改了这行代码以后，再使用hexo重新渲染处理所有页面以后。
It do works！！
It do works！！
It do works！！

后面也给这个repository提了[pr](https://github.com/hexojs/hexo-renderer-marked/pull/46)，自己认为这是一个bug，不知道作者会不会通过，毕竟第一次给开源项目的代码提pr。

Update: 2017-08-08

捂脸。。。选错分支提pr了，不应该选master提pr的。本来有一个Fix-checklist-issue， 我是基于这个分支添加的一点点代码，所以应该选这个branch提pr才对。

大概是今早，看到自己第一次可以给开源项目提pr内心有点小小的激动，同时也是有一点不太相信自己的感觉。提pr的时候，忘记看我自己开的分支与master分支的package.json不一样了，然后就start a pull request了。

Update: 2017-08-09 8:04 AM

一早醒来，就去看了昨晚再一次提的pr，作者给回复了:

>Just wonder why need to disable the check box? Even user ticked it, after refresh, it will be set back to the original.

那么我想这应该是没有必要的吧，随后我就close pull request了。不被merge，还是有点小失望吧，不过也没所谓，毕竟可能这个还是太简单了。那就自用好了。

说一下自己的想法吧，就是想让这个todo list，经过hexo渲染以后，在浏览器浏览这个页面时，用户可以看到这是一个task table. 像下面的效果一样：

task table

- [x] Done
- [ ] Undo

Update: 2017-08-12 00:16:40

作者给开了我之前关闭了的pr，等着用户来讨论是否需要这个功能，然后再决定是否merge或者是加上这个feature吧。
