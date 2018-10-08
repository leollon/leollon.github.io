---
title: '[Ubuntu 16.04] - build python3.6.4 from source code'
date: 2018-01-18 10:43:08
category: log
tags: [Ubuntu 16.04, Python3.6.4, build-Python3.6.4, Debian8]
---

刚开始我是按常规的编译安装方法：
```bash
1. $ ./configure --prefix=/usr/local/
2. $ ./configure --enable-optimizations # 这个是上一步结束给的提示，索性也就运行这条命令，意思大概说的就是开启优化之类的
3. $ make -j 8   # 开八个线程进行编译，超快，慢就慢在一些test， test example高达400+项。
4. $ sudo make install
```
安装完后，在python3.6 console里面`import ssl`的时候，就会找不到对应的模块。
在编译完之后，还有提示其他模块没有找到的，比如`readline`啥。这些在开始test之前都可以找得到。
下面的方法都能够解决。


下面是推荐的做法
## 先尝试更新升级Ubuntu
```bash
$ sudo apt update
$ sudo apt upgrade -y
```
## 安装build需要的工具链接
```bash
$ sudo apt install -y build-essential # gcc, g++, make等
```
## 安装build Python需要的
这里也是解决一些模块没有找到的问题。
```bash
$ sudo apt install libssl-dev zlib1g-dev libncurses5-dev libncursesw5-dev libreadline-dev libsqlite3-dev
$ sudo apt install libgdbm-dev libdb5.3-dev libbz2-dev libexpat1-dev liblzma-dev tk-dev
```

# Debian 8
**注意事项**

Ubuntu 跳过这一步。

因为我的小鸡(vps)使用的Linux发行版是Debian8，而Debian8的默认的Python最高版本是3.4.2。

贴一下这台小鸡的配置
```
CPU model            : Intel(R) Xeon(R) CPU E5-2650L v4 @ 1.70GHz
Number of cores      : 1
CPU frequency        : 1699.998 MHz
Total size of Disk   : 18.5 GB (4.2 GB Used)
Total amount of Mem  : 984 MB (912 MB Used)
Total amount of Swap : 1903 MB (3 MB Used)
System uptime        : 0 days, 6 hour 29 min
Load average         : 0.00, 0.00, 0.05
OS                   : Debian GNU/Linux 8
Arch                 : x86_64 (64 Bit)
Kernel               : 4.14.0-041400rc8-generic
```
配置也就这样子了。
目前Python的最新版本已经是3.6.4了，而我又不想升级Debian9,因此也就只能自己动手丰衣足食，手动编译安装了。
至于为什么有这一步呢，是因为，我在部署基于Python3.6.4并用Django(1.11.7)写的blog的时候，
`pip install -r requirements`到了最后一步的之后，总是提示诸如
1. Python-3.6.4/Modules/csv.gcda:Cannot Open
2. Python-3.6.4/Modules/fcntlmodule.gcda Cannot open
....
然后Google也没有结果。
当时的错误还有几项，但是浏览器中历史记录只有这样子两条记录了。
所以就自己尝试通过下面的方法给解决了。


192行-252行改成下面这样子。

```
$ vim Modules/Setup.dist
```

```bash
fcntl fcntlmodule.c     # fcntl(2) and ioctl(2)
spwd spwdmodule.c               # spwd(3)
grp grpmodule.c         # grp(3)
select selectmodule.c   # select(2); not on ancient System V

# Memory-mapped files (also works on Win32).
mmap mmapmodule.c

# CSV file helper
_csv _csv.c

# Socket module helper for socket(2)
_socket socketmodule.c

# Socket module helper for SSL support; you must comment out the other
# socket line above, and possibly edit the SSL variable:
#SSL=/usr/local/ssl
#_ssl _ssl.c \
#       -DUSE_SSL -I$(SSL)/include -I$(SSL)/include/openssl \
#       -L$(SSL)/lib -lssl -lcrypto

# The crypt module is now disabled by default because it breaks builds
# on many systems (where -lcrypt is needed), e.g. Linux (I believe).
#
# First, look at Setup.config; configure may have set this for you.

_crypt _cryptmodule.c # -lcrypt # crypt(3); needs -lcrypt on some systems


# Some more UNIX dependent modules -- off by default, since these
# are not supported by all UNIX systems:

#nis nismodule.c -lnsl  # Sun yellow pages -- not everywhere
#termios termios.c      # Steen Lumholt's termios module
#resource resource.c    # Jeremy Hylton's rlimit interface

#_posixsubprocess _posixsubprocess.c  # POSIX subprocess module helper

# Multimedia modules -- off by default.
# These don't work for 64-bit platforms!!!
# #993173 says audioop works on 64-bit platforms, though.
# These represent audio samples or images as strings:

#audioop audioop.c      # Operations on audio samples


# Note that the _md5 and _sha modules are normally only built if the
# system does not have the OpenSSL libs containing an optimized version.

# The _md5 module implements the RSA Data Security, Inc. MD5
# Message-Digest Algorithm, described in RFC 1321.

_md5 md5module.c


# The _sha module implements the SHA checksum algorithms.
# (NIST's Secure Hash Algorithms.)
_sha1 sha1module.c
_sha256 sha256module.c
_sha512 sha512module.c
#_sha3 _sha3/sha3module.c
```


## build Python3.6.4

```bash
$ ./configure --enable-optimizations
$ make -j 8 # 开八个jobs进行编译快一些
$ sudo make altinstall
```


### References

[Building Python 3.6 from source on Ubuntu and Debian Linux](https://solarianprogrammer.com/2017/06/30/building-python-ubuntu-wsl-debian/)

[How to Compile and Install Python with OpenSSL Support?](https://techglimpse.com/install-python-openssl-support-tutorial/)


