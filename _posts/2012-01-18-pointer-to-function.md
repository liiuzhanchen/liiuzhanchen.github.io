---
layout: post 
title:  "函数指针总结"
categories: 编程语言
tags: [c++, 函数指针]
---

1.一个函数的指针，必须确保该函数被定义且分配了内存，否则将指向空地址，这是指针的大忌。
 
2.函数指针的使用条件：参数、返回值都吻合‘
 
3.函数指针没有++或--的运算
 
4.函数指针作为参数的最好的例子就是回调函数
 
5.函数指针的使用

例：

```c++
void (*p)(int &, int &);  
void cube(int &x, int &y)  
{  
    x = x * x * x;  
    y = y * y * y;  
}  
void Test(void (*p)(int &, int &), int &a, int &b)  
{  
    p(a, b);  
    cout<<a<<' '<<b<<endl;  
}  
int main()  
{  
    int x = 2, y = 3;  
    p = cube;  
    p(x, y);//在主函数中使用函数指针  
    cout<<x<<' '<<y<<endl;  
  
    Test(p, x, y);//函数指针作为函数  
    return 0;  
}  
```

输出：

```
8 27
512 19683
```

用typedef可以简化函数指针的使用。

例：

```c++
typedef void (*p)(int &, int &);  
void cube(int &x, int &y)  
{  
    x = x * x * x;  
    y = y * y * y;  
}  
void Test(p vp, int &a, int &b)  
{  
    vp(a, b);  
    cout<<a<<' '<<b<<endl;  
}  
int main()  
{  
    int x = 2, y = 3;  
    p  vp = cube;  
    vp(x, y);//在主函数中使用函数指针  
    cout<<x<<' '<<y<<endl;  
  
    Test(vp, x, y);//函数指针作为函数  
    return 0;  
}  
```

输出：

```
8 27
512 19683
```

6.对于成员函数指针，注意以下几点

1）要在特定类的对象中调用成员函数指针

2）尽量利用typedef简化成员函数指针

3）尽量不用成员函数指针

7.数组名是数组第一个元素的指针。同理，函数名也是指向函数第一条指令的常量指针

程序编译后，每个函数都有个首地址，也就是函数第一条指令的地址。

如果用一个指针来保存这个地址，那么这个指针就是函数指针，该指针可以看作是函数名。

因此我们可以通过函数指针调用函数
 
8.

```c++
int* func(int);  
```

声明了一个带有整型参数的函数func，并返回一个整型的指针

声明了一个函数，返回一个指针

```c++
int (*func)(int);  
```

声明一个int型的指针func，它指向一个函数，这个函数带有一个int型的参数，并返回int型的值。

声明一个指针，返回一个函数
 
9.使用函数指针，便于用户选择调用不同名字但是又类型和参数相同的函数
 
10.函数指针数组

```c++
void (*p[5])(int, int);  
```

声明了一个有5个元素的数组指针，每个指针指向的是带有两个int参数且返回void的函数