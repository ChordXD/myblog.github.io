---
title: 【左偏树】洛谷 P1456 Monkey King
copyright: true
date: 2019-05-04 23:46:49
tags:
 - 左偏树
categories:
 - 数据结构
 - 树
keywords:
description:
---

## 题目描述
Once in a forest, there lived N aggressive monkeys. At the beginning, they each does things in its own way and none of them knows each other. But monkeys can't avoid quarrelling, and it only happens between two monkeys who does not know each other. And when it happens, both the two monkeys will invite the strongest friend of them, and duel. Of course, after the duel, the two monkeys and all of there friends knows each other, and the quarrel above will no longer happens between these monkeys even if they have ever conflicted.

Assume that every money has a strongness value, which will be reduced to only half of the original after a duel(that is, 10 will be reduced to 5 and 5 will be reduced to 2).

And we also assume that every monkey knows himself. That is, when he is the strongest one in all of his friends, he himself will go to duel.

一开始有n只孤独的猴子，然后他们要打m次架，每次打架呢，都会拉上自己朋友最牛叉的出来跟别人打，打完之后战斗力就会减半，每次打完架就会成为朋友（正所谓不打不相识o(∩_∩)o ）。问每次打完架之后那俩猴子最牛叉的朋友战斗力还有多少，若朋友打架就输出-1.

<!-- more -->

## 输入输出格式
### 输入格式：
There are several test cases, and each case consists of two parts.

First part: The first line contains an integer N(N<=100,000), which indicates the number of monkeys. And then N lines follows. There is one number on each line, indicating the strongness value of ith monkey(<=32768).

Second part: The first line contains an integer M(M<=100,000), which indicates there are M conflicts happened. And then M lines follows, each line of which contains two integers x and y, indicating that there is a conflict between the Xth monkey and Yth.

有多组数据

### 输出格式：
For each of the conflict, output -1 if the two monkeys know each other, otherwise output the strength value of the strongest monkey among all of its friends after the duel.

## 输入输出样例
输入样例#1： 
```
5
20
16
10
10
4
5
2 3
3 4
3 5
4 5
1 5
```
输出样例#1： 
```
8
5
5
-1
10
```
说明
题目可能有多组数据

## 思路
左偏树模板，注意更新数据的操作。
先把打架的猴子拆出来，然后把他的儿子合并了，再把他合并进去。
最后把两堆猴子合并。

## AC代码
```c++
#include<cstdio>
#include<iostream>
#include<algorithm>
using namespace std;

#define rs S[x].Son[1]
#define ls S[x].Son[0]
const int maxn = 5e5 + 7;

int F[maxn];

int Find(int x)
{
    return F[x] == x ? x : F[x] = Find(F[x]);
}

struct Tree {
    int Son[2], dis, val;
}S[maxn];

int Union(int x, int y)
{
    if (!x || !y)return x + y;
    if (S[x].val < S[y].val || (S[x].val == S[y].val && x < y))
        swap(x, y);
    rs = Union(rs, y);
    if (S[ls].dis < S[rs].dis) 
        swap(ls, rs);
    F[x] = F[ls] = F[rs] = x;
    S[x].dis = S[rs].dis + 1;
    return x;
}

int Updata(int x)
{
    S[x].val /= 2;
    int rt = Union(ls, rs);
    ls = rs = 0;
    F[x] = rt;
    return Union(rt, x);
}


int main(void)
{
    int n;
    while (cin >> n) {
        for (int i = 1; i <= n; i++) {
            cin >> S[i].val;
            S[i].dis = 0;
            F[i] = i;
            S[i].Son[0] = S[i].Son[1] = 0;
        }
        int m;
        cin >> m;
        while (m--) {
            int a, b;
            cin >> a >> b;
            int aa = Find(a);
            int bb = Find(b);
            if (aa == bb)
                cout << "-1" << endl;
            else {
                int ansrt = Union(Updata(aa), Updata(bb));
                cout << S[ansrt].val << endl;
            }
        }
    }
    return 0;
}

```