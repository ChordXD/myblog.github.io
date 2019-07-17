---
title: 【第k大01背包】HDU Bone Collector II
copyright: true
date: 2019-07-17 09:52:39
tags:
 - 背包
categories:
 - 动态规划
 - 01背包
keywords:
description:
---

Time Limit: 5000/2000 MS (Java/Others)    Memory Limit: 32768/32768 K (Java/Others)

## Problem Description
The title of this problem is familiar,isn't it?yeah,if you had took part in the "Rookie Cup" competition,you must have seem this title.If you haven't seen it before,it doesn't matter,I will give you a link:

Here is the link:http://acm.hdu.edu.cn/showproblem.php?pid=2602

Today we are not desiring the maximum value of bones,but the K-th maximum value of the bones.NOTICE that,we considerate two ways that get the same value of bones are the same.That means,it will be a strictly decreasing sequence from the 1st maximum , 2nd maximum .. to the K-th maximum.

If the total number of different values is less than K,just ouput 0.

<!-- more -->

## Input
The first line contain a integer T , the number of cases.
Followed by T cases , each case three lines , the first line contain two integer N , V, K(N <= 100 , V <= 1000 , K <= 30)representing the number of bones and the volume of his bag and the K we need. And the second line contain N integers representing the value of each bone. The third line contain N integers representing the volume of each bone.

## Output
One integer per line representing the K-th maximum of the total value (this number will be less than 231).

## Sample Input
```
3
5 10 2
1 2 3 4 5
5 4 3 2 1
5 10 12
1 2 3 4 5
5 4 3 2 1
5 10 16
1 2 3 4 5
5 4 3 2 1
```

## Sample Output
```
12
2
0
```

## Author
teddy

## Source
百万秦关终属楚

## 题意
求01背包的第k大值。

## 思路
不断维护每个体积背包的前K大值的堆，反复从两个背包中的K大堆中选出前k大，形成一个新堆。
复杂度是O(NMK)。

## AC代码
```c++
#include<bits/stdc++.h>
using namespace std;
const int maxn = 1e4 + 7;
int dp[maxn][40],s1[maxn],s2[maxn],tj[maxn],jz[maxn],n,m,k;

void solve(void)
{
    int t;
    cin>>t;
    while(t--){
        cin>>n>>m>>k;
        for(int i = 1 ; i <= n ; i++ )
            cin>>jz[i];
        for(int i = 1 ; i <= n ; i++)
            cin>>tj[i];
        memset(dp,0,sizeof dp);
        memset(s1,0,sizeof s1);
        memset(s2,0,sizeof s2);
        //cout<<"yes"<<endl;
        for(int i = 1 ; i <= n ; i++)
            for(int j = m ; j >= tj[i] ; j--){
                for(int p = 1 ; p <= k ; p++){
                    s1[p] = dp[j][p];
                    s2[p] = dp[j - tj[i]][p] + jz[i];
                }
                /*防止对比数组越界*/
                s1[k+1] = -1;
                s2[k+1] = -1;
                int a = 1,b = 1,op = 1;
                while(op <= k && (a <= k || b <= k)){
                    if(s1[a] > s2[b]) dp[j][op] = s1[a++];
                    else dp[j][op] = s2[b++];
                    if(dp[j][op] != dp[j][op - 1] || op == 1)
                        op++;
                }
            }
        cout<<dp[m][k]<<endl;
    }
}

int main(void)
{
    solve();
    return 0;
}
```