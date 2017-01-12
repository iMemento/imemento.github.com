---
layout: post
title: Jenkins 'Waiting for next available executor'
categories:
- Unity3d
tags:
- Unity3d

---
用Jenkins部署Unity3d项目自动编译，新建一个Job，配置完成后，点击构建一直提示：
{% highlight no-highlight %}
连接等待中 - Waiting for next available executor
{% endhighlight %}
一番google之后发现原因可能有几个：一个是节点offline了，一个是节点disk不够了，还有number of executor是0。
但是我去[系统管理->进入节点->节点管理]里看了下，都是好的。

没办法只能用重启大法了   
<font color=DeepPink>jenkins_url//safeRestart</font>等待现有构建完成后重启。    
<font color=DeepPink>jenkins_url//restart</font>直接重启。    
重启之后就好了。

嗯，重启大法好。

--EOF--						