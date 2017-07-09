---
layout: post
title: iOS Mono Full AOT 模式下的限制
categories:
- Unity3d
tags:
- Unity3d

---
<span style="color:DeepPink; font-size: 14pt">IL</span>         
IL是.NET框架中中间语言（Intermediate Language）的缩写。使用.NET框架提供的编译器可以直接将源程序编译为.exe或.dll文件，
但此时编译出来的程序代码并不是CPU能直接执行的机器代码，而是一种中间语言IL（Intermediate Language）使用中间语言可以实现平台无关性，既与特定CPU无关；实现.NET框架中语言之间的交互操作，这就是为什么unity3D里面可以c#和js混编。

<span style="color:DeepPink; font-size: 14pt">JIT</span>  
即时编译（英语：Just-in-time compilation），又译及时编译、实时编译，动态编译的一种形式，是一种提高程序运行效率的方法。通常，程序有两种运行方式：静态编译与动态解释。静态编译的程序在执行前全部被翻译为机器码，而解释执行的则是一句一句边运行边翻译。JIT狭义来说是当某段代码即将第一次被执行时进行编译，因而叫“即时编译”。JIT编译是动态编译的一种特例。
  
<span style="color:DeepPink; font-size: 14pt">AOT</span>     
静态编译、提前编译(Ahead-of-time compilation)

<span style="color:DeepPink; font-size: 14pt">Mono Full AOT 模式的限制</span>        
在iOS平台中，Mono是以Full AOT模式运行的，无法使用JIT引擎，Full AOT常见的限制：            
1.不支持泛型虚方法。         
因为对于泛型代码，Mono通过静态分析以确定要实例化的类型并生成代码，但静态分析无法确定运行时实际调用的方法（C++也因此不支持虚模版函数）。

2.不支持对泛型类的P/Invoke。
目前不能使用反射中的Property.SetInfo给非空类型赋值。

3.值类型作为Dictionary的Key时会有问题，实际上实现了IEquatable<T>的类型都会有此问题，因为Dictionary的默认构造函数会使用EqualityComparer<TKey>.Default作为比较器，而对于实现了IEquatable<T>的类型，EqualityComparer<TKey>.Default要通过反射来实例化一个实现了IEqualityComparer<TKey>的类（可以参考EqualityComparer<T>的实现）。 解决方案是自己实现一个IEqualityComparer<TKey>，然后使用Dictionary<TKey, TValue>(IEqualityComparer<TKey>)构造器创建Dictionary实例。
之前[GC优化篇](http://peakcoder.com/unity3d/2016/12/16/unity3d-gc.html)有提过。

4.由于不允许动态生成代码，不允许使用System.Reflection.Emit，不允许动态创建类型。
由于不允许使用System.Reflection.Emit，无法使用DLR及基于DLR的任何语言。
不要混淆了Reflection.Emit和反射，所有反射的API均可用

--EOF--						