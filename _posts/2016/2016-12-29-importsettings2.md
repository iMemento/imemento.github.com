---
layout: post
title: 错误的「Import Settings」 会拖垮你的 Unity3d 游戏 [Part 2]
categories:
- Unity3d
tags:
- Unity3d

---

[Part 1]里说到贴图的improt settings会影响性能，这次来说下Audio Clip的import settings.

Unity的默认设置大部分情况下是ok的，当然默认设置有些情景下不是最优的，很不幸Audio Clip Import Settings 默认设置就蛮糟糕。

与贴图的import settings 类似. Unity 支持各种音频格式，目前，Unity 5.2.1 支持 PCM, ADPCM, Vorbis/MP3 和 HEVAG. 
并不是每个平台都支持所有的压缩格式，有些只支持一种(WebGL只能用AAC)

主要还是Memory matters吧

Default Import Settings:    
Load Type: Decompress On Load    
Compression Format: Vorbis  

默认设置会占用非常大的内存！如果有很多音频文件移动设备可能顶不住。

Load Types:
Compressed In Memory – Audio Clip 存储在RAM里，播放时解压. 播放时不需要额外的内存
Streaming – Audio Clip 存储在 persistent memory (hard drive, flash drive etc)里.存储和播放都不需要RAM(播放时需要极少).
Decompress On Load – Audio Clip 无压缩存储在RAM里. 需要最多的RAM，但是不耗CPU
到底选哪个视情况而定.

<font color=DeepPink size=4>Music and/or Ambient Sounds</font>

音乐、BGM、环境声音一般比较大，存到RAM里很占内存，我们肯定不选 Decompress On Load， 至少要压缩过放到RAM里。  
所以有两种选择  
1.Load Type: Streaming Compression Format: Vorbis. 这个组合是最不占用内存的，但是耗CPU和磁盘I/O.  
2.Load Type: Compressed In Memory Compression Format: Vorbis. 跟第一种的区别是把I/O占用换成一部份内存占用了。  
你可以调整 Quality 来降低大小，100%最高，一般推荐70%.

空间换时间，代价就是这个比较耗CPU，可以在profiler里看下。

<font color=DeepPink size=4>Sound Effects</font>

Sounds effects 一般为中短型. 播放频率要么很多，要么很少。

播放频率高而且很短的使用 Decompress On Load，Compression 格式 使用PCM 或者 ADPCM . 选择PCM就不用解压了，音频很短，也会load很快.
ADPCM需要解压，但比Vorbis轻量。

播放频率高中型长度的使用 Compressed In Memory 和 ADPCM. ADPCM 差不多比PCM小3.5倍，解压比Vorbis快的多。

播放频率低而且很短的使用 Compressed In Memory 和 ADPCM. 

播放频率低中型长度的使用 Compressed In Memory 和 Vorbis. 用ADPCM可能还是会太占用RAM，反正不经常用，不会很耗CPU.

总结

build 版本前check 下 Audio Clip Import Settings. 


个人翻译.转载请注明出处.[原文](http://blog.theknightsofunity.com/wrong-import-settings-killing-unity-game-part-2/)

--EOF--		