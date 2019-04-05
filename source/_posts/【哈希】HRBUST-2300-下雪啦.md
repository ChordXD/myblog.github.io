---
title: 【哈希】HRBUST 2300 下雪啦
copyright: true
date: 2019-04-05 20:06:20
tags:
 - 哈希
categories:
 - 数据结构
 - 哈希表
keywords:
 - 数据结构
 - 哈希表
 - hrbustoj2300
description:
---

Time Limit: 2500 MS	Memory Limit: 32768 K
## Description
陈月亮最喜欢的季节就是冬天了，这不看着窗外飘起了雪花，陈月亮开心的跑出屋来看雪。但是迷迷糊糊的陈月亮不知道自己是在做梦还是真的下起了雪。突然她想起了一句话，在真实世界中是没有两片一样的雪花的。于是你的任务就是比较这场雪中的所有雪花，如果出现了两朵完全一致的雪花，则证明陈月亮是在梦中。

每朵雪花用六个整数表示，范围在（1 – 10000000）之间，表示雪花六个花瓣的长度，六个整数的先后出现顺序可能是顺时针顺序也可能是逆时针顺序，并且可能是从任意一个花瓣开始的。比如说对同一个花瓣，描述方法可能是1 2 3 4 5 6 或者 4 3 2 1 6 5

<!-- more -->

## Input
第一行为一个整数T，表示有T组测试数据。
每组测试数据第一行为一个整数N(0 < N <= 100000),表示雪花的数目。
接下来n行每行六个整数，描述一朵雪花。

## Output
如果没有相同的雪花，输出“No two snowflakes are alike.”，否则输出“Twin snowflakes found.”

## Sample Input
1
2
1 2 3 4 5 6
4 3 2 1 6 5

## Sample Output
Twin snowflakes found.

## Source
2016级新生程序设计全国邀请赛

## 题意
给出N个序列，若两个序列从从任意位置开始的顺时针或逆时针方向读取序列所得到的序列相同，则这两个序列完全相同。找出所给的序列当中有没有两个相同的序列。

## 思路
对于每个序列，我们都可以使用其的序列和当成一个哈希值，将同一个的哈希值存在一个线性表当中，对于新序列，我们只需要查询以存储过的哈希表当中是否有相同哈希值的元素。
当然，对于相同的哈希值，也有可能出现序列不相等的情况，这时候我们只需要用一个比对函数比对所有可能的序列出现情况即可。
对于序列插入哈希表的时候，若哈希值相同，但序列不相同的情况，我们需要存储到相同哈希值下的不同位置，这时候我们就需要对于每一个哈希值创建一个数组记录相同哈希值但是却是不同序列的情况。
所以说，vector\<int>是存储这道题的数据结构，也是这题当中哈希表的存储方式。

读入很大，可以考虑使用读入挂。
亲测使用读入挂比scanf要快一倍。

## AC代码
`877607	2300	Accepted	G++	355ms	7144k	软件-Chord	1985B	2019-04-03 21:30:03`
```c++
#include <bits/stdc++.h>
using namespace std;
const int maxn = 1e5 + 7;
int a[maxn][6];
bool check(int o, int t)
{
    for (int i = 0; i < 6; i++)
        if ((a[o][0] == a[t][i % 6] && a[o][1] == a[t][(i + 1) % 6] && a[o][2] == a[t][(i + 2) % 6] && a[o][3] == a[t][(i + 3) % 6] &&
             a[o][4] == a[t][(i + 4) % 6] && a[o][5] == a[t][(i + 5) % 6]) ||
            (a[o][0] == a[t][i % 6] && a[o][1] == a[t][(i + 5) % 6] && a[o][2] == a[t][(i + 4) % 6] && a[o][3] == a[t][(i + 3) % 6] && a[o][4] == a[t][(i + 2) % 6] && a[o][5] == a[t][(i + 1) % 6]))
            return true;
    return false;
}

template <class T>
inline bool scan_d(T &ret)
{
    char c;
    int sgn;
    if (c = getchar(), c == EOF)
        return 0;
    while (c != '-' && (c < '0' || c > '9'))
        c = getchar();
    sgn = (c == '-') ? -1 : 1;
    ret = (c == '-') ? 0 : (c - '0');
    while (c = getchar(), c >= '0' && c <= '9')
        ret = ret * 10 + (c - '0');
    ret *= sgn;
    return 1;
}

int main(void)
{
    std::ios::sync_with_stdio(false);
    int t;
    scan_d(t);
    while (t--)
    {
        int n;
        scan_d(n);
        for (int i = 0; i < n; i++)
            for (int j = 0; j < 6; j++)
                scan_d(a[i][j]);
        vector<int> Hash[maxn];
        int flag = 0;
        for (int i = 0; i < n; i++)
        {
            int sum = 0;
            for (int j = 0; j < 6; j++)
                sum += a[i][j];
            sum %= maxn;
            for (int j = 0; j < Hash[sum].size(); j++)
            {
                if (check(i, Hash[sum][j]))
                    flag = 1;
                if (flag)
                    break;
            }
            if (flag)
                break;
            Hash[sum].push_back(i);
        }
        if (flag)
            cout << "Twin snowflakes found." << endl;
        else
            cout << "No two snowflakes are alike." << endl;
    }
    return 0;
}
```
