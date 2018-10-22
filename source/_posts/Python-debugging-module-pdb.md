---
title: Python-debugging-module-pdb
date: 2017-11-20 10:34:39
categories: Python
tags: [Python, notes, bug, debug, pdb]
---


# 命令行执行
```bash
$ python -m pdb script.py
```

# 从脚本内部执行

```python
import pdb

def test_pdb():
    pdb.set_trace()
    print("I am test_pdb function")

print(test_pdb())
```

将上面的代码保存为python脚本文件并运行，会马上进入debugger模式。

debugger模式下的一些命令列表：
  - c: 继续执行
  - w: 显示当前正在执行的的代码行的上下文信息
  - a: 打印当前函数的参数列表
  - s: 执行当且代码行，并停在第一个能停的地方(相当于单步进入)
  - n: 继续执行到当前函数的下一行，或者当前行直接返回(单步跳过)




更多参考 
1. [Python2-pdb文档](https://docs.python.org/2/library/pdb.html)
2. [python3-pdb文档](https://docs.python.org/3/library/pdb.html)