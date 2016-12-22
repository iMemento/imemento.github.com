---
layout: post
title: NGUI多个「弹出框」同时出现UI错乱解决方案
categories:
- Unity3d
tags:
- Unity3d

---
在制作UI时，在「主场景」或者「主城」可能会同时触发多条网络协议，或者通知协议，
如果她们的回包里又需要有弹出UI显示，那这些UI就会堆叠，错乱。

我这边有个取巧的解决方案，做一个抽象类，让所有的弹出界面继承这个类，有一个收集当前所有弹出界面的静态list，
在OnEnable() 和 OnDisable()函数里做处理，只让最后一个弹出框显示，把其界面的UIPanle的alpha置为零。(NGUI UIPanel的透明度
是零的话，也不会接受事件处理，这个蛮好)

使用这个方法的前提是所有的弹出界面的根节点上需要有UIPanel脚本(这个基本所有弹出界面都会挂)，再有就是如果弹出界面也需要用到 
OnEnable() 和 OnDisable()，override后，需要分别调用base.OnEnable()和base.OnDisable()

代码如下：
{% highlight objc %}
using UnityEngine;
using System.Collections.Generic;
public abstract class PopupPanelView : MonoBehaviour {

	private UIPanel panel;
	private static List<PopupPanelView> list = new List<PopupPanelView>();

	protected virtual void OnEnable () 
	{
		if (panel == null)
		{
			panel = GetComponent<UIPanel>();
		}

		if (panel != null)
		{
			list.ForEach(s => s.panel.alpha = 0);
			list.Add (this);
		}
	}

	protected virtual void OnDisable() {
		list.Remove (this);

		if (list.Count > 0)
		{
			list[list.Count - 1].panel.alpha = 1;
		}
	}
}
{% endhighlight %}

--EOF--						