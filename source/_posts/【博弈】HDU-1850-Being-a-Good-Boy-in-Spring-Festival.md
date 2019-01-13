---
title: 【博弈】HDU 1850 Being a Good Boy in Spring Festival
copyright: true
date: 2019-01-12 22:32:25
tags:
 - 博弈
categories:
 - 博弈
 - Nim博弈
keywords:
description:
---

# Being a Good Boy in Spring Festival

**Time Limit: 1000/1000 MS (Java/Others)    Memory Limit: 32768/32768 K **

## Problem Description
一年在外 父母时刻牵挂
春节回家 你能做几天好孩子吗
寒假里尝试做做下面的事情吧

陪妈妈逛一次菜场
悄悄给爸爸买个小礼物
主动地 强烈地 要求洗一次碗
某一天早起 给爸妈用心地做回早餐

如果愿意 你还可以和爸妈说
咱们玩个小游戏吧 ACM课上学的呢～

下面是一个二人小游戏：桌子上有M堆扑克牌；每堆牌的数量分别为Ni(i=1…M)；两人轮流进行；每走一步可以任意选择一堆并取走其中的任意张牌；桌子上的扑克全部取光，则游戏结束；最后一次取牌的人为胜者。
现在我们不想研究到底先手为胜还是为负，我只想问大家：
——“先手的人如果想赢，第一步有几种选择呢？”
<!-- more -->

## Input
输入数据包含多个测试用例，每个测试用例占2行，首先一行包含一个整数M(1<M<=100)，表示扑克牌的堆数，紧接着一行包含M个整数Ni(1<=Ni<=1000000，i=1…M)，分别表示M堆扑克的数量。M为0则表示输入数据的结束。

## Output
如果先手的人能赢，请输出他第一步可行的方案数，否则请输出0，每个实例的输出占一行。

## Sample Input

```
3
5 7 9
0
```

## Sample Output

```
1
```

## Author
lcy

## Source

[ACM Short Term Exam_2007/12/13](http://acm.hdu.edu.cn/search.php?field=problem&key=ACM+Short+Term+Exam_2007%2F12%2F13&source=1&searchmode=source)

## 题意
先判断先手是否能赢，若能赢输出第一步必胜的移除的方案数量。

## 思路
判断先手是否处于必胜点根据nim博弈特性我们只要算所有堆的异或和是否为0即可

若为0直接输出0.
若不为零，则需要输出第一步的方案数量。
设a为某一堆的数目，a'为a**取走一部分**之后剩余的数目，b为剩余堆的异或和，sum为所有堆的异或和，（+）为异或符号。
若当前状态处于必胜态，即：
（1）、a (+) b = sum != 0.
若我们需要从必胜态的a堆当中取走一部分的石子将其转换为必败态（即P状态）,即需要取走一部分之后使得：
（2）、a' (+) b = 0
由于异或的性质，要想从（1）转移到（2），我们必须需要：
a' = b，
又因为b = sum (+) a。
所以可得a' = sum (+) a 。
且，由于**a'是a取走一部分的结果**，所以必有a' < a
即若，a > a' = sum (+) a，即可满足从(1) 转移到 (2)的要求，
即 若某一堆a 满足 ，a > sum (+) a，表示该堆存在一种方案可以将局势从N态转移到P态，即转移到必败态，即为一种方案。

那么程序设计起来就十分简单，我们只要先把整体异或值sum扫一遍扫出来，然后在对于每个a来判断是否满足，a > sum (+) a，即可，统计一下满足的情况数量即为所求。

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
const int maxn = 1e6 + 7;
int a[maxn];
int main(void)
{
    int n;
    while (cin >> n, n)
    {
        int sum = 0;
        for (int i = 0; i < n; i++)
        {
            cin >> a[i];
            sum ^= a[i];
        }
        if (sum == 0)
        {
            cout << 0 << endl;
        }
        else
        {
            int ans = 0;
            for (int i = 0; i < n; i++)
            {
                if ((a[i] ^ sum) < a[i])
                {
                    ans++;
                }
            }
            cout << ans << endl;
        }
    }
    //system("pause");
    return 0;
}
```