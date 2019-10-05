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

## 使用asyncio和aiohttp下载

```python
"""Download flags of top 20 countries by population
Sequential version
Sample run::
    $ python3 flags.py
    BD BR CD CN DE EG ET FR ID IN IR JP MX NG PH PK RU TR US VN
    20 flags downloaded in 5.49s
"""
# BEGIN FLAGS_PY
import os
import time
import sys

import requests  # <1>

POP20_CC = ('CN IN US ID BR PK NG BD RU JP '
            'MX PH VN ET EG DE IR TR CD FR').split()  # <2>

BASE_URL = 'http://flupy.org/data/flags'  # <3>

DEST_DIR = 'downloads/'  # <4>


def save_flag(img, filename):  # <5>
    path = os.path.join(DEST_DIR, filename)
    with open(path, 'wb') as fp:
        fp.write(img)


def get_flag(cc):  # <6>
    url = '{}/{cc}/{cc}.gif'.format(BASE_URL, cc=cc.lower())
    resp = requests.get(url)
    return resp.content


def show(text):  # <7>
    print(text, end=' ')
    sys.stdout.flush()


def download_many(cc_list):  # <8>
    for cc in sorted(cc_list):  # <9>
        image = get_flag(cc)
        show(cc)
        save_flag(image, cc.lower() + '.gif')

    return len(cc_list)


def main(func):  # <10>
    t0 = time.time()
    count = func(POP20_CC)
    elapsed = time.time() - t0
    msg = '\n{} flags downloaded in {:.2f}s'
    print(msg.format(count, elapsed))


if __name__ == '__main__':
    main(func=download_many)
# END FLAGS_PY
```

```python
#!/usr/bin/env python3
'''using asyncio
'''
import asyncio
import aiohttp

from flags import BASE_URL, save_flag, show, main

async def get_flag(cc):
    url = '{}/{cc}/{cc}.gif'.format(BASE_URL, cc=cc.lower())
    async with aiohttp.request('GET', url) as resp:
        # https://github.com/aio-libs/aiohttp/issues/4065
        image = await resp.read()
    return image


async def download_one(cc):
    image = await get_flag(cc)
    show(cc)
    save_flag(image, cc.lower() + '.gif')
    return cc

def download_many(cc_list):
    loop = asyncio.get_event_loop()
    to_do = [download_one(cc) for cc in sorted(cc_list)]
    wait_coro = asyncio.wait(to_do)
    res, _ = loop.run_until_complete(wait_coro)
    loop.close()
    return len(res)

if __name__ == '__main__':
    main(func=download_many)
```


