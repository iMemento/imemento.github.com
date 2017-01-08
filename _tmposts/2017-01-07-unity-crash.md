---
layout: post
title: Unity3d Android crash error 'Fatal signal 11 (SIGSEGV), code 1'
categories:
- Unity3d
tags:
- Unity3d

---
项目一切都是OK的，接入某渠道SDK后，打开app 就 crash，人啊，这一辈子注定要遇到几个 'SIGSEGV'，摊手。
{% highlight objc %}
01-05 18:48:12.124: A/libc(727): Fatal signal 11 (SIGSEGV), code 1, fault addr 0x0 in tid 776 (UnityMain)
01-05 18:48:12.124: E/libEGL(727): call to OpenGL ES API with no current context (logged once per thread)
01-05 18:48:12.234: A/DEBUG(533): *** *** *** *** *** *** *** *** *** *** *** *** *** *** *** ***
01-05 18:48:12.234: A/DEBUG(533): Build fingerprint: 'samsung/c5pltezc/c5pltechn:6.0.1/MMB29M/C5000ZCU1APJ4:user/release-keys'
01-05 18:48:12.234: A/DEBUG(533): Revision: '10'
01-05 18:48:12.234: A/DEBUG(533): ABI: 'arm'
01-05 18:48:12.234: A/DEBUG(533): pid: 727, tid: 776, name: UnityMain  >>> com.kongzhong.c1.uc <<<
01-05 18:48:12.234: A/DEBUG(533): signal 11 (SIGSEGV), code 1 (SEGV_MAPERR), fault addr 0x0
01-05 18:48:12.254: A/DEBUG(533):     r0 9ebc82a8  r1 00000008  r2 00000000  r3 00000000
01-05 18:48:12.254: A/DEBUG(533):     r4 a169ec36  r5 9ebc7010  r6 9eaadb10  r7 fffff0d4
01-05 18:48:12.254: A/DEBUG(533):     r8 00001298  r9 0000002b  sl 00000001  fp 00000002
01-05 18:48:12.254: A/DEBUG(533):     ip 0000002b  sp a082e378  lr ffffffff  pc a0d01c3c  cpsr 60070010
01-05 18:48:12.254: A/DEBUG(533): backtrace:
01-05 18:48:12.254: A/DEBUG(533):     #00 pc 004d2c3c  /data/app/com.kongzhong.c1.uc-1/lib/arm/libunity.so
01-05 18:48:12.254: A/DEBUG(533):     #01 pc 00333cb4  /data/app/com.kongzhong.c1.uc-1/lib/arm/libunity.so
01-05 18:48:12.254: A/DEBUG(533):     #02 pc 0030e0d8  /data/app/com.kongzhong.c1.uc-1/lib/arm/libunity.so
01-05 18:48:12.254: A/DEBUG(533):     #03 pc 0035e680  /data/app/com.kongzhong.c1.uc-1/lib/arm/libunity.so
01-05 18:48:12.254: A/DEBUG(533):     #04 pc 00509e58  /data/app/com.kongzhong.c1.uc-1/lib/arm/libunity.so
01-05 18:48:12.254: A/DEBUG(533):     #05 pc 0050cc00  /data/app/com.kongzhong.c1.uc-1/lib/arm/libunity.so
01-05 18:48:12.254: A/DEBUG(533):     #06 pc 003f3441  /data/app/com.kongzhong.c1.uc-1/oat/arm/base.odex (offset 0x21f000)
{% endhighlight %}

毫无头绪，看backtarce，挂在unity的C++代码里，鬼知道是什么。
如果遇到这个问题，这之前有可能还会遇到说GL找不到上下文的报错。
{% highlight objc %}
call to OpenGL ES API with no current context (logged once per thread)
{% endhighlight %}

--EOF--						