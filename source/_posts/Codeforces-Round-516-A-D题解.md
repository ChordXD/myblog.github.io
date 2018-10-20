---
title: 'Codeforces Round #516 (Div. 2, by Moscow Team Olympiad) A~D题解'
copyright: true
date: 2018-10-20 15:42:24
tags:
 - Codeforces
categories:
 - 竞赛题解
 - Codeforcess
keywords:
 - Codeforces Round #516 题解
---

**Codeforces Round #516 (Div.2, by Moscow Team Olympiad) **
A Make a triangle!
B Equations of Mathematical Magic
C Oh Those Palindromes
D Labyrinth
**解题报告**
<!-- more -->

------------------------------------------
## A. Make a triangle!

![A](myblog.github.io/photo/cf1.jpg)

### 题意
给你三角形的三条边，你可以任意缩短任意一条边n厘米，代价是花费n个单位的时间，问使得三根木棒变成一个三角形所需要的最短时间是多少。

### 思路
构成一个三角形无疑使三条边满足 a + b > c && b + c > a && c + a > b.
很明显我们发现我们只需要对这三个三角形排个序，如果最长边直接可以与剩下的两条边构成三角形那么必然满足三角形构成条件。
否则，只需将最长的三角形减去 一定长度 从而小于较短两条边之和即可。
而这个减去的长度就是所需的时间。
反过来也容易想到，最短两条边总的增长上述长度也即可与最长边构成三角形。

### AC代码
```c++
#include<bits/stdc++.h>
using namespace std;

#define ll long long
const int maxn = 1e5 + 7;
int a[maxn];
int main(void)
{
    vector<int>p;
    for(int i = 0 ; i < 3 ; i++)
    {
        int t;
        scanf("%d",&t);
        p.push_back(t);
    }
    sort(p.begin(),p.end());
    if(p[0] + p[1] > p[2])
        puts("0\n");
    else
        printf("%d\n",p[2] - (p[0] + p[1]) + 1);

    return 0;
}
```

## B. Equations of Mathematical Magic
![B](myblog.github.io/photo/cf2.jpg)

### 题意
给你一个方程与一种操作
问对于给你给定参数a的取值，有多少种可行解。

### 思路
不难发现这是一道找规律的题目，答案即是2的(a二进制当中的1的个数)次方.

### AC代码
```c++
#include<bits/stdc++.h>
using namespace std;

#define ll long long
const int maxn = 1e5 + 7;
int a[maxn];

int BitCount2(unsigned ll n)
{
    unsigned ll c =0 ;
    for (c =0; n; ++c)
    {
        n &= (n -1) ; // Çå³ý×îµÍÎ»µÄ1
    }
    return c ;
}

int main(void)
{
    int t;
    scanf("%d",&t);
    while(t--)
    {
        int a;
    scanf("%d",&a);
    ll ans = 1;
    ans<<=BitCount2(a);
    printf("%lld\n",ans);
    }

    return 0;
}
```
## C. Oh Those Palindromes
![C](myblog.github.io/photo/cf3.jpg)

### 题意
给出回文字串的定义，问给你一个字符串，如何构造可以使这个字符串的回文子串的数量最大。

### 思路
万万没想到，sort一遍就是答案。

### AC代码
```c++
#include<bits/stdc++.h>
using namespace std;

int main(void)
{
    string a;
    int n;
    cin>>n>>a;
    sort(a.begin(),a.end());
    cout<<a<<endl;
    //system("pause");
}
```

## D. Labyrinth
![D](myblog.github.io/photo/cf4.jpg)
### 题意
给你一个地图，地图上标记为'.'的点为可以移动的点，起始坐标与你最多可以向左和向右移动的步数，上下移动没有限制。问在这张地图当中，你最多可以访问多少个点。

### 思路
**01BFS**
创建一个双向队列。
对于每个对于整张地图进行bfs，如果需要耗费左右的移动步数的点将其放在队尾，不需要的将其放在对头。
出队时优先出队上下移动的点，即队头元素。

### 坑点
倘若使用一般BFS，会FST。
很明显，对于到达任意一个点，先上下移动比先左右移动的花费要更少一些，所以上下移动更优。

### AC代码
```c++
#include <bits/stdc++.h>
using namespace std;
const int maxn = 2050;
int vis[maxn][maxn];
char Map[maxn][maxn];

int f[4][2] = {{0, -1}, {0, 1}, {1, 0}, {-1, 0}};

int maxnl, maxnr;
int n, m;
int ans;
int startx, starty;

typedef struct
{
    int x, y, l, r;
} poi;

bool judge(poi a)
{
    if (a.x >= 0 && a.y >= 0 && a.x < n && a.y < m && a.l <= maxnl && a.r <= maxnr && vis[a.x][a.y] == 0&& Map[a.x][a.y] == '.')
        return true;
    return false;
}

void bfs(void)
{
    memset(vis, 0, sizeof vis);
    poi first;
    first.x = startx;
    first.y = starty;
    first.l = first.r = 0;
    ans = 0;
    deque<poi> p;
    p.push_front(first);
    vis[first.x][first.y] = 1;
    while(!p.empty()){
        poi then = p.front();
        p.pop_front();
        //printf("%d %d\n",then.x,then.y);
        ans++;
        for( int i = 0 ; i < 4 ; i++ )
        {
            poi Next;
            Next.x = then.x + f[i][0];
            Next.y = then.y + f[i][1];
            Next.l = then.l;
            Next.r = then.r;
            if(i == 0) Next.l = then.l + 1;
            else if(i == 1) Next.r = then.r + 1;
            if(judge(Next))
            {
                vis[Next.x][Next.y] = 1;
                if(i == 0|| i == 1)
                    p.push_back(Next);
                else
                    p.push_front(Next);
            }
        }
    }
}

int main(void)
{
    scanf("%d%d%d%d%d%d",&n,&m,&startx,&starty,&maxnl,&maxnr);
    startx--;
    starty--;
    for(int i = 0 ; i < n ; i ++)
        scanf("%s",Map[i]);
    bfs();
  //  for(int i = 0 ; i < n ; i ++ )
  //  {
  //      for(int j = 0 ; j < m ; j ++)
  //          printf("%d ",vis[i][j]);
  //      printf("\n");
  // }
    printf("%d\n",ans);
  //  system("pause");
    return 0;
}
```