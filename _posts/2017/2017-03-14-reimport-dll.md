---
layout: post
title: Unity Reimport 'UnityEngine.UI.dll'
categories:
- Unity3d
tags:
- Unity3d

---
代码在切换分支后，Unity工程下Library文件夹(如果没有加入到版本控制中)可能会被污染，导致各种Unity的dll文件找不到，例如：
{% highlight no-highlight %}
"Asset 'C:/Program Files (x86)/Unity/Editor/Data/UnityExtensions/Unity/GUISystem/4.6.0/UnityEngine.UI.dll'
{% endhighlight %}

Unity的dll文件不能单独reimport，只能remiport all，虽然能解决问题，但是太慢太慢了，尤其是Android平台Load贴图，非常浪费时间。   
一番google之后，在Unity的坛子里，网友'j-robichaud'贡献了脚本，解决了问题，如救世主一般，感动。

{% highlight csharp %}
using UnityEngine;
using System.Collections.Generic;
using UnityEditor;
using System.Text.RegularExpressions;
using System.IO;
using System.Text;
 
public class ReimportUnityEngineUI
{
    [MenuItem( "Assets/Reimport UI Assemblies", false, 100 )]
    public static void ReimportUI()
    {
#if UNITY_4_6
        var path = EditorApplication.applicationContentsPath + "/UnityExtensions/Unity/GUISystem/{0}/{1}";
        var version = Regex.Match( Application.unityVersion,@"^[0-9]+\.[0-9]+\.[0-9]+").Value;
#else
        var path = EditorApplication.applicationContentsPath + "/UnityExtensions/Unity/GUISystem/{1}";
        var version = string.Empty;
#endif
        string engineDll = string.Format( path, version, "UnityEngine.UI.dll");
        string editorDll = string.Format( path, version, "Editor/UnityEditor.UI.dll");
        ReimportDll( engineDll );
        ReimportDll( editorDll );
               
    }
    static void ReimportDll(string path )
    {
        if ( File.Exists( path ) )
            AssetDatabase.ImportAsset( path, ImportAssetOptions.ForceUpdate| ImportAssetOptions.DontDownloadFromCacheServer );
        else
            Debug.LogError( string.Format( "DLL not found {0}", path ) );
    }
}
{% endhighlight %}

--EOF--						