---
layout: post
title: Xcode error 'MapFileParser.sh Permission denied - not set as executable'
categories:
- Unity3d
tags:
- Unity3d

---

公司的项目很大，而我的macbook ssd 只有128G。为了调试公司Unity项目的iOS版本，我都是先在PC上编译好，然后拷贝到mac编译。
这样做没什么问题，但是会有一些脚本文件失去可执行权限，比如可能会报以下的错：
{% highlight no-highlight %}
'MapFileParser.sh: Permission denied - not set as executable'
{% endhighlight %}

解决方法也比较简单，再将文件赋予可执行权限即可。

{% highlight csharp %}
chmod +x /path/to/MapFileParser
{% endhighlight %}

--EOF--						