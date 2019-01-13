---
title: 【博弈】HDU 1848 Fibonacci again and again
copyright: true
date: 2019-01-13 21:32:18
tags:
 - 博弈
categories:
 - 博弈
 - Nim博弈
keywords:
description:
---

# Fibonacci again and again

**Time Limit: 1000/1000 MS (Java/Others)    Memory Limit: 32768/32768 K (Java/Others)**

## Problem Description
任何一个大学生对菲波那契数列(Fibonacci numbers)应该都不会陌生，它是这样定义的：
F(1)=1;
F(2)=2;
F(n)=F(n-1)+F(n-2)(n>=3);
所以，1,2,3,5,8,13……就是菲波那契数列。
在HDOJ上有不少相关的题目，比如1005 Fibonacci again就是曾经的浙江省赛题。
今天，又一个关于Fibonacci的题目出现了，它是一个小游戏，定义如下：
1、  这是一个二人游戏;
2、  一共有3堆石子，数量分别是m, n, p个；
3、  两人轮流走;
4、  每走一步可以选择任意一堆石子，然后取走f个；
5、  f只能是菲波那契数列中的元素（即每次只能取1，2，3，5，8…等数量）；
6、  最先取光所有石子的人为胜者；

假设双方都使用最优策略，请判断先手的人会赢还是后手的人会赢。

## Input
输入数据包含多个测试用例，每个测试用例占一行，包含3个整数m,n,p（1<=m,n,p<=1000）。
m=n=p=0则表示输入结束。

## Output
如果先手的人能赢，请输出“Fibo”，否则请输出“Nacci”，每个实例的输出占一行。

## Sample Input
```
1 1 1
1 4 1
0 0 0
```

## Sample Output
```
Fibo
Nacci
```

## Author
lcy

## Source
[ACM Short Term Exam_2007/12/13](http://acm.hdu.edu.cn/search.php?field=problem&key=ACM+Short+Term+Exam_2007%2F12%2F13&source=1&searchmode=source)

## 题意
三堆石子，每次取走的数量为斐波那契数列。

## 思路
求一波SG函数的值，然后对于初始堆判断一下SG值的异或和是否为0，是的话就是先手必败。

## 坑点
1、对于一个取走数量确定的任意数量的堆来说任意数值的SG值是确定的，先离线处理出SG值，最后对于任意堆再处理，否则会TLE。
2、注意异或优先级低于逻辑判等。

## AC代码
```c++
#include <iostream>
#include <cstdio>
#include <cstdlib>
#include <algorithm>
#include <cmath>
#include <vector>
#include <set>
#include <map>
#include <unordered_set>
#include <unordered_map>
#include <queue>
#include <ctime>
#include <cassert>
#include <complex>
#include <string>
#include <cstring>
#include <chrono>
#include <random>
#include <queue>
#include <bitset>
#include <stack>
using namespace std;
typedef long long ll;
typedef pair<int, int> pii;
typedef pair<ll, int> pli;
typedef pair<ll, ll> pll;
typedef long double ld;
#define mp make_pair
const int maxn = 1e4 + 7;
int sg[maxn];
int t[maxn];
int f[maxn];
int p = 31;
void getSG(int n, int sg[])
{
    sg[0] = 0;
    for (int i = 1; i <= n; i++)
    {
        memset(t, 0, sizeof t);
        for (int j = 0; f[j] <= i && j <= p; j++)
            t[sg[i - f[j]]] = 1;
        for (int j = 0;; j++)
        {
            if (!t[j])
            {
                sg[i] = j;
                break;
            }
        }
    }
}
int main(void)
{
    f[0] = 1;
    f[1] = 2;
    for (int i = 2; i < p; i++)
    {
        f[i] = f[i - 1] + f[i - 2];
    }
    int n, m, q;
    getSG(10000, sg);
    while (cin >> n >> m >> q)
    {
        if (n == m && m == q && n == 0)
            return 0;
        if ((sg[n] ^ sg[m] ^ sg[q]) == 0)
        {
            cout << "Nacci" << endl;
        }
        else
        {
            cout << "Fibo" << endl;
        }
    }
    //system("pause");
    return 0;
}
```