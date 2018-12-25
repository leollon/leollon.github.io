---
title: Log about Nginx build with openssl 1.1.1 on Debian jessie
date: 2018-11-12 17:00:58
categories: Blog
tags: [Blog, openssl 1.1.1, Nginx, TLS 1.3v]
---

开始操作之前，之前已经使用`letsencrypt`提供的证书配置`Nginx`来支持`https` + `HTTP2`了。

是这样子的，之前我的小站已经使用`letsencrypt`提供数字加密证书来支持`https`了，然后呢，最近不是从v站上面也学来一些新姿(知)势(识)嘛,也就是`brotli` + `0-RTT(early data)` + `HTTP/2` + `server push` + `https`，使得网站加载速度提升嘛，所以呢也要对这一新技术进行尝鲜。因为之前网站已经是`HTTP/2` + `https` + `gzip`，所以呢这次主要工作就是`Nginx`弃用`gzip`，启用`brotli` + `0-RTT(early data)`。`brotli`是Google的开发并在`Github`上面开源的`Nginx`模块，名字是`ngx_brotli`，而`0-RTT(early data)`是`openssl 1.1.1`提供的`features`之一。

然后呢，之前vps上面部署本小站的时候，都是下面这样子按部就班安装`Nginx`的：

```bash
$ curl -fsSL http://nginx.org/keys/nginx_signing.key -o - | sudo apt-key add -
$ echo -e "deb http://nginx.org/packages/debian/ $(lsb_release -cs) nginx
deb-src http://nginx.org/packages/debian/ $(lsb_release -cs) nginx" | sudo tee /etc/apt/sources.list.d/nginx.list
$ sudo apt-get update && sudo apt-get install nginx
```

vps的系统使用的Linux发行版是`Debian8 x86_64`，然后这样子的方式呢，得到的nginx版本是stable version(1.14.1)的，但是`Debian8`的`openssl`版本是要低于1.1.1，并且`Nginx`也不支持`brotli`，同时还不支持在`Nginx`的配置文件中使用`ssl_early_data`，这个选项就是为了支持`0-RTT`的。那么如何得知不支持呢，[点这儿](https://trac.nginx.org/nginx/milestone/1.15)。

那么，从[这一链接](https://trac.nginx.org/nginx/milestone/1.15)得知，1.15才支持`0-RTT`，那么就需要安装mainline version的`Nginx`了，但是通过上面的方式是行不通了，因为`Debian8`上面的`openssl`版本是`OpenSSL 1.0.1t  3 May 2016`，但是如果不确定的服务器上面的是这个版本，可以执行如下命令得知:
```bash
$ openssl version
```

## 换另一种方式安装(手动编译以及安装)

### 更改`Nginx`源(source)

当时google一番，最后得到几个链接，但是没发现有`Debian8`的，可能是我的关键词没用对吧。但搜到[ubuntu 18.04](https://www.linuxbabe.com/ubuntu/enable-tls1-3-nginx-ubuntu-18-04-16-04)的也OK了，反正Ubuntu也是debian-like的嘛，那就用着吧。但需要更改`Nginx`的软件源：

```bash
$ echo -e "http://nginx.org/packages/mainline/debian/ $(lsb_release -cs) nginx
deb-src http://nginx.org/packages/mainline/debian/ $(lsb_release -cs) nginx" | sudo tee /etc/apt/sources.list.d/nginx.list
$ sudo apt-get update 
```

然后换一种方式安装`Nginx`，之前的安装方式也还是不支持`brotli`的，那么就需要编译安装了。

我没有完全跟着[ubuntu 18.04](https://www.linuxbabe.com/ubuntu/enable-tls1-3-nginx-ubuntu-18-04-16-04)上面的步骤，但也都还是大同小异吧。

我选择在自己的主目录下进行相关操作：
```bash
$ mkdir build && cd build
$ sudo apt-get source
$ ls -l
$ cd nginx-1.15.6 # writing this artile at that time
$ wget https://www.openssl.org/source/openssl-1.1.1.tar.gz  # download openssl
$ tar zxvf openssl-1.1.1.tar.gz  # extract openssl
$ mv openssl-1.1.1 openssl
$ git clone https://github.com/google/ngx_brotli.git
$ pwd
/home/Username/build/nginx-1.15.6/  # Username is your username.
$ vim debian/rules
```

在打开的文件里面第41行的末尾空格加上`/home/username/build/nginx-1.15.6/openssl --add-module=/home/username/build/nginx-1.15.6/ngx_brotli`，同理46行也加上，虽然那个选项是用于编译出调试版的`Nginx`的。

### 开始编译(compile)
这里对应`ubuntu 18.04`链接里面的第4步。

```bash
$ sudo dpkg-buildpackage -b
```

如果出现跟第四部中相同的错误，跟着做就是了，我当时也是有这个错误。按着这个做了，就编译通过了。

编译完了之后，就是安装了。在`buill`目录下面可以找到编译并且打包之后的安装包，`.deb`结尾的，类似这样子的`nginx_1.15.6-1~jessie_amd64.deb`，其中带`dbg`，是调试版本的`Nginx`。

### 安装、配置、重启

#### 安装(installation)

```bash
$ sudo apt-get remove nginx -y  # keep old configuration file，want to remove them, them use purge instead of remove.
$ sudo apt-get install nginx_1.15.6-1~jessie_amd64.deb
```

#### 配置(configuration)
[配置1](https://love4taylor.me/archives/nginx-openssl-dev-tls1.3.html)
[配置2, Nginx-doc](http://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_early_data)


#### 重启(restart)
```bash
$ sudo nginx -t && sudo nginx -t reload  # After execution, having a problem and displays later.
```

### 总结(Conclusion)
总结通过这样子的方法安装，还保留有原来的那种服务启动方式，而且还可以在相同服务器之间进行共享。


References:

- [Configuring-nginx-paths](https://docs.nginx.com/nginx/admin-guide/installing-nginx/installing-nginx-open-source/#configuring-nginx-paths)
- [Building Nginx from sources](https://nginx.org/en/docs/configure.html)
- [https://kn007.net/topics/my-nginx-compilation-tour/](https://kn007.net/topics/my-nginx-compilation-tour/]
- [https://www.linuxbabe.com/ubuntu/enable-tls1-3-nginx-ubuntu-18-04-16-04](https://www.linuxbabe.com/ubuntu/enable-tls1-3-nginx-ubuntu-18-04-16-04)
- [https://love4taylor.me/archives/nginx-openssl-dev-tls1.3.html](https://love4taylor.me/archives/nginx-openssl-dev-tls1.3.html)
