---
title: 【博弈】HDU 2147 kiki's game
copyright: true
date: 2019-01-11 21:33:51
tags:
 - 巴士博弈
categories:
 - 博弈
 - 巴士博弈
keywords:
description:
---

# kiki's game
Time Limit: 5000/1000 MS (Java/Others)    Memory Limit: 40000/10000 K (Java/Others)

## Problem Description
Recently kiki has nothing to do. While she is bored, an idea appears in his mind, she just playes the checkerboard game.The size of the chesserboard is n*m.First of all, a coin is placed in the top right corner(1,m). Each time one people can move the coin into the left, the underneath or the left-underneath blank space.The person who can't make a move will lose the game. kiki plays it with ZZ.The game always starts with kiki. If both play perfectly, who will win the game?
<!-- more -->

## Input
Input contains multiple test cases. Each line contains two integer n, m (0<n,m<=2000). The input is terminated when n=0 and m=0.

## Output
If kiki wins the game printf "Wonderful!", else "What a pity!".

## Sample Input
5 3
5 4
6 6
0 0

## Sample Output
What a pity!
Wonderful!
Wonderful!

## Author
月野兔

## Source
HDU 2007-11 Programming Contest

## 题意
博弈，从左上角开始走，每次可以向左，左下，下方移动一格，且最后不能再走的人输，问kiki先手的情况下是否会赢得这次游戏。

## 思路
观察后不难发现，所有的必败点（前一个人必胜）的位置均为行列都为基数的点，所以只用判断起始点是否在必败点即可。

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
const int maxn = 1e5 + 7;
int main(void)
{
    int a, b;
    while (cin >> a >> b)
    {
        if (a == 0 && b == 0)
            break;
        if (a % 2 == 1 && b % 2 == 1)
        {
            cout << "What a pity!" << endl;
        }
        else
            cout << "Wonderful!" << endl;
    }
    //system("pause");
    return 0;
}
```