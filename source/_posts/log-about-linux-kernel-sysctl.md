---
title: log-about-linux-kernel-s-sysctl
date: 2018-12-04 16:45:57
categories: Linux
tags: [Linux, 系统性能调优]
---
## vfs_cache_pressure
------------------
这个参数用于控制内核回收缓存目录和索引节点对象的频率。

它的默认值是100，相对于页缓存和交换缓存回收，内核将尝试以一种相对快的频率试图回收目录条目(dentries)和inode。降低`vfs_cache_pressure`使得内核更偏向于保存目录条目(dentry)和inode。当`vfs_cache_pressure`为0的时候，由于内存压力的原因，系统内核将不回收目录数据结构和索引信息节点，这很容易就导致内存不足的情况。提高`vfs_cache_pressure`到100以上，将会导致内核提高回收目录条目(dentry)和inode的频率，这可能对系统性能产生负面的影响。回收代码需要耗费多种锁去查找可释放的目录条目(dentry)和inode对象。如果`vfs_cache_pressure`=1000，这将会查找比现有的可释放对象多十倍。


## swappiness
------------------
它是一个百分比数值，这个内核参数用来控制内核交换内存页的数量。数值越高，交换的数量就越多。
数值越低，会减少交换的数量。等于0的时候，等于告诉内核直到自由的文件备份页的数量少于某一数值的时候，才开始使用交换分区。它的默认的值是60。


## 总结：

   - 虚拟文件缓存系统(vfs)，影响交换区的性能，百分比越高，将会提高内存回收频率。数值越低，说明内存回收频率越低，不需要频繁查询内存，寻找自由的目录数据结构和索引节点对象，系统性能相对要好，但是有可能会导致内存不足的情况发生。

   - 交换分区，控制系统内核如何使用交换分区，百分比越低，则系统内核更多的使用内存，使用交换分区的数量就会比较少，反之，则内核使用交换分区相对更多。可以说，交换区位于内存和磁盘中间一层，起到暂存区的作用，当内存不够用的时候，内存中的数据可以暂时放置到这个交换分区中。

References:
- [https://wiki.archlinux.org/index.php/Swap#VFS_cache_pressure](https://wiki.archlinux.org/index.php/Swap#VFS_cache_pressure)
- [https://wiki.archlinux.org/index.php/Swap#Swappiness](https://wiki.archlinux.org/index.php/Swap#Swappiness)
- [https://www.kernel.org/doc/Documentation/sysctl/vm.txt](https://www.kernel.org/doc/Documentation/sysctl/vm.txt)
- [https://doc.opensuse.org/documentation/leap/tuning/html/book.sle.tuning/cha.tuning.memory.html#cha.tuning.memory.vm.reclaim](https://doc.opensuse.org/documentation/leap/tuning/html/book.sle.tuning/cha.tuning.memory.html#cha.tuning.memory.vm.reclaim)
