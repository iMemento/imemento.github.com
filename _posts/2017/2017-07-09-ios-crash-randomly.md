---
layout: post
title: Unity3d iOS crash 'Ran out of trampolines' 
categories:
- Unity3d
tags:
- Unity3d

---
iOS平台不允许JIT，会通过Full AOT直接编译为ARM汇编代码。但在编译的时候仍有一些无法静态确定的事情：例如泛型接口可能需要不同的虚拟表（运行时存放执行方法的集合），这取决于接口被实例化时的类型。（对于这种情况，从技术上讲确定虚拟表的最大数量是可能的，但是这数量可能会是庞大的--即泛型接口数乘以应用中类型数).我们无法为这些虚拟表在运行时动态的分配内存，所以我们指定了一个合理的默认值并且允许用户在运行时出现问题的情况下增加默认值。这就是蹦床的基本理论（实际情况略微不同，这依赖于蹦床数的类型，但这并不重要）以上是[Rolf Bjarne Kvinge的解释](http://monotouch.2284126.n4.nabble.com/Understanding-the-impact-of-trampolines-td4495086.html)

#### Ran out of trampolines of type 0 
如果你再运行时，输出这个错误，你可以在项目option--iphone build处添加额外的参数-aot "ntrampolines=2048" 
这个参数默认值为1024，试着增加这个值直到满足你的应用的需求。
#### Ran out of trampolines of type 1 
如果你使用了过多的泛型嵌套，如List<T>中还有List<T>成员，你可以同上通过添加额外的参数-aot "nrgctx-trampolines=2048" 来解决。
蹦床类型1的默认值为1024.
#### Ran out of trampolines of type 2 
如果你界面操作频繁，你可以通过添加额外的参数-aot "nimt-trampolines=512" 来解决。
这里的默认值为128.


以上参数在Build settings的 <span style="color:DeepPink">AOT Compilation Options</span> 里添加

--EOF--						