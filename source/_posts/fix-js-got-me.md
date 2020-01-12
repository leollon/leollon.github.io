---
title: js got me
date: 2018-08-04 12:42:38
categories: FrontEnd
tags: [前端, input, disabled, 事件委托]
---

## 踩坑记录


- `input`使用`disabled`属性`

   不是使用`ajax`提交表单数据。`input`标签通过`jQuery`增加`disabled`属性之后，并且使用了`$('selector').val('value')`更改其中的值，
   当提交表单后，后端取字段的值的时候取不到，返回来的页面的输入框周围一直显示红色并报错(该字段需要xxxx)。还尝试过`$('selector').attr('value', 'value')`以后，情况依旧。改`disabled`属性`readonly`之后，就可以了。

- 事件委托

   > 因为要在前端上面对一些数据进行多选，并将选择的数据都放入到一个`input`标签中，并用逗号分隔。反过来，从输入框中获取其中的数据，然后根据里面的数据勾选`checkbox`。

   问题来了，通过`ajax`获取回来`json`数据，然后渲染到前端页面上，最终显示的结果是生成多个`checkbox`，然后当鼠标勾选对应的`checkbox`时候，将`checkbox`上面绑定的值放入到`input`标签中，但情况是这样子并不成功。但是通过使用浏览器开发工具中的`console`可以达到想要的效果。
   刚开始的时候的代码是：
   ```js
   $('.selector input').click(function () {
       // do something
   })
   ```
   使用时间委托之后的代码:
   ```js
   $('body').on('click', '.tickets input', function () {
       // do something again as above
   })
   ```

