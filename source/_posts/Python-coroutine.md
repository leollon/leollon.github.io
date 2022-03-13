---
title: Python-coroutine
date: 2018-10-28 23:33:00
categories: Python
tags: [notes, Python, coroutine]
---

> 协程是指一个过程，这个过程与调用方协作，产出由调用方提供的值。

使用`.send(value)`向生成器发送数据，发送的数据会成为生成器函数中`yield`表达式的值。

生成器中给`return`语句提供返回值，不再报`SyntaxError`异常了。

使用`yield from`可以把复杂的生成器重构成小型的嵌套生成器。


### 简单协程

```Python
import inspect


def simple_coroutine():
    print('-> coroutine started.')
    x = yield
    print('-> coroutine received: ', x)

my_coro = simple_coroutine() # generator created
next(my_coro) # start generator and suspend generator
# my_coro.send(None) # start generator and suspend generator
my_coro.send(233) # generator running
# generator closed

print(inspect.getgeneratorstate()) # GEN_CLOSED
```

调用`send()`之前，生成器处于

协程四状态:
  - 'GEN_CREATED'

    等待执行
  - 'GEN_RUNNING'

    解释器在执行。只有在多线程中才能看到该状态。
  - 'GEN_SUSPENDED'

    在yield表达式处暂停
  - GEN_CLOSED'

    执行结束

### 预激协程的装饰器

```
from functools import wraps

def coroutine(func):
    @wraps(func)
    def primer(*args, **kwargs):
        gen = func(*args, **kwargs)
        next(gen)
        return gen
    return primer

@coroutine
def averager():
    total = 0.0
    count = 0
    average = None
    while True:
        term = yield average
        count += 1
        average = total / count

coro = avarager() # generator created,primer and suspended
coro.send(10)
```

> 使用`yield from`调用协程，会自动预激。`Python3.4`标准库里的`async.coroutine`装饰器不会预激协程，因为能兼容`yield from`句法。

### 终止协程和异常处理

> 协程中未处理的异常会往上冒泡，传给`next`函数或`send`方法的调用方(触发协程的调用方)。

终止协程的一种方式: 发送某个哨符值，让协程退出。内置的`None`和`Ellipsis`等常量经常用作哨符值。`Ellipsis `的有点是，数据流不太经常有这个值。还可以使用`StopIteration`作为哨符值。像这样子使用，`coro.send(StopIteration)`。

`generator.throw(exec_type[, exec_value[, traceback]])`

致使生成器在暂停的`yield`表达式处抛出指定异常。

`generator.close()`

致使生成器在暂停的`yield`表达式处抛出`GeneratorExit`异常。

> `Python 3.3` 引入 `yield from` 结构的主要原因之一与把异常传入嵌套的协程有关。另一个原因是让协程更方便地返回值。

### 使用yield from

在生成器`gen`中使用`yield from subgen()`时，subgen会获得控制权，把产出的值传给`gen`调用方，即调用方可以直接控制`subgen`。与此同时，`gen`会阻塞，等待`subgen`终止。

```Python
def gen():
    for c in 'AB':
        yield c
    for i in range(1, 3):
        yield from i

list(gen())  # ['A', 'B', 1, 2]
```

改写

```Python
def gen():
    # parent gen
    yield from 'AB' # subgen
    yield from range(1, 3) # subgen

list(gen())  # ['A', 'B', 1, 2]
```


事件驱动框架(如`Tornado` 和 `asyncio`)的运作方式：在单线程中使用一个主循环驱动写成执行并发活动。使用写成做面向事件编程是，协程或不断把控制权让步给主循环，激活并向前运行其他协程，从而执行各个并发活动。
