---
title: command ln and man
date: 2017-11-16 16:34:22
category: Notes
tags: [Linux, ln, man]
---

# ln

ln：创建硬链接或软链接

使用语法：
1. 创建硬链接
  ```bash
  $ ln <source_file/dir> <link>
  ```
  ```bash
  $ echo 'hello' > a.txt
  $ ln a.txt a
  $ ls -l ./
  -rw-rw-r-- 2 monkey monkey    6 Nov 16 17:01 a
  -rw-rw-r-- 2 monkey monkey    6 Nov 16 17:01 a.txt
  ```
  上面的2 是目录条目

  硬链接局限性：

    1. 硬连接不能引用自身文件系统之外的文件。也就是说，链接不能引用与该文件不
       在同一磁盘分区中的文件。
    2. 硬连接无法引用目录

    如果删除了源文件，链接文件依旧是可用的，但是链接文件的目录条目减1。

2. 创建符号链接/软链接
   它是包含了指向引用的文件或目录的一个文本指针。
   ```bash
   $ ln -s <source_file/dir> <link>
   ```

   解决了硬链接的局限性
   如果删除了它引用的文件，链接文件的内容就为空，是损坏状态的。

# man
  man: 查看程序手册或帮助文档
  语法：
  > man section_number keyword
  
|section number| description |
|:----|:----:|
| 1 | 用户命令 |
| 2 | 内核系统调用的程序接口 |
| 3 | C库函数程序接口 |
| 4 | 特殊文件 |
| 5 | 文件格式 |
| 6 | 游戏和娱乐|
| 7 | 其他杂项 |
| 8 | 系统管理命令 |

显示隐藏文件夹/文件
```bash
$ ls -d \.[[:alpha:]] # 仅显示隐藏的文件夹
$ ls \.[[:alpha:]] # 显示所有隐藏文件以及隐藏文件夹
```