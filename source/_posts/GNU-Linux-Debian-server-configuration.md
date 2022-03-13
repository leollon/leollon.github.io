---
title: GNU/Linux-Debian-server-configuration
date: 2018-01-12 22:53:36
categories: Linux
tags: [Debian, vps, configuration, GNU/Linux]
---

前几天买了个小鸡，坐标位于中欧的一个小国家吧。
服务器装完系统后，ssh上去，装些在服务器上自己基本会使用到的工具的时候，就产生下面的提示了。

## What?
--------
```
errors:

Can't set locale; make sure $LC_* and $LANG are correct!
perl: warning: Setting locale failed.
perl: warning: Please check that your locale settings:
	LANGUAGE = "en_US:en",
	LC_ALL = (unset),
	LC_TIME = "en_HK.UTF-8",
	LC_MONETARY = "en_HK.UTF-8",
	LC_CTYPE = "en_US.UTF-8",
	LC_ADDRESS = "en_HK.UTF-8",
	LC_TELEPHONE = "en_HK.UTF-8",
	LC_NAME = "en_HK.UTF-8",
	LC_MEASUREMENT = "en_HK.UTF-8",
	LC_IDENTIFICATION = "en_HK.UTF-8",
	LC_NUMERIC = "en_HK.UTF-8",
	LC_PAPER = "en_HK.UTF-8",
	LANG = "en_US.UTF-8"
    are supported and installed on your system.
perl: warning: Falling back to a fallback locale ("en_US.UTF-8").
locale: Cannot set LC_ALL to default locale: No such file or directory
```

## How
------

因为我使用的shell是`zsh`，所以我是这么设置的：
当然如果使用的shell是`bash`，只需要替换`.zshrc` 为`.bashrc`即可。
fix:
```
  $ echo -e "export LC_ALL=en_US.UTF-8\nexport LANGUAGE=en_US.UTF-8 >> .zshrc
```

完了之后

```bash
$ sudo locale
$ sudo dpkg-reconfigure locales
# 这里配置一下系统环境的使用语言，以及字符类型。
```

