---
title: 【gcd/lcm】蓝桥杯2013 核桃的数量
copyright: true
date: 2019-02-03 21:06:32
tags:
 - 蓝桥杯
 - 数论
categories:
 - 蓝桥杯
keywords:
description:
---

时间限制：1.0s   内存限制：256.0MB

## 问题描述

小张是软件项目经理，他带领3个开发组。工期紧，今天都在加班呢。为鼓舞士气，小张打算给每个组发一袋核桃（据传言能补脑）。他的要求是：
1. 各组的核桃数量必须相同
2. 各组内必须能平分核桃（当然是不能打碎的）
3. 尽量提供满足1,2条件的最小数量（节约闹革命嘛）

<!-- more -->

## 输入格式
输入包含三个正整数a, b, c，表示每个组正在加班的人数，用空格分开（a,b,c<30）

## 输出格式
输出一个正整数，表示每袋核桃的数量。

## 样例输入1
	2 4 5

## 样例输出1
	20

## 样例输入2
	3 1 1

## 样例输出2
	3

## 题意
RT

## 思路
满足题意条件的数量即为这三个数的最小公倍数。

## AC代码
	得分	100
	CPU使用	437ms
	内存使用	828.0KB
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
const int maxn = 1e5 + 7;

int gcd(int a, int b)
{
    if (b == 0)
        return a;
    else
        return gcd(b, a % b);
}

int lcm(int a, int b)
{
    if (a < b)
        swap(a, b);
    return a * b / gcd(a, b);
}

int main(void)
{
    int a, b, c;
    cin >> a >> b;
    int ans = lcm(a, b);
    cin >> c;
    ans = lcm(ans, c);
    cout << ans << endl;
    //system("pause");
    return 0;
}
```