---
layout: post
title: "python限时输入"
date: 2013-01-26 14:49
comments: true
categories: [python, 计算机]
keywords: python, 限时输入
---
<!--more-->
使用threading模块实现限时输入：
``` python input_with_time_limit.py
#!/usr/bin/env python
import time
import threading
import os
 
 
def exit(msg):
    print(msg)
    os._exit(1)
 
 
def timer(limit):
    time.sleep(limit)
    exit("\n%s秒已过，超时 !" % (limit))
 
 
def put(limit=30):
    t = threading.Thread(target=timer, args=(limit, ))
    try :
        t.start()
        words = input("你有%s秒输入：" % (limit))
    except KeyboardInterrupt :
        pass
    exit("你输入了%s" % (words))
```
