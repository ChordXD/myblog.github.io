---
title: 【负数01背包】POJ 2184 Cowow Exhibition
copyright: true
date: 2019-07-16 20:18:43
tags:
 - 背包
categories:
 - 动态规划
 - 01背包
keywords:
description:
---

## Description

"Fat and docile, big and dumb, they look so stupid, they aren't much 
fun..." 
- Cows with Guns by Dana Lyons 

The cows want to prove to the public that they are both smart and fun. In order to do this, Bessie has organized an exhibition that will be put on by the cows. She has given each of the N (1 <= N <= 100) cows a thorough interview and determined two values for each cow: the smartness Si (-1000 <= Si <= 1000) of the cow and the funness Fi (-1000 <= Fi <= 1000) of the cow. 

Bessie must choose which cows she wants to bring to her exhibition. She believes that the total smartness TS of the group is the sum of the Si's and, likewise, the total funness TF of the group is the sum of the Fi's. Bessie wants to maximize the sum of TS and TF, but she also wants both of these values to be non-negative (since she must also show that the cows are well-rounded; a negative TS or TF would ruin this). Help Bessie maximize the sum of TS and TF without letting either of these values become negative. 

<!-- more -->

## Input

* Line 1: A single integer N, the number of cows 

* Lines 2..N+1: Two space-separated integers Si and Fi, respectively the smartness and funness for each cow. 
Output

* Line 1: One integer: the optimal sum of TS and TF such that both TS and TF are non-negative. If no subset of the cows has non-negative TS and non- negative TF, print 0. 

## Sample Input
```
5
-5 7
8 -6
6 -3
2 1
-8 -5
```

## Sample Output
```
8
```

## Hint
OUTPUT DETAILS: 

Bessie chooses cows 1, 3, and 4, giving values of TS = -5+6+2 = 3 and TF 
= 7-3+1 = 5, so 3+5 = 8. Note that adding cow 2 would improve the value 
of TS+TF to 10, but the new value of TF would be negative, so it is not 
allowed. 

## Source
USACO 2003 Fall

## 题意
给你n只牛，每只牛有两个属性幽默感和智慧，现在你需要选择一些牛出来使其的幽默感和智慧值之和最大，且所有牛的幽默感和智慧值均不能为负数。

## 思路
这题由于有负数，所以需要将dp数组偏移1e5个范围，使负数的值变为正数。

之后我们就可以把其堪称一个价值为智商，体积是幽默感的01背包或者反过来的01背包都行。
s
但是这里注意一点，若以价值为智商的时候，若智商为正数，则正常按照01背包从后向前遍历dp数组；反之应该从前往后遍历数组。原因很简单，应为01背包保证每个物品只能被使用一次，即任意一个状态被更新之后不得再次被使用，若为正数则从后往前遍历可以保证，若为负数，则转移公式中的dp[j - tf[i]]会向后移动，此时从前往后移动遍历满足01背包要求。

最后从dp[1e5]开始查找最大的背包价值即可，背包价值就是dp值+上数组下标。

## AC代码
```c++
/* ***********************************************
Author :Chord
Email:pengrj2018@gmail.com
Created Time :2019年07月16日 星期二 10时34分30秒
File Name :poj2184.cpp
 ************************************************ */

#include <iostream>
#include <cstdio>
#include <cstdlib>
#include <algorithm>
#include <vector>
#include <queue>
#include <set>
#include <map>
#include <string>
#include <cmath>
#include <cstdlib>
#include <ctime>
#include <cstring>
#include <stack>
#include <bitset>
using namespace std;
typedef long long ll;
typedef pair<int, int> pii;
typedef pair<ll, int> pli;
typedef pair<ll, ll> pll;
const int maxn = 2e5 + 7;
const int INF = 0x3f3f3f3f;
int dp[maxn],PY = 1e5;
int ts[106],tf[106];
void solve(void)
{
	int n;
	while(cin>>n){
		memset(dp,-INF,sizeof dp);
		dp[PY] = 0;
		for(int i = 1 ; i <= n ;  i++){
			cin>>ts[i]>>tf[i];
		}
		for(int i = 1 ; i <= n ; i++){
			if(ts[i] < 0 && tf[i] < 0) continue;
			if(tf[i] < 0)
				for(int j = 0 ; j <= PY*2 + tf[i] ; j++)
					dp[j] = max(dp[j],dp[j - tf[i]] + ts[i]);
			else
				for(int j = PY*2 ; j >= tf[i] ; j--)
					dp[j] = max(dp[j],dp[j - tf[i]] + ts[i]);
		}
		int ans = 0;
		for(int i = PY ; i <= PY*2 ; i++)
			if(dp[i] > 0) ans = max(dp[i] +i - PY , ans);
		cout<<ans<<endl;
	}
}
int main()
{
	//freopen("1.in","r",stdin);
	//freopen("out.txt","w",stdout);
	solve();
	return 0;
}
```