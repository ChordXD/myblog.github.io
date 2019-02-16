---
title: '【树型dp】HDU1520 Anniversary party '
copyright: true
date: 2019-02-15 20:53:23
tags:
 - dp
categories:
 - dp
 - 树型dp
keywords:
description:
---


Time Limit: 2000/1000 MS (Java/Others)    Memory Limit: 65536/32768 K (Java/Others)

## Problem Description
There is going to be a party to celebrate the 80-th Anniversary of the Ural State University. The University has a hierarchical structure of employees. It means that the supervisor relation forms a tree rooted at the rector V. E. Tretyakov. In order to make the party funny for every one, the rector does not want both an employee and his or her immediate supervisor to be present. The personnel office has evaluated conviviality of each employee, so everyone has some number (rating) attached to him or her. Your task is to make a list of guests with the maximal possible sum of guests' conviviality ratings.
 <!-- more -->

## Input
Employees are numbered from 1 to N. A first line of input contains a number N. 1 <= N <= 6 000. Each of the subsequent N lines contains the conviviality rating of the corresponding employee. Conviviality rating is an integer number in a range from -128 to 127. After that go T lines that describe a supervisor relation tree. Each line of the tree specification has the form: 
L K 
It means that the K-th employee is an immediate supervisor of the L-th employee. Input is ended with the line 
0 0
 

## Output
Output should contain the maximal sum of guests' ratings.
 

## Sample Input
7
1
1
1
1
1
1
1
1 3
2 3
6 4
7 4
4 5
3 5
0 0
 

## Sample Output
5

## 题意
有一个party，参加者都有一定的权值，参加者之间有上下所属关系，上司参加了下属就不能参加，问这个聚会所能带来的最大权值是多少。

## 思路
树上DP
dp[i]][0] -> 第i个人不参加时其和他的下属们所能带来的最大权值
dp[i][1] -> 第i个人参加时其和他们的下属所能带来的最大的权值。

dp[i][0] += for(所有部下) max(部下选择去，部下选择不去）。
dp[i][1] += for(所有部下) 部下选择不去。

从叶子节点开始向上回溯即可。

## AC代码
```c++
#include <bits/stdc++.h>
using namespace std;
/*
28188939	2019-02-15 20:33:30	Accepted	1520	109MS	3612K	1055 B	G++	古弦
*/
const int maxn = 6e4 + 7;
int n, dp[maxn][2];
vector<int> G[maxn];
int f[maxn];

void dfs(int root)
{
    for (int i = 0; i < G[root].size(); i++)
    {
        dfs(G[root][i]);
    }
    for (int i = 0; i < G[root].size(); i++)
    {
        dp[root][0] += (dp[G[root][i]][1] > dp[G[root][i]][0] ? dp[G[root][i]][1] : dp[G[root][i]][0]);
        dp[root][1] += dp[G[root][i]][0];
    }
}

int main(void)
{
    int n;
    while (~scanf("%d", &n))
    {
        memset(f, -1, sizeof f);
        memset(dp, 0, sizeof dp);
        for (int i = 1; i <= n; i++)
        {
            scanf("%d", &dp[i][1]); //读入每个节点的价值。
            G[i].clear();
        }
        int u, v;
        while (scanf("%d%d", &u, &v) && u && v)
        {
            G[v].push_back(u); //将儿子节点推入每个节点当中。
            f[u] = v;          //记录每个儿子节点的父亲节点。
        }
        int root = 1;
        while (f[root] != -1) //找到父亲节点。
            root = f[root];
        dfs(root);
        printf("%d\n", dp[root][0] > dp[root][1] ? dp[root][0] : dp[root][1]);
    }
    return 0;
}
```
