---
layout: post
title: NGUI Atlas Maker Bug
categories:
- Unity3d Tips
tags:
- Unity3d

---

既有项目大部分界面还在使用 NGUI 制作 UI, 简直上古社会。 最近美术同学发现了一个 bug，说使用 NGUI Atlas Maker 打图，既有的碎图会变的越来越糊。
经过调试，发现是 Unity 更新了贴图导入机制导致的。
需要在 NGUIEditorTools.cs 的 MakeTextureReadable 函数里写行代码，显示的将导入的 Atlas 的大图设为不压缩格式，不然之前的大图如果有 compression, GetPixels32 函数获取的像素就为失真的，会越来越糊。

```
ti.textureCompression = TextureImporterCompression.Uncompressed; 
```

```
static public bool MakeTextureReadable (string path, bool force)
{
    if (string.IsNullOrEmpty(path)) return false;
    TextureImporter ti = AssetImporter.GetAtPath(path) as TextureImporter;
    if (ti == null) return false;

    TextureImporterSettings settings = new TextureImporterSettings();
    ti.ReadTextureSettings(settings);

    if (force || !settings.readable || settings.npotScale != TextureImporterNPOTScale.None || settings.alphaIsTransparency)
    {
        settings.readable = true;
        if (NGUISettings.trueColorAtlas) settings.textureFormat = TextureImporterFormat.AutomaticTruecolor;
        settings.npotScale = TextureImporterNPOTScale.None;
        settings.alphaIsTransparency = false;
        ti.SetTextureSettings(settings);
        ti.textureCompression = TextureImporterCompression.Uncompressed; 
        AssetDatabase.ImportAsset(path, ImportAssetOptions.ForceUpdate | ImportAssetOptions.ForceSynchronousImport);
    }
    return true;
}
```


--EOF--						