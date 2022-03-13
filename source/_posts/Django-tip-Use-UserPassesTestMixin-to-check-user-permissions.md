---
title: Django tip -- Use UserPassesTestMixin to check user permissions
date: 2018-10-25 21:52:23
categories: Python
tags: [django,tips,用户权限验证,Python]
---

更为优雅一点地进行用户权限验证。

例如，在博客中要更新文章内容的时候，总要验证一些权限，不然数据库中的数据容易容易出儿。

假设原先博客中用户权限的判断的代码有如下两种方式：

### 方式一

*在获取/或更新文章的时候直接判断。Yikes(哎呀)，这貌似就是业务逻辑了呀，现在终于知道什么是写业务了。*

```Python
class UpdateArticleView(LoginRequiredMixin, UpdateView):
    """View function for editing a posted article
    """
    # ...
    # 省略部分代码
    raise_exception = True  # call handler403

    def get(self, request, *args, **kwargs):
        response = super(UpdateArticleView, self).get(request, *args, **kwargs)
        if request.user.is_superuser or request.user == self.object.author:
            # 这里验证要更新内容的文章的作者和当前登录用户是否是同一个人，或则当前的用户是否是网站的超级管理员
            return response
        return self.handle_no_permission()

    def post(self, request, *args, **kwargs):
        article = Article.objects.get(slug=kwargs.get('slug', None))
        if request.user.is_superuser or request.user == article.author:
            # 这里验证要更新内容的文章的作者和当前登录用户是否是同一个人，或则当前的用户是否是网站的超级管理员
            super(UpdateArticleView, self).post(request, *args, **kwargs)
            return HttpResponseRedirect(reverse("articles:manage"))
        return self.handle_no_permission()

```

### 方式二

*覆盖(override)`LoginRequiredMixin` 中的`dispatch`方法*

```Python
class UpdateArticleView(LoginRequiredMixin, UpdateView):
    """View function for editing a posted article
    """
    # ...
    # 省略部分代码
    raise_exception = True  # call handler403

    def dispatch(self, request, *args, **kwargs):
        if user.is_superuser or request.user == self.object.user:
            return super(UpdateArticleView, self).dispatch(request, *args, **kwargs)
        return self.handle_no_permission()

    def get(self, request, *args, **kwargs):
        return super(UpdateArticleView, self).get(request, *args, **kwargs)

    def post(self, request, *args, **kwargs):
        super(UpdateArticleView, self).post(request, *args, **kwargs)
        return HttpResponseRedirect(reverse("articles:manage"))
```

### 方式三

更为的优雅的方式。

*继承`Django`中的`UserPassesTestMixin`类，实现该类中的`test_func`

```Python
class UpdateArticleView(LoginRequiredMixin, UserPassesTestMixin， UpdateView):
    """View function for editing a posted article
    """
    # ...
    # 省略部分代码
    raise_exception = True  # call handler403

    def test_func(self):
        article = self.get_object()
        return self.request.user == article.author

    def get(self, request, *args, **kwargs):
        return super(UpdateArticleView, self).get(request, *args, **kwargs)

    def post(self, request, *args, **kwargs):
        super(UpdateArticleView, self).post(request, *args, **kwargs)
        return HttpResponseRedirect(reverse("articles:manage"))
```

方式三，看起来是不是更为优雅一些。嘻嘻