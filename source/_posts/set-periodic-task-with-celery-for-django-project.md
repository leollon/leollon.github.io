---
title: set-periodic-task-with-celery-for-django-project
date: 2019-07-16 13:57:16
categories: django
tags: [django, celery, Python, periodic_task]
---



## The first way

add sample code like below in the `app_name/tasks.py`:

```Python
from celery.schedules import crontab
from mysite import celery_app  # celery app defined module


@celery_app.on_after_finalize.connect
def setup_periodic_task(sender, **kwargs):

    sender.add_periodic_task(
        schedule=crontab(minute="*/1"),  # 任务日程，每分钟执行一次
        sig=debug_periodic_task.s("[Debug periodic task]"),  # 信号处理函数
        args=("[Debug periodic task]",)  # 必须是一个元组，如果只有一个元素在元组中，得在这个元素后面加上逗号。
        name="debug periodic task",
    )


@celery_app.task
def debug_periodic_task(args):
    print(args)
```

## The second way

步骤一，创建像下面的代码，放入到`app_name/tasks.py`：
```Python
from celery import shared_task


@shared_task
def debug_periodic_task(args):
    print(args)

```

步骤二，在`proj/settings.py`文件中添加

```Python
from celery.schedules import crontab

CELERY_BEAT_SCHEDULE = {
    "debug-periodic-task": {
        "task": "app_name.tasks.debug_periodic_task",
        "schedule": crontab(minute="*/1"),
        "args": ("[Periodic task]",),
    },
}

```
