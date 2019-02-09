---
title: 【dfs】蓝桥杯 剪格子
copyright: true
date: 2019-02-05 00:42:01
tags:
 - 蓝桥杯
categories:
 - 蓝桥杯
keywords:
description:
---

## 问题描述
如下图所示，3 x 3 的格子中填写了一些整数。

	+--*--+--+
	|10* 1|52|
	+--****--+
	|20|30* 1|
	*******--+
	| 1| 2| 3|
	+--+--+--+
我们沿着图中的星号线剪开，得到两个部分，每个部分的数字和都是60。

本题的要求就是请你编程判定：对给定的m x n 的格子中的整数，是否可以分割为两个部分，使得这两个区域的数字和相等。

如果存在多种解答，请输出包含左上角格子的那个区域包含的格子的最小数目。

如果无法分割，则输出 0。
<!-- more -->
## 输入格式
程序先读入两个整数 m n 用空格分割 (m,n<10)。

表示表格的宽度和高度。

接下来是n行，每行m个正整数，用空格分开。每个整数不大于10000。

## 输出格式
输出一个整数，表示在所有解中，包含左上角的分割区可能包含的最小的格子数目。
## 样例输入1
	3 3
	10 1 52
	20 30 1
	1 2 3
## 样例输出1
	3
## 样例输入2
	4 3
	1 1 1 1
	1 30 80 2
	1 1 1 100
## 样例输出2
	10
## 题意
RT，从左上角开始问是否有一种连续的分割方法把整个图划分为图内所有点权值之和相等的两个区域的方法，要求从左上角开始的连续区块数最小。

## 思路
dfs

## AC代码
	得分	100
	CPU使用	0ms
	内存使用	1.0MB
后台数据真的弱。。
```c++
#include <iostream>
#include <cstdio>
#include <cstdlib>
#include <algorithm>
#include <cmath>
#include <vector>
#include <set>
#include <map>
#include <queue>
#include <ctime>
#include <cassert>
#include <complex>
#include <string>
#include <cstring>
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
const int maxn = 106;
int a[maxn][maxn];
int vis[maxn][maxn];
int ans = 0x3f3f3f3f;
int SUM;
int n, m;
bool judge(int x, int y)
{
    if (vis[x][y] == 1 || x >= n || x < 0 || y >= m || y < 0)
        return false;
    return true;
}
void dfs(int temp, int x, int y, int sum)
{

    if (sum == SUM / 2 && temp < ans)
    {
        ans = temp;
        return;
    }
    else if (sum < SUM / 2)
    {
        //printf("%d %d %d\n", x, y, sum);
        vis[x][y] = 1;
        temp++;
        sum += a[x][y];
        if (judge(x + 1, y))
            dfs(temp, x + 1, y, sum);
        if (judge(x - 1, y))
            dfs(temp, x - 1, y, sum);
        if (judge(x, y + 1))
            dfs(temp, x, y + 1, sum);
        if (judge(x, y - 1))
            dfs(temp, x, y - 1, sum);
        temp--;
        vis[x][y] = 0;
        sum -= a[x][y];
    }
}
int main(void)
{

    scanf("%d%d", &m, &n);
    for (int i = 0; i < n; i++)
    {
        for (int j = 0; j < m; j++)
        {
            scanf("%d", &a[i][j]);
            SUM += a[i][j];
        }
    }
    //cout << SUM << endl;
    dfs(0, 0, 0, 0);
    if (ans != 0x3f3f3f3f)
        printf("%d\n", ans);
    else
        puts("0");
    system("pause");
    return 0;
}
```