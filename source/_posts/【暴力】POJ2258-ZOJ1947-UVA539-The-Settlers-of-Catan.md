---
title: 【暴力】POJ2258/ZOJ1947/UVA539 The Settlers of Catan
copyright: true
date: 2019-02-16 11:20:51
tags:
 - 暴力
 - 回溯
categories:
 - 暴力
keywords:
description:
---

Time Limit: 1000MS		Memory Limit: 65536K

## Description

Within Settlers of Catan, the 1995 German game of the year, players attempt to dominate an island by building roads, settlements and cities across its uncharted wilderness. 
You are employed by a software company that just has decided to develop a computer version of this game, and you are chosen to implement one of the game's special rules: 

When the game ends, the player who built the longest road gains two extra victory points. 
The problem here is that the players usually build complex road networks and not just one linear path. Therefore, determining the longest road is not trivial (although human players usually see it immediately). 
Compared to the original game, we will solve a simplified problem here: You are given a set of nodes (cities) and a set of edges (road segments) of length 1 connecting the nodes. 
The longest road is defined as the longest path within the network that doesn't use an edge twice. Nodes may be visited more than once, though. 

Example: The following network contains a road of length 12. 

o      o--o      o

 \    /    \    /

  o--o      o--o

 /    \    /    \

o      o--o      o--o

           \    /

            o--o
<!-- more -->

## Input

The input will contain one or more test cases. 
The first line of each test case contains two integers: the number of nodes n (2<=n<=25) and the number of edges m (1<=m<=25). The next m lines describe the m edges. Each edge is given by the numbers of the two nodes connected by it. Nodes are numbered from 0 to n-1. Edges are undirected. Nodes have degrees of three or less. The network is not neccessarily connected. 
Input will be terminated by two values of 0 for n and m.
Output

For each test case, print the length of the longest road on a single line.

## Sample Input
```
3 2
0 1
1 2
15 16
0 2
1 2
2 3
3 4
3 5
4 6
5 7
6 8
7 8
7 9
8 10
9 11
10 12
11 12
10 13
12 14
0 0
```

## Sample Output
```
2
12
```

## Source
Ulm Local 1998

## 题意
给你一个无向图，问其中的最长边是多少。

## 思路
拿邻接矩阵把所有的边存进去，然后用dfs每个点都扫一遍，不断更新最长边即可。
数据很小，直接暴力写就好。
 
## AC代码
```c++
#include <cstdio>
#include <cstring>
using namespace std;

/*
19792272	pengrj	2258	Accepted	4228K	16MS	G++	844B	2019-02-16 11:08:57
*/

int n, m;
const int maxn = 1e3;
int Map[maxn][maxn];
int ans;

void dfs(int tans, int j)
{
    for (int i = 0; i < n; i++)
    {
        if (Map[j][i] == 1)
        {
            Map[j][i] = Map[i][j] = 0;
            dfs(tans + 1, i);
            Map[j][i] = Map[i][j] = 1;
        }
    }
    ans = tans > ans ? tans : ans;
}

void solve(void)
{
    while (~scanf("%d%d", &n, &m))
    {
        if (n == m && n == 0)
            break;
        memset(Map, 0, sizeof Map);
        ans = 0;
        for (int i = 0; i < m; i++)
        {
            int x, y;
            scanf("%d%d", &x, &y);
            Map[x][y] = Map[y][x] = 1;
        }
        for (int i = 0; i < n; i++)
            dfs(0, i);
        printf("%d\n", ans);
    }
}

int main(void)
{
    solve();
    return 0;
}
```
