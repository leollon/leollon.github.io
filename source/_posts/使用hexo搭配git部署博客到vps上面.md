---
title: 使用hexo搭配git部署博客到vps上面
date: 2017-08-05 01:52:30
categories: blog
tags: [hexo,git,blog]
---


参考了几篇博客，然后我就开始搭建了，最后我就总结一下自己在这过程中做了什么事情吧。

本教程的服务器使用Debian 8 64bit 的[vultr vps](http://www.vultr.com/?ref=7109674)，客户端使用Ubuntu 16.04 64bit进行动手实践。

> https, http2

<!-- more -->

开始之前搭建之前准备的东西：
   1. vps(Debian 8)
   2. Linux基本命令行的使用
      > man, useradd, passwd, usermod, mkdir, touch, cd, tar, xz, ln, vim
   3. hexo-cli
   4. git
   5. node, npm
   6. xz-utlis


## 客户端(本地)
   1. 下载并安装node
   ```bash
   $ sudo apt-get install xz-utlis
   $ wget https://nodejs.org/dist/v6.11.2/node-v6.11.2-linux-x64.tar.xz
   $ xz -d node-v6.11.2-linux-x64.tar.xz
   $ tar -xvf node-v6.11.2-linux-x64.tar
   $ mv node-v6.11.2-linux-x64 node_v6
   $ sudo mv node_v6 /usr/bin/
   $ cd /usr/bin/
   $ sudo ln -s ./node_v6/bin/node ./bin/node
   $ sudo ln -s ./node_v6/bin/npm ./npm
   ```
   2. 安装hexo
   ```bash
   $ sudo npm instal -g hexo-cli
   ```
   这里我遇到一个坑要说明一下，就是装完之后，当我要执行命令行`hexo init <folder>`时候，居然`zsh: command not found: hexo`，然后我`which hexo`了一下，也没有，后来我`npm list`了一下，确实是有装上这个hexo-cli。这时候，我就想到在当前的`$PATh`里面没有`hexo`，所以此时就需要一个软链接到这个`hexo-cli`了：
   ```bash
   $ cd /usr/bin/
   $ sudo ln -s /usr/bin/node_v6/lib/node_modules/hexo-cli/bin/hexo ./hexo
   ```

   3. 一些包的安装

   ```bash
   $ npm install hexo-deployer-git --save
   $ npm install hexo-server
   ```

   4. 好了，这时候，我们就可以开始写博客了，不过也还只是在本地跑给自己看的而已。
   ```bash
   $ hexo init blog
   $ hexo new [layout] "文件名"  # 不指定layout，则根据theme使用默认的
   $ hexo new page about # 解决浏览器访问出现 404 Not Found
   $ hexo g
   $ hexo server
   ```
   这时候可以看到终端是这样子的效果：

   ![hexo-server](https://wx4.sinaimg.cn/mw690/7372620bgy1fib2z2wgyxj20hv01q747.jpg)

   浏览器打开网站是这个样子的：

   ![localhost](https://wx1.sinaimg.cn/mw690/7372620bgy1fib2z66rwjj21200jzqdl.jpg)

   解决浏览器出现 403 Forbidden:

   ```bash
   $ hexo new page tags
   $ vim source/tags/index.md
   ```

   在title的下一行加入下面内容：

   ```
   type: "tags"
   ```

   解决浏览器访问出现 403 Forbidden

   ```bash
   $ hexo new page categories
   $ vim source/categories/index.md
   ```

   在title的下一行加入下面内容：
   ```
   type: "categories"
   ```

## 服务器端(vps)
   使用root用户登录vps，并且使用的是密钥登录。具体怎么使用密钥登录系统，搜索引擎一搜出来一堆。

   以下步骤，除需要之外，我都是在**root**用户下执行。所以要**谨慎使用`rm -rf`**等命令。

   1. git用户管理
      1. 添加git用户
         ```bash
         $ apt-get install sudo # 有些服务器需要自己安装
         $ adduser git # root用户登录服务器，增加git用户, 记住此时输入的密码
         $ passwd git  # root用户登录服务器，设置git用户的密码
         $ usermod -aG sudo git # 将git用户添加到sudo用户组
         ```
      2. 上传公钥到`/home/git/.ssh/authorized_keys`。
         ```bash
         $ mkdir -p /home/git/.ssh/
         $ cp ~/.ssh/authorized_keys /home/git/.ssh/
         $ chmod 600 /home/git/.ssh/authorize_keys
         $ chmod 700 /home/git/.ssh
         ```
         本地客户端用git用户ssh连接服务器测试一下：
         ```bash
         $ ssh -p 22 git@45.23.32.89
         ```

   2. 创建博客的根目录，并将其归为git用户，以及git用户组所有

      ```bash
      $ mkdir -p /var/www/blog
      $ chown git:git blog
      ```

   3. 生成git裸仓库，并配置git hooks
     ```bash
     $ su git
     $ cd ~
     $ git init --bare blog.git
     $ vim blog.git/hooks/post-receive
     ```

     添加如下内容，并保存：

     ```bash
     #!/bin/bash
     GIT_REPO=/home/git/blog.git #git仓库
     TMP_GIT_CLONE=/tmp/blog
     PUBLIC_WWW=/var/www/blog #网站目录
     rm -rf ${TMP_GIT_CLONE}
     git clone $GIT_REPO $TMP_GIT_CLONE
     rm -rf ${PUBLIC_WWW}/*
     cp -rf ${TMP_GIT_CLONE}/* ${PUBLIC_WWW}

     git --work-tree=/var/www/blog --git-dir=/home/git/blog.git checkout -f
     ```

    然后更改这个文件的权限：
    ```bash
    $ chomd +x blog.git/hooks/post-receive
    $ exit # 退出git用户
    ```
   4. 安装nginx服务器，并且启动nginx

     ```bash
     $ wget -q -O - https://nginx.org/keys/nginx_signing.key | apt-key add -
     $ apt-get update && apt-get install -y nginx
     $ service nginx start
     ```

     浏览器输入服务器的ip地址进行访问，测试nginx是否已经运行：

     ![nginx](https://wx1.sinaimg.cn/mw690/7372620bgy1fib2yz2ibxj20fv07faao.jpg)

   5. 更改nginx配置

     ```bash
     $ cp /etc/nginx/conf.d/default.conf /etc/nginx/conf.d/default.conf.bak
     $ vim /etc/nginx/conf.d/default.conf
     ```

     修改内容如下：
     ```
    server {
        listen 80;
        server_name i.cthee.cyou;

        access_log  /home/git/log/nginx/host.access.log  main;
        error_log  /home/git/log/nginx/host.error.log  info;

        location / {
            root   /var/www/blog;  # 网站根目录
            if (-f $request_filename) {
                rewrite ^/(.*)$ /$1 break;
            }
        }

        location ~* ^.+\.(ico|git|jpg|jpeg|png)$ {
            root /var/www/blog;
            access_log off;
            expires 1d;
        }

        location ~* ^.+\.(css|js|txt|xml)$ {
            root /var/www/blog;
            access_log off;
            expires 10m;
        }
    }
    ```

## 客户端(本地)的最后配置
   1. 更改_config.yml, 自动部署到git服务器上面
      ```
      deploy:
        type: git
        repo: git@<server_ip>:/home/git/blog.git
        branch: master
        message: Site updated:{{ now('YYYY-MM-DD HH:mm:ss') }}
      ```
   2. 由于我更改了服务器的ssh端口，所以还需要配置一下ssh的端口。所以自动化部署的时候出现的错误描述：
      >Error: ssh: connect to host ×××.×××.×××.××× port ××××: Connection timed out
      fatal: Could not read from remote repository.

      ```bash
      $ vim ~/.ssh/config
      ```

     将下面内容复制进去更改为服务器上面的配置并保存

     ```
     Host <server_ip>
     HostName <server_ip>
     port <server_port>
     PreferredAuthentications publickey
     IdentityFile ~/.ssh/id_rsa
     ```

**参考过的文章：**

   1. [开启https](https://wrleskovec.com/2016/03/18/hexo-nginx-setup/)
   2. [启用http2](https://yq.aliyun.com/articles/7171)
   2. [letsencrypt](https://gist.github.com/xrstf/581981008b6be0d2224f)
   3. [https](https://www.embbnux.com/2015/12/29/letsencrypt_with_nginx_config_for_wordpress/)
   4. [free-certificates-lets-encrypt-and-nginx](https://www.nginx.com/blog/free-certificates-lets-encrypt-and-nginx/)
   5. [http://tiktoking.github.io/2016/01/26/hexo/](http://tiktoking.github.io/2016/01/26/hexo/)
   6. [Deploy Hexo Blog to VPS |部署Hexo博客到VPS](https://blog.peiyingchi.com/2017/03/20/deploy-hexo-blog-to-VPS/)
