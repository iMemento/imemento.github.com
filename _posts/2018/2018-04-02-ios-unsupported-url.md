---
layout: post
title: WWW 'unsupported url' error on iOS 
categories:
- Unity3d
tags:
- Unity3d

---
上周遇到一个奇葩问题，用 WWW 访问一个http 链接，在 `iOS` 上会报错 `unsupported url`，在 `android` 上没事，url 的编码都是英文和数字并没有特殊字符和汉字等。

用命令 `file --mime-encoding < file >`  
发现 `c#` 脚本的格式是 `us-ascii` 的，怀疑跟这个有关系   

先转成 `utf-8` 再说   
用命令 `iconv -f us-ascii -t utf-8 < file > < newfile >`   
发现新文件还是 `us-ascii` 的没变化，难道 `us-ascii` 是 `utf-8` 的子集，才没变化的么？

最后没办法了，在脚本里写了一行中文注释，保存，变成 `utf-8` 的 `encoding` 了，问题解决           
好傻啊

--EOF--						