---
layout: post
title: UGUI 'World Space' Canvas 不响应触摸
categories:
- Unity3d
tags:
- Unity3d

---
最近做航母飞机的预瞄，用的'World Space'的Canvas绘制在3d空间，但是不响应触摸事件，Canvas的'Sort Order'设置无效，事件永远被'Screen Space - Camera'的Canvas截获，之前的UGUI版本'GraphicRaycaster'脚本有显示的sort order设置，现在也没有了。

去bitbucket看了EventSystem源码发现比较顺序为    
1.GraphicRaycaster不同            
1) 都有event camera 先比较camera 的 depth         
2) 比较GraphicRaycaster 的 sortOrderPriority           
3) 比较GraphicRaycaster 的 renderOrderPriority         

2.GraphicRaycaster相同                
1) 比较Canvas 的 sortingLayer              
2) 比较Canvas 的 sortingOrder          
3) 比较控件的depth           
4) 比较与摄像机的distance，overlay为0            

{% highlight csharp %}

private static int RaycastComparer(RaycastResult lhs, RaycastResult rhs)
{
    if (lhs.module != rhs.module)
    {
        if (lhs.module.eventCamera != null && rhs.module.eventCamera != null && lhs.module.eventCamera.depth != rhs.module.eventCamera.depth)
        {
            // need to reverse the standard compareTo
            if (lhs.module.eventCamera.depth < rhs.module.eventCamera.depth)
                return 1;
            if (lhs.module.eventCamera.depth == rhs.module.eventCamera.depth)
                return 0;

            return -1;
        }

        if (lhs.module.sortOrderPriority != rhs.module.sortOrderPriority)
            return rhs.module.sortOrderPriority.CompareTo(lhs.module.sortOrderPriority);

        if (lhs.module.renderOrderPriority != rhs.module.renderOrderPriority)
            return rhs.module.renderOrderPriority.CompareTo(lhs.module.renderOrderPriority);
    }

    if (lhs.sortingLayer != rhs.sortingLayer)
    {
        // Uses the layer value to properly compare the relative order of the layers.
        var rid = SortingLayer.GetLayerValueFromID(rhs.sortingLayer);
        var lid = SortingLayer.GetLayerValueFromID(lhs.sortingLayer);
        return rid.CompareTo(lid);
    }


    if (lhs.sortingOrder != rhs.sortingOrder)
        return rhs.sortingOrder.CompareTo(lhs.sortingOrder);

    if (lhs.depth != rhs.depth)
        return rhs.depth.CompareTo(lhs.depth);

    if (lhs.distance != rhs.distance)
        return lhs.distance.CompareTo(rhs.distance);

    return lhs.index.CompareTo(rhs.index);
}

{% endhighlight %}

World Space Canvas 的 eventCamera 是'MainCamera',他的
depth一定是要小于UICamera的，基本没救了啊，只能把UI Canvas设置为Overlay来解决了，再做一个GraphicRaycaster子类设置sortOrderPriority来解决。

{% highlight csharp %}

public class WorldRaycaster : GraphicRaycaster
{
    [SerializeField]
    private int SortOrder = 0;

    public override int sortOrderPriority
    {
        get
        {
            return SortOrder;
        }
    }
}

{% endhighlight %}

找个时间通读一遍UGUI的源码，画下类图。


--EOF--						