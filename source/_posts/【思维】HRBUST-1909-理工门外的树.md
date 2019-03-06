---
title: 【思维】HRBUST 1909 理工门外的树
copyright: true
date: 2019-03-06 20:29:57
tags:
 - 思维
categories:
 - 思维
keywords:
description:
---
Time Limit: 1000 MS	Memory Limit: 32768 K
## Description
哈尔滨修地铁了~理工门口外长度为N的马路上有一排树，已知两棵树之间的距离都是1m。现在把马路看成是一个数轴，马路的一端在数轴0的位置，另一端在N的位置；数轴上的每个整数点，即0，1，2，……，L，都种有一棵树。马路上有一些区域要用来建地铁，这些区域用它们在数轴上的起始点和终止点表示。已知任一区域的起始点和终止点的坐标都是整数，区域之间可能有重合的部分。现在要把这些区域中的树（包括区域端点处的两棵树）移走。你的任务是计算将这些树都移走后，马路上还有多少棵树。 
<!--more-->
## Input
输入的第一行有两个整数N（1 <= N <= 1,000,000）和M（1 <= M <= 10,000），N代表马路的长度，M代表区域的数目，N和M之间用一个空格隔开。接下来的M行每行包含两个不同整数，用一个空格隔开，表示一个区域的起始点和终止点的坐标。 
## Output
输出包括一行，这一行只包含一个整数，表示马路上剩余的树的数目。 
## Sample Input
	500 3
	150 300
	100 200
	470 471
## Sample Output
	298
## Author
周洲 @hrbust

## 题意
RT，给出m次操作，移除一段区间内的所有树，问剩下多少树。

## 思路
有一种很巧妙的做法
1，标记起点和终点，让其一正一负。
2，设置一个flag，加上所有的点的权值。
若加完之后flag值仍然为0，说明该点不在任何移除区间之内，相反就在。很明显任何一个区间的收尾两端被flag相加之后必然为0。
注意处理一下区间终点即可。因为端点的树也被移走了。

## AC代码
```c++
#include <bits/stdc++.h>
using namespace std;
const int maxn = 1e6 + 7;
int a[maxn];
void solve(void)
{
    int n, m;
    std::ios::sync_with_stdio(false);
    while (cin >> n >> m)
    {
        memset(a, 0, sizeof a);
        while (m--)
        {
            int x, y;
            cin >> x >> y;
            a[x]--;
            a[y]++;
        }
        int sum = 0, flag = 0;
        for (int i = 0; i <= n; i++)
        {
            flag += a[i];
            if (flag == 0)
                sum++;
            if (flag == 0 && a[i] > 0)
                sum--;
        }
        cout << sum << endl;
    }
}
int main(void)
{
    solve();
    return 0;
}
```