---
layout: post
title: 指针二三事
categories:
- C&C++
tags:
- C语 
- 指针

---
C语言中最棘手问题的当属指针了，不过这也是C的精华所在。
指针是一种数据类型，区别在于指针类型的值是一个内存地址。32位机器上，指针变量占用四个字节。往往你觉得你把指针弄的很透彻了，但你还是会做错题目，或许因为粗心，或许自己混淆了，总之指针问题真的很绕。最近笔者在看「程序员求职成功之路」，书中伊始讲的就是C语言指针，看完之后获益匪浅，特在此总结一下。

1.先看一道经典的指针题目：

{% highlight objc %}
#include <stdio.h>
    int main()
    {
        int a[5][10];
        printf("%u,%u,%u\n",a,a+1,&a+1);
        return 0;
    }
{% endhighlight %}
输出结果为如下，试分析其原因：

![1](http://imemento.github.com/blogImages/20110806-1.png)

原因分析：a和&a都是数组a[5][10]的首地址。但是他们的类型却不相同，a是int a[10]的类型，而&a是a[5][10]的类型。指针运算中加减1代表的加减了指针类型的长度。故：
a+1=2293360+4 * 10=2293400
&a+1=2293360+4 * 10 * 5=2293560
更抽象的说，例如数组int a[M1][M2][.....][Mn]
a+1=首地址+M2 * M3 * ...... * Mn * sizeof(int)  
&a+1=首地址+M1 * M2 * M3 *...... * Mn * sizeof(int)

2.再看一道题：

{% highlight objc %}
#include <stdio.h>
    int main(void)
    {
        int a[5]={1,2,3,4,5};
        int *ptr1=(int*)(&a+1);
        int *ptr2=(int*)((int)a+1);
        printf("%x,%x",ptr1[-1],*ptr2);
        return 0;
    }
{% endhighlight %}
低位优先的平台输出16进制的结果如下，你做对了吗？？
![2](http://imemento.github.com/blogImages/20110806-2.png)
题目分析：有了上一道题目的基础，ptr1[-1]比较容易求出，正好是a[4]。重点来看*ptr2，
对于语句 * ptr2=(int *)((int)a+1)，在此处a已经被强制转化成了int型，所以ptr2指向的是首地址的下一个字节。所以ptr2这个int型指针指向的地址所存储的四个字节的值分别为00 00 00 02

![3](http://imemento.github.com/blogImages/20110806-3.png)

所以*ptr2所代表的整数是0x2000000.

3.const 关键字声明指针的含义：
{% highlight objc %}
#include <stdio.h>
int main()
{
    const int *a = (int*)malloc(sizeof(int));
    int const *b = (int*)malloc(sizeof(int));
    int* const c = (int*)malloc(sizeof(int));
    *a = 9;
    *b = 7;
    c = a;
    return 0;
}
{% endhighlight %}
报错如下：
![4](http://imemento.github.com/blogImages/20110806-4.png)
由上程序总结几点
1）int const * b;//指针常量，指针指向的变量不能改变值

2）const int * a //指针常量，同上

3)  int * const c; //常量指针，指针本身不能改变值，例如数组名就是一个常量指针

4）今天是七夕，我TM就这样陪着「指针」过了，「某*」求巴掌，求被扇。
【参考文献】程序员求职成功之路