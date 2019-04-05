---
title: 【哈希】HRBUST 1013 Eqs
copyright: true
date: 2019-04-02 22:08:10
tags:
 - 哈希
categories:
 - 数据结构
 - 哈希表
keywords:
description:
---

Time Limit: 5000 MS	Memory Limit: 65536 K
Total Submit: 908(377 users)	Total Accepted: 479(332 users)	
## Description
Consider equations having the following form: 
a1x13+ a2x23+ a3x33+ a4x43+ a5x53=0 
The coefficients are given integers from the interval [-50,50]. 
It is consider a solution a system (x1, x2, x3, x4, x5) that verifies the equation, xi∈[-50,50], xi != 0, any i∈{1,2,3,4,5}. 

<!-- more -->

Determine how many solutions satisfy the given equation.
## Input
For each test case :
Line 1: Five coefficients a1, a2, a3, a4, a5, separated by blanks.
Process to the end of file.

## Output
For each test case :

Line 1: A single integer that is the number of the solutions for the given equation.(The output will fit in 32 bit signed integers.)

## Sample Input
	37 29 41 43 47
## Sample Output
	654

## 题意
给你一个五元方程组，并给出五元方程组相关的参数，问有多少个可行解。

## 思路
将a3x3^3+a4x4^3+ a5x5^3移到等式另一侧，无疑就变成一个右边的值是否左边有一个式子等于其，而左边方程式的所有值我们可以枚举一下，由于同一个值可能出现多个左边的等式等于其，所以我们也要解决一下冲突。
说白了就是hash,看等式右边是否存在一个值在左边的等式出现，且能产生这个值的等式有多少个。

很明显，我们建立哈希统计表的时候，应该表次数越少越好，不然会出现大量的表操作而浪费时间。

同时，注意清空表。

## AC代码
```c++
#include <bits/stdc++.h>
using namespace std;

#define ull unsigned long long
#define ll long long
const ull maxn = 1e6 + 7;
const ull veryluck = 13131;
const int luck = 12500007;

typedef struct poi
{
    ull num;
    poi *next;
    int cet;
    poi()
    {
        cet = 0;
        num = 0;
        next = NULL;
    }
} poi;

poi aa[maxn];

ull get_Hash(ull num)
{
    return (num % veryluck) % maxn;
}

void insert(ull num)
{
    int Hash = get_Hash(num);
    poi *t = aa + Hash;
    while (t->next != NULL)
    {
        t = t->next;
        if (t->num == num)
        {
            (t->cet)++;
            return;
        }
    }
    t->next = new poi();
    t = t->next;
    t->num = num;
    t->cet = 1;
}

int Find(ull num)
{
    int Hash = get_Hash(num);
    poi *t = aa + Hash;
    while (t->next != NULL)
    {
        t = t->next;
        if (t->num == num)
            return t->cet;
    }
    return 0;
}

void Clear(poi *t)
{
    if (t->next != NULL)
        Clear(t->next);
    delete t;
}

int main(void)
{
    ll a, b, c, d, e;
    std::ios::sync_with_stdio(false);
    while (cin >> a >> b >> c >> d >> e)
    {
        for (ll x1 = -50; x1 <= 50; x1++)
            if (x1 != 0)
                for (ll x2 = -50; x2 <= 50; x2++)
                    if (x2 != 0)
                        insert((ull)(a * x1 * x1 * x1 + b * x2 * x2 * x2 + luck));
        int ans = 0;
        for (ll x3 = -50; x3 <= 50; x3++)
            if (x3 != 0)
                for (ll x4 = -50; x4 <= 50; x4++)
                    if (x4 != 0)
                        for (ll x5 = -50; x5 <= 50; x5++)
                            if (x5 != 0)
                                ans += Find((ull)((0 - (c * x3 * x3 * x3 + d * x4 * x4 * x4 + e * x5 * x5 * x5)) + luck));

        cout << ans << endl;
        for (ull i = 0; i < maxn; i++)
            if (aa[i].next != NULL)
            {
                Clear(aa[i].next);
                aa[i].next = NULL;
            }
    }
    return 0;
}
```