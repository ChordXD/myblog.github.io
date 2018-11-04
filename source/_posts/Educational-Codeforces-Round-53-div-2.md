---
title: Educational Codeforces Round 53 div.2 A,B,D 解题报告
copyright: true
date: 2018-11-05 00:44:51
tags: 
 - Codeforces
 - ACM
categories:
 - 竞赛题解
 - Codeforcess
keywords: Educational Codeforces Round 53 div.2
---
  
**Educational Codeforces Round 53 (Rated for Div. 2) A B D 解题报告**
 + A、Diverse Substring
 + B、Vasya and Books
 + D、Berland Fair

<!-- more -->
## A、Diverse Substring
题目连接：[Diverse Substring](http://codeforces.com/contest/1073/problem/A)

### 题意
从字符串中找到一个子串，使得子串当中的所有字母出现的次数不多于这个子串的长度的一半。

### 思路
想偏了不好做，其实只要找到一个长度为2的子串，两个字母都不相同的说就满足题目题意了。
如果有更长的子串，那其中也一定至少存在一个长度为二而且两个字母不相同的子串。

### AC代码
```c++
#include<bits/stdc++.h>
using namespace std;

int main(void)
{
   string a;
   int n;
   cin>>n;
   cin>>a;
   for(int i = 0 ; i < n - 1 ; i++)
   {
        if(a[i]!=a[i+1])
        {
            cout<<"YES"<<endl;
            cout<<a.substr(i,2);
            return 0;
        }
   } 
   cout<<"NO"<<endl;
   //system("pause");
   return 0;
}
```

## B、Vasya and Books
题目连接：[Vasya and Books](http://codeforces.com/contest/1073/problem/B)

### 题意
有n本书籍按照一定的顺序排放，你要按照一个新的取书顺序拿书，为了拿到书，你不得不每次把要取书之前的书全部取走，现在问你按照新的取书顺序，这n次取书每次都拿走了多少本书。如果某一次取的书已经被取走了，那么这一次输出0即可。

### 思路
模拟，怎么模拟都可以。
这里用一个数组标记每本书是否被取走，如果取走为0，没取走为1，然后使用一个标记，表示最上面一本书的位置。
每次先检查第i本要取的书是否被取走，取走就直接push 0，没取走从没被取走的最上面一本书的标记位置开始，一直计数到我们要取的书，对于不是我们要取的书，我们把标记数组设为0，因为既然不是我们要的书，那么一定在我们要取的书之上，这一次一定要将其取走；对于是我们要取的书，更新最上一本书的标记，把这轮取的书的数量Push到答案里面。

### AC代码
```c++
#include<bits/stdc++.h>
using namespace std;
const int maxn = 1e6 + 7;
#define ll long long
int a[maxn];
int b[maxn];
int vis[maxn];
int main(void)
{
    int n;
    scanf("%d",&n);
    vector<int>ans;
    int flag = 0;
    for(int i = 0 ; i < n ; i++) scanf("%d",&a[i]);
    for(int i = 0 ; i < n ; i++) scanf("%d",&b[i]);
    memset(vis,1,sizeof(vis));
    for(int i = 0 ; i < n ; i++)
    {
        if(vis[b[i]] == 0)
        {
            ans.push_back(0);
            continue;
        }
        int t = 0;
        int g = b[i];
        for(int i = flag ; i < n ; i++)
        {
            if(a[i]!=g)
            {
                vis[a[i]] = 0;
                flag++;
                t++;
            }
            else
            {
                vis[a[i]] = 0;
                break;
            }
        }
        if(i == 0) ans.push_back(t + 1);
        else
            ans.push_back(t);
    }
    for(int i = 0 ; i < n ; i++)
        printf("%d%c",ans[i],i == n-1 ? '\n' : ' ');
    return 0;
}
```

## D、Berland Fair
题目连接：[Berland Fair](http://codeforces.com/contest/1073/problem/D)

### 题意
初始有一定数量的钱，有n个摊位，每次顺时针到每个摊位取购买商品，一直顺时针走遍所有摊位，直到钱不够买任何一个摊位的物品为止，问一共买了多少次。

### 思路
常规做法必定TLE
我们只需看每轮能买多少个摊位a，同时将钱数减去这一轮每个能买的摊位的花费。
如果a为0，则说明一轮都不行，那么结束购买。
然后对于每轮 我们看剩余的钱可以进行多少次这样的轮数b
然后将钱数减去 b\*这一轮的花费。购买数加上 b\*这一轮能买的摊位的数量。
反复执行，由于缩短了每一种不同摊位购买的模式的重复计算数量，使得循环能在O(n)复杂度内结束。最多也就有n种不同的可购买店铺数量。每次循环至少可以把一家店买到不能买的模式。故最多有n中模式。

### AC代码
```c++
#include<bits/stdc++.h>
using namespace std;
const int maxn = 2e5 + 7;
unsigned long long a[maxn];
int vis[maxn];
int main(void)
{
    int n;
    unsigned long long int T;
    scanf("%d%llu",&n,&T);
    unsigned long long int sum = 0;
    for(int i = 0 ; i < n ; i++)
    {
        scanf("%llu",a + i);
    }
    long long int ans = 0;
    while(1)
    {
        int s = 0;
        unsigned long long int tsum = 0;
        for(int i = 0 ; i < n ; i++)
        {
            if(T >= a[i])
            {
                s++;
                tsum += a[i];
                T-=a[i];
            }
        }
       // printf("%d\n",T);
        if(!s) break;
        ans+=s;
        ans+=(T/tsum)*s;
        T-=(T/tsum) * tsum;
    }
    printf("%lld\n",ans);
  //  system("pause");
    return 0;
}
```
