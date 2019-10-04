---
title: asyncio 小小的实践笔记
date: 2019-10-04 12:53:36
categories: Fluent-Python
tags: [Python, Notes, asyncio]
---

本文将对Python的`asyncio`进行小小的实践后的一丢丢笔记。

`asyncio`包，使用事件循环驱动的协程实现并发。

## 线程与协程对比

```python
#!/usr/bin/env python3
'''using thread
'''
import threading
import itertools
import time
import sys


class Signal:
    go = True

def spin(msg, signal):
    write, flush = sys.stdout.write, sys.stdout.flush
    for char in itertools.cycle('|/-\\'): #  infinite loop
        status = char + ' ' + msg
        write(status)
        flush()
        write('\x08' * len(status)) #  使用退格符将光标移回来
        time.sleep(0.15)
        if not signal.go:
            break
    write(' ' * len(status) + '\x08' * len(status))


def slow_function():
    # pretend to wait for I/O for a while
    time.sleep(3)
    return 42


def supervisor():
    signal = Signal()
    spinner = threading.Thread(target=spin,
        args=('thinking!', signal))
    print('spinner object:', spinner)
    spinner.start()
    result = slow_function()
    signal.go = False  # send message to subordinate thread, so as to terminate slave thread.
    spinner.join()
    return result


def main():
    result = supervisor()
    print('Answer:', result)


if __name__ == '__main__':
    main()
```

```python
#!/usr/bin/env python3
'''using asyncio
'''
import asyncio
import itertools
import sys

async def spin(msg):
    write, flush = sys.stdout.write, sys.stdout.flush
    for char in itertools.cycle('|/-\\'):
        status = char + ' ' + msg
        write(status)
        flush()
        write('\b' * len(status))
        try:
            await asyncio.sleep(.15)
        except asyncio.CancelledError:
            break
    write(' ' * len(status) + '\b' * len(status))


async def slow_function():
    # coroutine function
    # pretend to wait for I/O for a while
    await asyncio.sleep(3)  # Block for delay seconds
    return 42


async def supervisor():
    spinner = asyncio.create_task(coro=spin('thinking!'))  # Return a task object. drive the coroutine.
    print('spinner object:', spinner)
    result = await slow_function()
    spinner.cancel()  # Request the Task to be cancelled.
    return result


def main():
    loop = asyncio.get_event_loop()
    result = loop.run_until_complete(supervisor())
    loop.close()
    print('Answer:', result)


if __name__ == "__main__":
    main()
```
