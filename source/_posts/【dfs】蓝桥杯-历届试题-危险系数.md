---
title: 【dfs】蓝桥杯 历届试题 危险系数
copyright: true
date: 2019-02-18 20:10:25
tags:
 - 搜索
 - 暴力
categories:
 - 蓝桥杯
keywords:
description:
---

## 问题描述
抗日战争时期，冀中平原的地道战曾发挥重要作用。
地道的多个站点间有通道连接，形成了庞大的网络。但也有隐患，当敌人发现了某个站点后，其它站点间可能因此会失去联系。
我们来定义一个危险系数DF(x,y)：
对于两个站点x和y (x != y), 如果能找到一个站点z，当z被敌人破坏后，x和y不连通，那么我们称z为关于x,y的关键点。相应的，对于任意一对站点x和y，危险系数DF(x,y)就表示为这两点之间的关键点个数。

本题的任务是：已知网络结构，求两站点之间的危险系数。
<!-- more -->
## 输入格式
输入数据第一行包含2个整数n(2 <= n <= 1000), m(0 <= m <= 2000),分别代表站点数，通道数；
接下来m行，每行两个整数 u,v (1 <= u, v <= n; u != v)代表一条通道；
最后1行，两个数u,v，代表询问两点之间的危险系数DF(u, v)。

## 输出格式
一个整数，如果询问的两点不连通则输出-1.

## 样例输入
7 6
1 3
2 3
3 4
3 5
4 5
5 6
1 6

## 样例输出
2

## 题意
给你一个无向图，问从给定起点到终点的所有路径当中哪些点是删除之后所有的路径都无法使起点到达终点。输出这类节点的数量。

## 思路
所谓的这类点，无疑就是所有路径都通过的点，我们只要找到那些所有路径都存在的点就是题目所求。
我们可以通过统计每个点被路径通过的次数，最后审查每个点，若某个点被路径通过的次数等于总的路径条数，那么这个点就是关键点。
找到从起点到终点的路径用dfs就可以，到达终点之后把该条路径上的点全部计数一遍，路径个数++，最后比较一下就好。

## AC代码
一开始中间的各个路径拿vector写的，又WA又T.....后面拿数组去模拟，０ms．
```c++
评测结果	正确
得分	100
CPU使用	0ms
内存使用	4.386MB

#include <bits/stdc++.h>
using namespace std;
const int maxn = 1e5 + 7;
vector<int> a[maxn];
int vis[maxn];
int tvis[maxn];
int t[maxn];
int s, e;
int flag;
void dfs(int root, int size)
{
    if (root == e)
    {
        flag++; //路径条数自增
        for (int i = 0; i < size; i++)
            vis[t[i]]++;
        /*将这条可达路径上的点全部计数。*/
        return;
    }

    for (int i = 0; i < a[root].size(); i++)
    {
        if (tvis[a[root][i]] == 0)
        {

            t[size] = root; /*记录路径*/
            tvis[root] = 1; /*当前节点标记已经走过。*/
            //printf("root:%d i:%d a[root][i]:%d\n", root, i, a[root][i]);
            //getchar();
            dfs(a[root][i], size + 1); //到下一个节点。
            t[size] = 0;               //回溯
            tvis[root] = 0;
        }
    }
}

void sovle(void)
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
    scanf("%d%d", &s, &e);
    //printf("%d", a[5][0]);
    // getchar();
    //getchar();
    for (int i = 0; i < a[s].size(); i++)
    {
        dfs(a[s][i], 0);
    }
    int ans = 0;
    //for (int i = 0; i <= n; i++)
    //    printf("%d%c", vis[i], i == n ? '\n' : ' ');
    for (int i = 1; i <= n; i++)
        if (vis[i] == flag)
            ans++;
    if (ans == 0)
        printf("-1");
    else
        printf("%d\n", ans);
}

int main(void)
{
    sovle();
    return 0;
}
```