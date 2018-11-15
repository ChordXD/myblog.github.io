---
title: Educational Codeforces Round 54 div.2 A,B,C 解题报告
copyright: true
date: 2018-11-05 00:44:51
tags: 
 - Codeforces
 - ACM
categories:
 - 竞赛题解
 - Codeforcess
keywords: Educational Codeforces Round 54 div.2
---
  
**Educational Codeforces Round 54 (Rated for Div. 2) A B C 解题报告**
 + A、Minimizing the String
 + B、Divisor Subtraction
 + C、Meme Problem

<!-- more -->

----------------------------------------------------------

## A、Minimizing the String
题目连接：[Minimizing the String](http://codeforces.com/contest/1076/problem/A)

### 题意
对于一个字符串，你至多可以删除一个字符，使得新生成的字符串再所有字符串当中字典序最小。

### 思路
模拟删除操作，删除某个字符之后，对于新生成的字符串，在删除之前的字符都必然相同（只能最多删一个），删除之后，被删除位置是被删除字符之后的第一个字符。
我们只要比较这两个字符的大小关系，如果删除的字符大于删除字符之后的第一个字符，那么删除之后字典序变小。
很明显，出现删除的情况越在前，所生成的字符字典序越小。

## 坑点
不是删除的字符越大生成的字符串字典序最小。
如：abaz
很明显，删除第二个b所生成的字符串：aaz 要比删除最大的字符z所生成的字符串：aba 字典序要小。

## AC代码
```c++
#include<bits/stdc++.h>
using namespace std;

int main(void){
    string a;
    int n;
    cin>>n;
    cin>>a;
    int flag = n-1;
    for(int i = 0 ; i  < n-1; i++){
        if(a[i] > a[i+1]){
            flag = i;
            break;
        }

    }
    for(int i = 0 ; i < n ;i++){
        if(i != flag) cout<<a[i];
    }
    cout<<endl;
    return 0;
}
```

-------------------------------------------------

## B、Divisor Subtraction
题目连接：[Divisor Subtraction](http://codeforces.com/contest/1076/problem/B)
### 题意
对于一个整数n，反复进行一下操作：
1、如果n为0，退出。
2、找到n的最小质因子d。
3、n = n-d。

### 思路
若n是偶数，结果是n/2。
因为2是所有偶数的最小质因子，而对于任意偶数减去2之后还是偶数，还会继续减去2，直到减到0为止。操作次数敲好就是n当中2的个数。
如果n是奇数，结果为1 + (n - n的最小质因子)/2。
n是奇数，那么他的最小质因子就一定不是偶数也是个奇数。奇数 - 奇数 = 偶数，这个奇数操作必然只有一次。之后n就是偶数了，操作次数就是偶数的处理方式了。
之后的任务就是找到n的最小质因子，n的数据范围很小，暴力找就好了。

### AC代码
```c++
#include<bits/stdc++.h>
using namespace std;
#define ll long long
int main(void)
{
    std::ios::sync_with_stdio(false);
    ll n;
    cin>>n;
    if(n%2==0) {cout<<n/2<<endl;return 0;}
    for(ll i = 2 ; i*i <= n ; i++){
        if(n%i == 0){
            cout<<1 + (n -i) /2<<endl;
            return 0;
        }
    }
    cout<<1<<endl;
    return 0;
}
```

## C、Meme Problem
题目连接：[Meme Problem](http://codeforces.com/contest/1076/problem/C)
### 题意
对于一个数n，问是否存在两个数a,b,使得a+b=d且a*b=d.

### 思路
联立方程式求解即可，简单数学问题。注意精度。

### AC代码
```c++
#include <bits/stdc++.h>
using namespace std;
#define esp 1e-7
int compare(double a)
{
    if (a < -esp)
        return -1;
    else
        return a > esp;
}
int main(void)
{
    std::ios::sync_with_stdio(false);
    int t;
    cin >> t;
    while (t--)
    {
        double d;
        cin>>d;
        double judge = d*d - 4*d;
        if(compare(judge) < 0){
            cout<<"N"<<endl;
        }
        else{
            judge = sqrt(judge);
            double x1 = (d + judge)/2.0;
           double x2 = d - x1;
           cout<<"Y ";
            cout<<fixed<<setprecision(9)<<x1 <<" "<< x2<<endl;
        }
    }

    return 0;
}
```