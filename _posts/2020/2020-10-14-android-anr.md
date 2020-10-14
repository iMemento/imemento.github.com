---
layout: post
title: Android ANR
categories:
- Android
tags:
- Android

---

ANR全称：`Application Not Responding`，也就是应用程序无响应。
logcat 定位不到具体哪个线程导致 ANR，可以通过 ANR 日志分析来定位问题。

当app出现ANR时会在data/anr/目录下生成traces.txt日志文件。每次发生ANR时都会删除旧的traces文件，重新创建新文件。Android只保留最后一次发生ANR时的信息。
可以使用adb命令导出traces文件：
```
adb pull /data/anr/traces.txt
```

默认导出文件路径在 adb 命令行运行的当前的目录。

一般trace文件顶部的线程即为ANR的原因。

--EOF--						