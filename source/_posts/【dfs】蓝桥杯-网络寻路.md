---
title: 【dfs】蓝桥杯　网络寻路
copyright: true
date: 2019-02-21 11:29:29
tags:
 - dfs
categories:
 - 蓝桥杯
keywords:
description:
---

## 问题描述
X 国的一个网络使用若干条线路连接若干个节点。节点间的通信是双向的。某重要数据包，为了安全起见，必须恰好被转发两次到达目的地。该包可能在任意一个节点产生，我们需要知道该网络中一共有多少种不同的转发路径。

源地址和目标地址可以相同，但中间节点必须不同。

如下图所示的网络。
![图一](/photo/wlxl.jpg)

1 -> 2 -> 3 -> 1 是允许的
1 -> 2 -> 1 -> 2 或者 1 -> 2 -> 3 -> 2 都是非法的。

<!-- more -->

## 输入格式
输入数据的第一行为两个整数N M，分别表示节点个数和连接线路的条数(1<=N<=10000; 0<=M<=100000)。
接下去有M行，每行为两个整数 u 和 v，表示节点u 和 v 联通(1<=u,v<=N , u!=v)。
输入数据保证任意两点最多只有一条边连接，并且没有自己连自己的边，即不存在重边和自环。

## 输出格式
输出一个整数，表示满足要求的路径条数。

## 样例输入1
	3 3
	1 2
	2 3
	1 3
## 样例输出1
	6

## 样例输入2
	4 4
	1 2
	2 3
	3 1
	1 4
## 样例输出2
	10

## 题意
给你一个无向图，问走四步且中间两点不同，起始两点可相同可不同，且必须中间两点为两个除了起始点之外不同的点的路径有多少条。

## 思路
dfs，直接记录经过点的次数，对于经过点的次数为３的结果有效路径，我们可以再判断一下能不能到达终点，能的话结果加１．其他情况正常判断能否经过４个点且满足题意即可。

## AC代码
评测结果	正确
得分	100
CPU使用	734ms
内存使用	4.199MB
```c++
#include <bits/stdc++.h>
using namespace std;
const int M = 1e5 + 7;
vector<int> a[M];
int start, vis[M], ans;
void dfs(int root, int go)
{
    if (go == 4) //经过了四个点且中间两个点与起始点不同
    {
        ans++;
        return;
    }
    vis[root] = 1; //当前点已经走过，标记一下。
    for (int i = 0; i < a[root].size(); i++)
    {
        if (a[root][i] == start && go == 3) //恰好经过三个点，下一个点可以到达起点的情况。
            ans++;
        else
        {
            if (vis[a[root][i]] == 0)
            {
                dfs(a[root][i], go + 1);
            }
        }
    }
    vis[root] = 0;
}
int main(void)
{
    int n, m;
    scanf("%d%d", &n, &m);
    for (int i = 0; i < m; i++)
    {
        int x, y;
        scanf("%d%d", &x, &y);
        a[x].push_back(y);
        a[y].push_back(x);
    }
    for (int i = 1; i <= n; i++)
    {
        if (a[i].size() == 0)
            continue;
        start = i; //记录起始点。
        dfs(i, 1);
    }
    printf("%d\n", ans);
    return 0;
}
```