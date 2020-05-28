---
layout: post
title: 使用 MaterialPropertyBlock 
categories:
- Unity3d
tags:
- Unity3d

---

前些天做性能优化使用 `Frame Debugger` 发现坦克凭空多了很多dc，每一个负重轮都需一个dc，一下多了十几个，同一材质却不能合批，这不合理。
后发现坦克涂装做decal贴图，抓出material设置贴图导致 (在材质上读取设置任何参数都会导致Instanced)
虽然是同一材质，参数也完全相同，只要Instance了，就无法合批。

嗯，使用 `MaterailPropertyBlock` 可以阻止材质Instance，继而能够合批，有效降低dc数目。


示例代码如下：

```
private MaterialPropertyBlock decalMPB;

void Start()
{
	decalMPB = new MaterialPropertyBlock();
}

public void SetExclusiveDecoration(string path)
{
	if (allRenderTexDic.Count == 0) 
		InitializeMaterialDic();

	var tankKeyname = tank.ProtoKeyname;
	foreach (var k in allRenderTexDic.Keys.ToList())
	{
		var tex = Resources.Load(path) as Texture;
		if (tex == null) continue;
		
		k.GetPropertyBlock(decalMPB);
		decalMPB.SetTexture("_CMap", tex);
		k.SetPropertyBlock(decalMPB);
		allRenderTexDic[k] = tex;
	}
}

```

--EOF--						