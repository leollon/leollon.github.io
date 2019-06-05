---
title: the art of command line 笔记整理
date: 2019-06-02 12:39:02
categories: Notes
tags: [bash, notes, shell]
---

- `man` 用来阅读命令的官方文档， 比如 `man` `man`

  命令的分区号
  ![分区号](https://i.quantuminit.com/eed7d1556634417e.png)
  - 1 普通命令
  - 5 文件/约定
  - 8 管理命令
- help` 和 `help -d`可得知一个命令是否是可执行程序。`shell`内置命令或者别名，使用`type command`，例如 `fg`, `bg`, `jobs`等。
- `curl cheat.sh/command`会给出如何使用shell命令常用例子的表单。

重定向
  - `>`: 覆盖输出
  - `<`: 输入
  - `|`: 管道
  - `>>`: 末尾添加

一些标点符号
  - `*`：通配符
  - `?`：单个字符匹配符号
  区别：
    - `"`：双引号
    - `'`：单引号

任务管理
  - `&`
  - ctrl-z
  - ctrl-c
  - `jobs`
  - `fg`
  - `bg`
  - `kill`

ssh
  - `ssh-agent`
  - `ssh-add`

基本文件管理
  - `ls -l`, `ls`
  - `less`, `less +F`
  - `head`
  - `tail`, `tail -f`
  - `ln -s`
  - `chown`
  - `chomd`
  - `du`, `du -sh *`, 磁盘使用情况
  - `df`， `mount`， `fdisk`, `mkfs`, `lsblk`,用于文件系统管理，inode相关，`ls -i`，`df -i`。
  - `ip`, `ifconfig`, `dig`, `traceroute`, `route`, 网络管理
  - `git`,版本控制系统
  - `grep`，`egrep`，以及它们的`-i`, `-o`, `-v`, `-A`, `-B`, `-c`选项。
  - `apt-get`, `yum`, `dnf`, `pacman`，各个linux发行版的包管理工具，`pip`，`Python`包管理命令

## 日常使用
  - 在`bash`中使用**Tab**进行命令行参数的补全或列出所有可供使用的命令。使用**ctrl-r**进行搜索历史命令，搜索期间使用该按键组合进行循环搜索，直到找到匹配的命令行，按下**Enter**键，或使用键盘上右箭头按键将命令行放入
  当前行来进行编辑。
  - 在`bash`中，**ctrl-w**组合删除最后一个单词，**ctrl-u**删除从光标开始处到当前行开头。逐个单词移动使用**alt-b** 和 **alt-f**。**ctrl-e**移动光标到行尾。**ctrl-k**删除从光标到行为的内容，**ctrl-l(L)**进行清屏，`man readline`查看所有`bash`的所有的默认绑定按键。例如**alt-.**查找命令行的最后一个的参数，**alt-\****展开和通配符匹配的所有可能。
  - `set -o vi`使用vi风格的按键绑定, `set -o emacs`重置为原来的按键绑定。
  - 在设置编辑器之后（例如，`export EDITOR=vim`），对于编辑长的命令，**ctrl-x** **ctrl-e**打开文本编辑器用来编辑当前多行命令。如果绑定了`vi`风格的按键，则使用`escape-v`按键组合。。
  - 查看最近的命令`history`。`!n`再次执行该行命令(n为行号)，`!$`最后一条命令的最后一个参数，`!!`执行最后一条命令。`man history`查看"HISTORY EXPANSION"。然而总是经常使用**ctrl-r** 和 **alt-.**来代替这些操作。
  - 使用`cd`切换到home目录。访问相对于使用`~`前缀（`~/.bashrc`）的home目录下的文件。在`sh`脚本中，使用`$HOME`引用home目录。
  - 使用`cd -`切换之前的目录。
  - 使用**alt-#** 或 **ctrl-a, #, Enter**在命令的开头添加注释，将正输入或要执行的命令注释掉。
  - 使用`xargs`或 （`parallel`）。功能非常的强大。它可以控制每行执行项数（-L），`-I{}`也很实用。举些例子：
    ```shell
    $ find /usr/lib/python3/ -name "*.py" | xargs grep "codecs"  # 查询包含codecs的相关文件
    $ cat hosts | xargs -I{} ssh root@{} hostname
    ```
  - `pstreee -p` 显示进程树。
  - `pgrep`和`pkill`根据名字查找进程或给进程发信号（-f 很实用）。
  - 给进程发各种信号。例如，`kill -STOP pid`，`man 7 signal`查看完整的信号参数。
  - 使用`nohup`或`disown`使程序不停地在后台运行。
  - 使用`netstat -lntp` 或 `ss -plat` 或 `lsof -iTCP -sTCP:LISTEN -P -n`（同样适用于macOS） 检查哪些进程正在监听端口（`-t`用于TCP，`-u`用于UDP）。
  - 使用`lsof` 和 `fuser`查看打开的套接字和文件。
  - 使用`uptime` 和 `w` 查看系统已经运行多长时间。
  - 使用`alias`给经常使用的的命令创建快捷方式。例如，`alias ll=ls -latr` 创建新的别名`ll`。
  - 将常用的别名，shell设置以及函数放到`~/.bashrc`中，并且[登录shell自动导入它](http://superuser.com/a/183980/7106)。这样在所有的的shell会话中都能够使得这些设置能够生效。
  - 将登录的时候需要执行的环境变量和命令放置在`~/.bash_profile`中。需要将从图形环境登录和`cron`任务启动的shell的配置进行单独配置。
  - 在许多计算机之间使用Git同步配置文件（例如，`。bashrc` 和 `.bash_profile`）.
  - 变量名和文件名包含空白字符的时候，需要特别注意。使用`-0` 或 `-print0`选项输出空字符来分隔文件名字。例如，`locate -0 pattern | xargs -0 ls -al` 或 `find / -0 print0 -type d | xargs -0 ls -al`。为了在循环中迭代文件名包含空格的文件，使用`IFS=$'\n'`设置IFS为换行。
  - Bash脚本中，使用`set -x`（或 `set -v`，它可以记录原来的输入，包含了不能扩展开的变量和注释）进行调试输出。最好设置`set -e`在错误时终止执行（非0退出代码）。`set -u`检测未设置的变量。`set -o pipefail` 在管道中产生错误是终止执行。在联系紧密的脚本中，一旦遇到EXIT 或者 ERR时，使用`trap`。在脚本中发生错误时，进行检测并终止执行和打印信息。
  ```bash
  set -euo pipefail
  trap "echo 'error: Script failed: see failed command above'" ERR
  ```
  - bash脚本中，子shells（使用括号编写的）方便组织命令。普遍的例子就是临时切换到不同的工作目录，例如:
  ```shell
  # 在当前目录工作
  (cd /some/other/dir && other command)
  # 继续在原来的目录
  ```
  - `${variable:?error message}`用于检测变量的存在，例如，脚本中的单个参数，可以这样子写`input_file=${1:?usage: $0 input_file}`。设置一个变量的默认值`${name:-default}`。给上一个例子添加额外(可选的)的参数，可以这样子`output_file=${2:-logfile}`。如果省略`$2`并且是空的，那么output_file将设置为`logfile`。算术运算：`i=$(( (i + 1) % 5 ))`。序列：{1..10}。字符串裁剪：`${var%后缀}` 和 `${var#前缀}`，例如，`var=bar.pdf`, `echo ${bar%.pdf}.txt`则输出`foo.txt`。
  - 花括号展开`{`...`}`可以减少重新输入相似的文本并且自动进行组合。比如，`mv foo.{txt.pdf} some-dir`（移动这两个文件），`cp somefile{,.bak}`（展开成`cp somefile somefile.bak`）或 `mkdir -p test-{a,b,c}/subtest-{1,2,3}`(展开所有可能的组合并且创建目录树)，花括号优先于比其他展开而展开。
  - 展开顺序：花括号;波浪号展开，参数和变量展开，算术运算展开，和命令替换(从左到右进行);单词分割;以及文件名展开。（例如，像这样子`{1..20}`的一个区间不能用这样子的变量`${$a..$b}`来表达。使用`seq` 或 一个`for`循环来代替，例如，`seq $a $b` 或 `for((i=a; i<=b; i++)); ...; done`）
  - 一行命令的输出可以通过使用`<(some command)`（目前已知的进程替换）将其像文件一样来对待。例如，对比本地的`/etc/hosts`和远程服务器上面的那个文件：
  ```shell
  $ diff /etc/hosts < (ssh somehost cat /etc/hosts)
  ```
  -编写脚本时，可将所有的代码放入花括号内。如果缺少右花括号，由于语法错误，脚本将不能被执行。当脚本从网页上下载东西的时候，这样子就会很有用，这样子可以阻止脚本只下载一部分。
  ```shell
  {
        # 需要放进来的代码

  }
  ```
  - "here doccument" 允许[重定向多行输入](https://www.tldp.org/LDP/abs/html/here-docs.html)，就像来自一个文件一样。
  ```shell
  $ cat << EOF
  input
  on multiple lines
  EOF
  ```
  - 使用`some-command > logfile 2>&1` 或 `some-commmand &> logfile`将在Bash中的标准输出和标准错误重定向到相同文件中。经常为了保证命令不退出打开的文件到标准输入，在你登录的终端中尝试使用它，增加`</dev/null`也是一个很好的实践。
  - 使用`man ascii` 查看ASCII表，里面包含十六进制和十进制的数值。可通过`man unicode`，`man utf-8` 和 `man latin1`来了解通用编码信息。
  - 使用`screen` 或`tmux` 来提供多窗口，尤其在远程ssh会话中，它可以分离和重新连接会话。`byobu`可以通过提供更多的信息和更简易的管理功能来增强`screen` 或`tmux`。`dtach`是只用于会话持久的另一个更轻量的工具。
  - 在ssh中，知道使用`-L` 或 `-D`（偶尔`-R`）来建立隧道，例如，通过隧道访问来自远程服务器的网页。
  - 优化ssh连接的配置。例如，`~/.ssh/config`这个文件包含了避免在某些网络环境中断开连接的设置，使用压缩（这在低带宽链接中使用scp时非常有用）和 使用本地控制文件提供多信道给相同的服务器：
      ```
      TCPKeepAlive=yes
      ServerAliveInterval=15
      ServerAliveCountMax=6
      Compression=yes
      ControlMaster auto
      ControlPath /tmp/%r@%h:%p
      ControlPersist yes
      ```
  - 应该考虑打开跟ssh安全相关的其他选项，例如，每个子网或主机或在可信任的网络中：`StrictHoskKeyChecking=no`, `ForwardAgent=yes`。
  - 将使用UDP协议的[`mosh`](https://mosh.mit.edu/)作为ssh的另一种选择，它避免的掉链接和增加了通信过程中的便利（需要服务端配置）。
  - 获取一个文件权限的八进制形式，在系统配置的时候很有帮助，但是在`ls`中并不可用并且，比如：
  ```shell
  stat -c '%A %a %n' /etc/timezone
  ```
  - 交互式选择来自另一个命令输出的值，使用[`percol`](https://github.com/mooz/percol) 或 [`fzf`](https://github.com/junegunn/fzf)。
  - 使用`fpp`（[PathPicker](https://github.com/facebook/PathPicker)）与基于其他命令(`git`)输出的文件进行交互。
  - `python -m SimpleHTTPServer 7777` (端口是 7777 并且是Python 2) 和 `python -m http.server 7777`（端口 7777 并且是Python 3）列出当前目录（和子目录）中的所有文件的简易web服务器，用于提供给网络上的任何人。
  - 使用`sudo`以其他用户运行命令。默认的用户是root；用`-u` 选项指定用户。用`-i`登录该用户（会被询问登录密码）。
  - 使用`su username` 或 `su - username`切换shell到另外一个用户。后面有`-`表示获取一个环境，就好像另一个用户刚刚登录过。省略username将默认为root。切换过程中，将要求输入切换到该用户的密码。
  - 命令行的[128K 限制](https://wiki.debian.org/CommonErrorMessages/ArgumentListTooLong)。当时用通配符匹配大量文件的时候，“Argument list too long” 错误是普遍存在的。（此时应该考虑使用`find` 或 `xargs`来提供帮助。）
  - 简单的简单计算器（当然得访问Python），使用`python`计时器。也可以用使用`bc`来替换。例如，使用Python：
  ```shell
  >>> 2 + 3
  5
  ```
# 处理文件与数据
  - `find . -iname "*something*"` 在当前目录查找文件。`locate something` 更具名字在任何地方查找文件（但是要记得的是`updatedb`可能没有对最见创建的文件建立索引）。
  - 搜索源文件或者数据文件，比`grep -r` 更高级或更快的搜索工具，按旧到新进行排序[`ack`](https://github.com/beyondgrep/ack2)，[`ag`](https://github.com/ggreer/the_silver_searcher)("the silver searcher")，和 [`rg`](https://github.com/BurntSushi/ripgrep)(ripgrep)。
  - `lynx -dump` 将HTML转换成text。
  - Markdown，HTML，和各种各样的文档转换，尝试使用[`pandoc`](http://pandoc.org/)。`pandoc READ.md --from markdown -- to docx -temp.docx`将Markdown文档转换成word文档。
  - `xmlstarlet时代旧了些，但还是能很好处理xml的软件。`
  - JSON文件，使用[`jq`](http://stedolan.github.io/jq/), [`jid`](https://github.com/simeji/jid) 和 [`jiq`](https://github.com/fiatjaf/jiq)可用与交互式的使用。
  - [`shyaml`](https://github.com/0k/shyaml)处理YAML。
  - 对于Excel 或 CSV 文件，[`csvkit`]()提供了`in2csv`， `csvcut`，`csvjoin`，`csvgrep`等等。
  - 对于Amazon S3，[`secmd`](https://github.com/s3tools/s3cmd)具有便捷型并且`s4cmd`更快。亚马逊的[`aws`](https://github.com/aws/aws-cli)和优化过的[`saws`](https://github.com/donnemartin/saws)是用于处理其他与AWS相关连的任务必不可少的工具。
  - 知道`sort` 和 `uniq`，包括uniq的`-u` 和 `-d`选项。还有`comm`。
  - 知道使用`cut`，`paste`和`join`来操作文本文件。大多数都知道`cut`但却不知`join`。
  - 知道`wc`计算行数，字符数（`-l`），单词书（`-w`），字节数（`-c`)。
  - `tee`将标准输入输出到文件和标准输出中，如`ls -al | tee file.txt`。
  - [`datamash`](https://www.gnu.org/software/datamash/)可用于更复杂的计算，包括分组，翻转字段，以及统计计算。
  -
  知道语言本地化以微妙的方式影响者命令行的结果，其中包括了排序顺序和性能。大多数Linux按转都会设置`LANG`或者其他语言本地化变量来本地化设置，比如美国英文。但是要知道如果更改语言本地化排序结果将会发生改变。并且知道i18n惯例使得排序或者其他命令运行速度慢许多倍。在某种情况下（比如下面的设置操作或唯一化操作）你可以完全安全地忽略掉慢速的i18n惯例并且使用传统的按照字节进行排序，终端中输入`export
  LC_ALL=C`。
  - 在调用命令时通过使用前缀环境变量的方式设置指定命令的环境，比如，`TZ=Pacific/fiji date`。
  - 知道`awk` 和 `sed` 基本的简单数据的更改。
  ```shell
  ps -ef | grep -ir "nginx" | awk '{print $2}' # 打印出所有的nginx进程号
  ```
  - 在一个或多个文件中，原地替换所有重复的字符串：
  ```shell
  perl -pi.bak -e 's/old/new/g' file*.txt
  ```
  - 使用[`repren`](https://github.com/jlevy/repren)重命名多个文件或搜索和在文件中替换。（在一些情况下`rename`也可以提供多重命名，但是得注意并不是所有的Linux发行版都提供同样的功能。）
  ```shell
  # 全部重命名文件名，目录以及文件内容 foo 重命名为 bar：
  repren --full --preserve-case --from foo --to bar .
  # 从备份文件还原回来
  repren --renames --from '(.*)\.bak' --to '\1' *.bak
  # 和上面一样，使用重命名，如果可用的话：
  rename 's/\.bak$//' *.bak
  ```
  - 正如man page所言，`rsync`很快并且是非常优秀的多文件拷贝工具。因可以在机器之间进行同步而得名但也可用于本地同步。当有安全限制的时候，使用`rsync`而不是`scp`，它支持恢复传输而不需要重新开始。也是删除大量文件[最快的方式](https://web.archive.org/web/20130929001850/http://linuxnote.net/jianingy/en/linux/a-fast-way-to-remove-huge-number-of-files.html)：
  ```shell
  mkdir empty && rsync -r --delete empty/ some-dir && rmdir some-dir
  ```
  - 显示文件处理的进度，可用`pv`，`pycp`，`progress`，`rsync --progress`，或使用`dd status=progross`进行块级别的复制。
  - 使用`shuf`打乱或随机选择来自文件的内容行。
  - 知道`sorg`选选项。对于数字，使用`-n`，或 `-h` 来处理成人类可读的数字（比如，`du -h`）。知道`-t` 和 `-k`。特别注意只通过第一个字段进行排序时，使用的`-k1,1`；`-k1`的意思是根据正行进行排序。稳定排序(`sort -s`)可能有用处。例如，根据第二个字段进行排序，然后根据字段1，可以这样子写`sort -k1,1 | sort -k2,2`。
  - 如果需要在bash的命令行中输入制表符（例如，`sort`的`-t`参数），可以这么输入**ctrl-v [Tab]**或者写入`$'\t'`（后面的更好一些就好像复制/粘贴一样）。
  - 给源文件进行打补丁标准的工具是`diff` 和 `patch`。`diffstat`是一个diff的汇总统计工具，`sdiff`并排进行比较。注意`diff -r`用户整个目录。`diff -r tree1 tree2 | diffstat` 汇总变化。使用`vimdiff`来比较文件并进行编辑。
 ```shell
  echo -e 'halo\nhello' > file1
  echo -e 'halo' > file2
  diff file1 file2 > diff_fil1_file2
  patch file1 file2
  diffstat diff_file1_file2
  sdiff file1 file2
  ```
  - 二进制文件，`hd`，`hexdump`，`xxd`用来简单导出十六进制，`bvi`，`hexedit`，`biew`用于二进制编辑。
  - `strings`（加上`grep`，等等）可用于找到二进制文件中的文本位。
  - 比较二进制（增量压缩），使用`xdelta3`。
  - 使用`iconv`转换文本编码。或更高级的使用，可以选择`uconv`。它支持一些高级的Unicode。例如：
  ```shell
  # 显示十六进制编码哦人字符的实际名字（对于调试能够起到帮助）：
  uconv -f utf-8 -t utf-8 -x '::Any-Hex;' < input.txt
  uconv -f utf-8 -t utf-8 -x '::Any-name;' < input.txt
  # 转换成小写并且一处所有的发音（通过扩大和丢弃）：
  uconv -f utf-8 -t utf-8 -x '::Any-Lower; ::Any-NFD; [:Nonspacing Mark:] >; ::Any-nFC;' < input.txt > output.txt
  ```
  - 进行文件进行分割成小部分，使用`split`（根据大小）和 `csplit`（根据一种模式）。
  - 日期与时间：按照[ISO 8601](https://en.wikipedia.org/wiki/ISO_8601)获取当前日期和时间的，可以这样子`date -u +"%Y-%m-%dT%H:%M:%SZ"`（其他选项是[有](https://stackoverflow.com/questions/7216358/date-command-on-os-x-doesnt-have-iso-8601-i-option)[问题](https://unix.stackexchange.com/questions/164826/date-command-iso-8601-option)的）。使用`dateutils`的`dateadd`，`datediff`，`strptime`等等来操作日期和时间表达式。
  - 使用`zless`，`zmore`，`zcat` 和 `zgrep`来操作压缩的文件。
  - 通过`chatrr`设置文件的属性并且提供更低级的选择给文件权限。例如，阻止文件的误删除：`sudo chattr -i /path/to/file/or/directory`。
  - 使用`getacl` 和 `setfacl` 来保存和恢复文件权限。例如:
  ```shell
  getfacl -R /some/path > permissions.txt
  setfacl --restore=permissions.txt
  ```
  - `truncate`（创建[稀疏文件](https://en.wikipedia.org/wiki/Sparse_file)）,`fallocate`（ext4, xfs, btrfs 和 ocfs2 文件系统），`xfs_mkfile`（几乎任何文件系统，存在于xfsprogs包中），`mkfile`（在类似于Solaris，Mac OS的Unix-like系统中）。

# 系统调试
  - 网页调试，`curl` 和 `curl -I`很便捷，或与之相匹配的`wget`，或者更加新颖的[`httpie`](https://github.com/jkbrzt/httpie)。
  - 获取当前cpu/disk状态，经典的工具是`top`（或者更好`htop`），`iostat`，和 `iotop`。使用`iostat -mxz 15`获取基本的CPU和详细化的每个分区的磁盘状态和性能观察。
  - 网络连接详细信息，使用`netstat` 和 `ss`
  - 快速预览系统现在运行的情况，`dstat`能够提供很大的帮助。更为详细的系统运行情况，使用[`glances`](https://github.com/nicolargo/glances)。
  - 获取内存状态，运行并理解`free` 和 `vmstat` 的输出。注意这个“cached”的值是被Linux内核像文件缓存一样占有的内存数量，因此这个有效的内存计数是“free”对应的值。
  - Java系统调试是一个不同的，但关于Oracle和其他Java虚拟机的一些简单的技巧是你可以跑`kill -3 <pid>`并且一个完整的栈跟踪和堆概况（包含分代垃圾回收细节，这些细节非常有用）将会导出到标准错误（stderr）/日志（logs）。JDK的`jps`，`jstat`，`jstack`，`jmap`都能够提供帮助。[JSK 工具集](https://github.com/aragozin/jvm-tools)更高级。
  - 使用[`mtr`](http://www.bitwizard.nl/mtr/)作为更好的路由追踪工具，可以识别出网络问题。
  - 查找磁盘满了的原因，比起像`du -sh *`这样的常用的命令，`ncdu`能够节省很多时间。
  - 查看那个socket或进程占用带宽，使用[]`iftop`](http://www.ex-parrot.com/~pdw/iftop/) 和 [`nethogs`](https://github.com/raboof/nethogs)。
  - `ab`(与Apache一起的)工具对于web服务器性能压力测试非常有帮助。更高级的负载测试，可尝试使用`siege`。
  - 更多严重的网络调试，使用`wireshark`，`tshark`，或 `ngrep`。
  - 知道`strace` 和 `ltrace`。如果一个程序执行失败，挂起，崩溃并且不知道原因，或者想知道程序的性能，可以使用它们来提供帮助。注意测试选项（`-c`），并且能够分离一个运行中的进程（`-p`）。使用追踪子选项（`-f`）避免丢失重要的调用。
  - 使用`ldd`检查共享库等等——但[不能用来跑未被信任的文件](http://www.catonmat.net/blog/ldd-arbitrary-code-execution/)。
  - 知道如何使用`gdb`连接一个进程并且获取它的栈跟踪。
  - 使用`/proc`，当调试线上问题的时候，它可以无与伦比的帮助。例如：`/proc/cpuinfo`，`/proc/meminfo`，`/proc/cmdline`，`/proc/xxx/exe`，`/proc/xxx/fd/`，`/proc/xxx/smaps`（xxx是进程id或pid）。
  - 当调试过去发生的错误的时候，[`sar`]()可以提供很有用的帮助。它能展示关于CPU，内存，网络等等的历史数据。
  - 更深层次的系统性能分析，查看`stap`（SystemTap），[`perf`](https://en.wikipedia.org/wiki/Perf_%28Linux%29) 和 [`sysdig`](https://github.com/draios/sysdig)。
  - 检查操作系统相关信息，使用`uname` 或 `uname -a`（通常是Unix/内核信息）或者 `lsb_release -a`（Linux发行版信息）。
  - 无论什么时候，发生什么奇怪的事情时（可能是硬件或驱动的问题），使用`dmesg`。
  - 如果删除了一个文件并且没有释放由`du`显示的预期磁盘空间，检查这个文件是否正在被一个进程使用：`lsof | grep deleted | grep "filename-of-my-big-file"`
