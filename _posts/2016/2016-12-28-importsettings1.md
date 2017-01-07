---
layout: post
title: 错误的「Import Settings」 会拖垮你的 Unity3d 游戏 [Part 1]
categories:
- Unity3d
tags:
- Unity3d

---

游戏开发过程中需要注意的事情有很多。目标平台可能会因为你的模型面数太多而难以运行；你的算法可能会很耗CPU；
你可能用了太多的材质，Unity不能更高效的batching。这些都是棘手的问题，作为游戏开发者从始至终都要把握好视觉效果和性能之间的平衡。
然而，有些我们忽视了的小事儿，也对性能有显著的影响。

Texture Import Settings

当你导入一张贴图到你的工程里的时候，Unity会基于当前的 texure import settings 将其转成一种合适的格式，在大多数情况下这就足够了，
但是直接就这样发布给玩家可能不够好，这就是我们要说的地方。选中一张texture，在Inspector里就能看到她的import settings.

有三件事情需要注意

<font color=DeepPink size=4>Texture Type</font>

Texture Type 主要是用来告诉Unity这张贴图是用来做什么的，所有的选项除了「Advanced」都会调整贴图的内部设置而达到各自最佳的目的。
贴图的「Texture」是一种简单的diffuse类型，
用来给3d空间和3d游戏的。贴图的类型主要作用可以在Unity官方manual里找到。

The risk: 设置了错误的贴图类型，一开始可能没有察觉，就会导致性能损失。有些类型是必须设置的，因为shader需要这种类型才能工作(比如用作bump mapping的贴图必须设置为「Normal map」类型)。

<font color=DeepPink size=4>Size</font> 

贴图的大小和压缩格式

这些设置会决定你的贴图怎样在ram里存储，有多大。这里是Max size 而不是 size，因为你可是在Quality Settings里设置所有贴图的分辨率。
即便设置max size 为2048也不能保证这张图是2048的，就算uality Settings 设置为 highest，分辨率也可能比2048小，因为你原始贴图没有这么大
（感觉在说废话啊）。其实你的贴图是可以设置的比预期大一点，反正可以直接设置所有的quality。

The Risk: 有些模型在场景里显示的很小，但是贴图用的是高分辨率的。你能通过scene -> Mipmaps 筛选出这些物件（变红的就是过大分辨率的贴图）。

<font color=DeepPink size=4>Format</font>  

除了「Advanced」有四种格式  

<font color=DeepPink>·Compressed</font> – GPU 付出一点点换来贴图大瘦身，Compressed 会为你的目标平台选择一种最合适的压缩算法.  

<font color=DeepPink>·16 bits</font> – 无压缩 16 bit，适用于色彩少的。  

<font color=DeepPink>·Truecolor</font> – 无压缩 32 bit 真彩色. 看着爽, 但一张2048x2048 17M.  

<font color=DeepPink>·Crunched</font> – 用支持GPU压缩格式的方式压缩了一遍，然后再用CPU能够处理的格式再压缩一遍，下载Asset bundle的贴图比较适用。

<font color=DeepPink>建议就是除非有什么特殊目的直接用Compressed.</font>


格式「advanced」还没说，设置为「advanced」能够设置更多的压缩格式，
但很多压缩格式有些平台是不支持的（比如Android好多设备支持不同的压缩格式，但应该都支持ETC）.  

The Risk: 压缩过的贴图有时候蛮丑的，16bits和Truecolor倒是好看，但占用更多的内存，你可以在build完之后查看editor.log，看图片到底占了多大空间。


个人翻译.转载请注明出处.[原文](http://blog.theknightsofunity.com/wrong-import-settings-killing-unity-game-part-1/)  

--EOF--						