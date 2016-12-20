---
layout: post
title: Unity3d 实现 NotificationCenter 
categories:
- Unity3d
tags:
- Unity3d

---
2012年时候还在做iOS，OC里有个NotifationCenter蛮好用的，之后做Coscos2d也有这个，
然Unity并没有这个，且Unity SendMessage的消息机制效率低下，而且inactive的物件收不到消息，还只能带一个参数，忒不好用。
C#的Action和委托倒是好，但是公司项目里用多播注册事件的地方，很多人忘记取消注册，导致各种bug，我心酸。心想还是自己写个Notification吧。

代码如下：

{% highlight csharp %}
/*
  Author: Danny
  CreateDate: 2016-08-27 09:22:01
  Desc: Custom NotificationCenter
*/

using UnityEngine;
using System.Collections.Generic;
using System;
using System.Collections;

public class NotificationCenter
{
	public static NotificationCenter DefaultCenter
	{
		get
		{
			if (defaultCenter == null)
			{
				defaultCenter = new NotificationCenter();
			}
			return defaultCenter;
		}
	}

	private static NotificationCenter defaultCenter;

	private NotificationCenter()
	{
		notifications = new Hashtable();
	}

	private Hashtable notifications;

	public delegate void NotificationHandler0();

	public delegate void NotificatonHandlerN(params object[] args);

	public void AddObserver(object observer, string notification, NotificationHandler0 handler)
	{
		AddObserverFunc(observer, notification, handler);
	}

	public void AddObserver(object observer, string notification, NotificatonHandlerN handler)
	{
		AddObserverFunc(observer, notification, handler);
	}

	private void AddObserverFunc(object observer, string notification, object handler)
	{
		if (string.IsNullOrEmpty(notification))
		{
			Debug.Log("Null notification specified for notification in AddObserver.");
			return;
		}

		if (observer == null)
		{
			Debug.Log("Null observer specified for notification in AddObserver.");
			return;
		}

		if (notifications[notification] == null)
		{
			notifications[notification] = new Dictionary<object, object>();
		}

		var handlers = notifications[notification] as Dictionary<object, object>;
		if (!handlers.ContainsKey(observer))
		{
			handlers.Add(observer, handler);
		}
	}

	public void RemoveObserver(object observer, string notification)
	{
		if (notifications[notification] == null)
		{
			Debug.LogWarning("No need to remove notification not exist");
			return;
		}

		var handlers = notifications[notification] as Dictionary<object, object>;

		if (handlers != null)
		{
			if (handlers.ContainsKey(observer))
			{
				handlers.Remove(observer);
			}
		}

		if (handlers.Count == 0)
		{
			notifications.Remove(notification);
		}
	}

	private List<object> observersToRemove = new List<object>();

	public void PostNotification(string notification, params object[] args)
	{
		var handlers = notifications[notification] as Dictionary<object, object>;

		observersToRemove.Clear();

		if (handlers != null)
		{
			foreach (var handler in handlers)
			{
				if (handler.Key != null)
				{
					if (handler.Value != null)
					{
						if (handler.Value is NotificationHandler0)
						{
							(handler.Value as NotificationHandler0)();

							if (args.Length != 0)
							{
								Debug.LogWarning(string.Format("handler '{0}' receive useless paramter. 
                                                 notfication: {1}", handler.Value, notification));
							}

						}
						else
						{
							(handler.Value as NotificatonHandlerN)(args);
						}
					}
					else
					{
						Debug.LogError(string.Format("Opps! Receive notification '{0}', 
                                       but handler has been destroyed ", notification));
					}
				}
				else
				{
					Debug.LogError(string.Format("Opps! Receive notification '{0}', 
                                   but observer has been destroyed ", notification));
                                   
					observersToRemove.Add(handler.Key);
				}
			}
		}

		if (observersToRemove.Count > 0)
		{
			foreach (var o in observersToRemove)
			{
				handlers.Remove(o);
			}
		}
	}
}
{% endhighlight %}


--EOF--						