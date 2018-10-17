---
title: 【数据结构学习】实用程序软件包utility.h代码分析
copyright: true
date: 2018-08-26 22:51:04
tags:
 - 数据结构
 - C++
categories:
 - 数据结构
keywords:
 - 数据结构
 - C++
 - 实用程序软件包utility.h代码分析
---

代码来自数据结构与算法教程（c++）版 P15页
在一些部分做了微小的调整。

<!-- more -->

-------------------------------

## 代码概述
代码总体来说算是一个简化版的bits/stdc++.h。里面集合了大部分常用的头文件，一些简单的交互io的设计。给出了随机数，定时器的实现。还有一些简单的输出实现。
总的来说非常简单，敲一遍代码就会了。

具体分析请见代码注释。

------------------------------------------------

## 代码实现
```c++
#ifndef UTILITY_H ///标准头文件写法。
#define UTILITY_H

using namespace std;    ///标准库定义在命名空间std当中。

#include<string>    ///标准串和操作
#include<iostream>  ///标准流操作
#include<limits>    ///极限
#include<cmath>     ///数据函数
#include<fstream>   ///文件流输入输出
#include<cctype>    ///字符处理
#include<ctime>     ///日期和时间函数
#include<cstdlib>   ///标准库
#include<cstdio>    ///标准输入输出
#include<iomanip>   ///输入输出流格式设置
#include<cstdarg>   ///支持边长函数参数
#include<cassert>   ///支持断言

#define DEFAULT_SIZE 100 ///默认元素个数
#define DEFAULT_INFINITY 0x3f3f3f3f ///0x3f3f3f3f是一个比较大的数，这里可以当成正无穷使用。原书定义为1e6。

///返回用户的选择的结果。如果用户输入y，返回true，用户输入n，返回false。
static bool UserSaysYes(void)
{
    char ch;
    bool initialResponse = true;

    do{ ///循环知道用户输入到恰当的回答为止。
        if(initialResponse)     ///初始回答。
            cout << "(y,n)?";
        else
            cout << "用y或n回答："; ///非初始回答。
        while((ch = cin.get())==' '||ch=='\t'||ch=='\n')
            ; ///跳过输入当中的所有的空格，制表符，换行
        while(cin.get()!='\n')///跳过输入第一个字母之后所有的其他符号。
            ;
        initialResponse = false;
    } while (ch != 'y' && ch != 'Y' && ch != 'n' && ch != 'N');///如果读取的字符不符合规范，继续循环。

    if(ch=='y'||ch=='Y')///判断一下用户键入的字符，返回相应的结果。
        return true;
    else
        return false;
}

class Timer///定时器类。
{
    private:
      clock_t startTime;    ///起始时间

    public:
      Timer() { startTime = clock(); } ///起始时间时间设置为当前系统的时间。
      ~Timer(){};
      double ElapsedTime()  ///查询使用的时间。
      {
          clock_t endTime = clock();
          return (double)(endTime - startTime) / (double)CLK_TCK; ///返回从开始启动或者最后一次调用Reset()之后使用的CPU时间。
      }
      void Reset() { startTime = clock(); }///重置时间。
};

class Rand ///随机数类Rand
{
    public:
      static void SetRandSeed() { srand((unsigned)time(NULL)); } /// 设置当前时间为随机数种子。
      static int GetRand(int n) { return rand() % (n); }        /// 返回从0~n-1.
      static int GetRand() { return rand(); }                   /// 生成随机数。
};

template<class ElemType> ///交换函数实现。
void Swap(ElemType &e1,ElemType &e2)
{
    ElemType temp = e1;
    e1 = e2;
    e2 = temp;
}

template<class Elemtype>  ///输出一个是序列的前n个
void Show(Elemtype elem[],int n)
{
    for(int i = 0 ; i < n ; i++)
        cout << elem[i] << (i == n - 1 ? '\n' : ' ');
}

template<class Elemtype>///显示一个数据元素。
void Show(const Elemtype &e)
{
    cout << e <<" ";
}


#endif // !UTILITY_H
```

---------------------

## 代码测试
同样来自书本第P18页
没有什么需要特别说明的难点，敲一遍读一遍注释就都会了。

```c++
#include "utility.h"
#define NUM 280
int main(void)
{
    try
    {
        if(NUM > 280) throw"NUM的值太大了!"; ///抛出异常。

        int a[NUM + 1][NUM + 1],b[NUM + 1][NUM + 1],c[NUM + 1][NUM + 1];
        bool isContinue = true; ///是否继续。
        Timer objTimer;         ///设定一个定时器类。

        while(isContinue)
        {
            int i,j,k;
            Rand::SetRandSeed();///给随机数一个一个种子。
            objTimer.Reset();   ///开始计时

            for(i = 1 ;i < NUM ; i++)
            {
                for(j = 1 ; j < NUM ; j++)
                {
                    a[i][j] = Rand::GetRand();///获得一个随机数
                    b[i][j] = Rand::GetRand();
                }
            }

            for(int i = 1 ; i <= NUM ; i++)
            {
                for(j = 1 ; j <= NUM ; j++)
                {
                    c[i][j] = 0;
                    for(k = 0 ; j <= NUM ; j++)
                    {
                        c[i][j] += c[i][k]*c[k][j];///累加求和。
                    }
                }
            }

            cout<<"用时："<<objTimer.ElapsedTime()<<"秒"<<endl;///显示上述程序时间。
            cout<<"是否继续";
            isContinue = UserSaysYes(); ///读取用户的选择。
        }
    }
    catch(char *mess)///捕捉异常。
    {
        cout<<mess<<endl;///显示异常
    }
    return 0;
}

```