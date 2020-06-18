---
layout: post
title: NavMeshAgent 'SetDestination' error
categories:
- Unity3d
tags:
- Unity3d

---

使用 `NavMeshAgent`的`SetDestination`函数，一直报错 `"SetDestination" can only be called on an active agent that has been placed on a NavMesh.` 
导致寻路失败。
导航网格生成完好，Destination的点使用 `NavMesh.SamplePosition` 验证过，也定会在可行走区域内。
Debug许久未果，后在Unity Forum找到答案。

`If you create a NavMeshAgent and set its position via transform.position=... and then try to SetDestination, it fails because the NavMeshAgent did not recognize the position change and does not know that it already is on the NavMesh. Use NavMeshAgent.Warp instead of transform.position to initialize the position before calling SetDestination.`

开始时直接使用`transfrom.poistion`赋值，`agent`未识别位置变化，不知道自己已在导航网格之上，可在初始时调用`Wrap`函数来初始化`agent`的位置。
示例代码如下：

```
agent.Warp(tank.BornPosition);

```

--EOF--						