---
layout: post
title: Creating plugins C# sripts to .dll files 
categories:
- Unity3d
tags:
- Unity3d

---
想着把第三方库的 C# 文件编译成 `.dll` 不然看着凌乱，方便管理

1) 新建 VS `Class Library` 工程

2) 删除工程的所有 `references`

3) Unity API 的 dll 引用 
添加 `UnityEngine.dll` 的引用    
编辑器代码的dll 则需要 `UnityEditor` 生成 runtime 的 dll 不能添加这个 dll               
路径在 `*Unity Install Dir*\Editor\Data\Managed`     

有使用新 UI 则需要 `UnityEngine.UI`    
路径在 `*Unity Install Dir*\Editor\Data\UnityExtensions\Unity\GUISystem`            

4) 添加条件编译的宏
工程->属性->生成 添加 需要的宏 `UNITY_EDITOR` `UNITY_IPHONE` `UNITY_ANDROID`

5) 修改 Target Framework
工程->属性->应用 修改目标框架改为 `Unity 3.5 .net full base class libraries`


我尝试用 `NGUI` 生成了一下，但是工程里已有的物件挂的 `NGUI` 脚本全掉了，毕竟之前挂靠的对应关系是在 .meta 文件中存储的，dll 没有了，这有点坑，拉到了，拉到了。

--EOF--						