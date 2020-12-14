---
layout: post
title: Unity3d gradle 编译后处理 
categories:
- Android
tags:
- Android

---

Unity 可以在 Android 目录直接放置 java 源文件，加上自定义 gradle 配置文件，使得接入第三方的 SDK 变得十分优雅。
Unity 使用 gradle 编译，直接导出 apk ，会在 tmp 文件下面先导出名为 gradleOut 的 android studio 工程。
在 as 工程导出后、编译前，可以做一些文件拷贝处理等工作，例如在接入 Firebase SDK 时需要拷贝 json文件到 launcher 目录。
代码入下：

```
using UnityEditor;
using UnityEditor.Android;
using UnityEngine;
using System.IO;
public class AndroidPostBuildProcessor : IPostGenerateGradleAndroidProject
{
    public int callbackOrder
    {
        get
        {
            return 0;
        }
    }
    void IPostGenerateGradleAndroidProject.OnPostGenerateGradleAndroidProject(string path)
    {
        Debug.Log("Bulid path : " + path);
        path = path.Replace("unityLibrary", "launcher");
        var json = path + "/google-services.json";
        File.Copy("Assets/Plugins/Android/google-services.json", json);
    }
}
```
--EOF--						