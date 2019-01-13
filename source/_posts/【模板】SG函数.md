---
title: 【模板】SG函数
copyright: true
date: 2019-01-13 11:47:00
tags:
 - 模板
categories:
 - 模板
keywords:
description:
---

先挖个坑，自己写的博弈板子。
<!-- more -->
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
int sg[maxn];
int t[maxn];
int f[maxn] = {1, 3, 4};
int p = 3;
void getSG(int n)
{
    sg[0] = 0;
    for (int i = 1; i <= n; i++)
    {
        memset(t, 0, sizeof t);
        for (int j = 0; j < p; j++)
        {
            if (i - f[j] >= 0)
                t[sg[i - f[j]]]++;
        }
        for (int j = 0;; j++)
        {
            if (!t[j])
            {
                sg[i] = j;
                break;
            }
        }
    }
}
int main(void)
{
    getSG(7);
    for (int i = 0; i <= 7; i++)
    {
        cout << sg[i] << endl;
    }
    system("pause");
    return 0;
}
```