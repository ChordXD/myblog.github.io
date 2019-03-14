---
title: Educational Codeforces Round 61 ABCDF题解
copyright: true
date: 2019-03-14 21:51:53
tags:
categories:
 -竞赛题解
 - Codeforcess 
keywords:
description:
---

Educational Codeforces Round 61 ABCDF题解
<!-- more -->
## A.Regular Bracket Sequence
### 题意
给你四种括号的数量，问是否存在一种组合方式让所有的括号匹配
### 思路
水题，怎么操作都可以。
### AC代码
```c++
#include <bits/stdc++.h>
using namespace std;
#define ll long long
int main(void)
{
  ll l = 0, r = 0;
  int a, b, c, d;
  cin >> a >> b >> c >> d;
  l = a * 2 + b + c;
  r = b + c + d * 2;
  if (a == 0 && a == b && b == c && c == d)
    puts("1");
  else if (r == l)
  {
    if (c != 0 && (a == 0 || d == 0))
      puts("0");
    else
      puts("1");
  }
  else
    puts("0");
  // system("pause");
  return 0;
}
```
## B.Discounts
### 题意
给你一系列的糖果，你有一些优惠券，你可以选择等同于某张优惠券相同数字个的糖果，并将其中最便宜的那个糖果免费拿，之后其他糖果全价。问对于不同数字的优惠券，最便宜的购买方案是什么。
### 思路
水题，排个序然后选则比较贵的前几个就行。
### AC代码
```c++
#include <bits/stdc++.h>
using namespace std;
#define ll long long
const int maxn = 3 * 1e5 + 7;
ll p[maxn];
ll aa[maxn];
int main(void)
{
    ll n, m;
    ll ans = 0;
    scanf("%lld", &n);
    for (int i = 0; i < n; i++)
    {
        scanf("%lld", &p[i]);
        ans += p[i];
    }
    sort(p, p + n, greater<ll>());
    scanf("%lld", &m);

    for (int i = 0; i < m; i++)
    {
        scanf("%lld", &aa[i]);
    }
    for (int i = 0; i < m; i++)
    {
        printf("%lld\n", ans - p[aa[i] - 1]);
    }
    // system("pause");
    return 0;
}
```
## C.Painting the Fence
### 题意
有一些油漆工刷模板，每个人刷的范围不一样，现在撤掉两个油漆工，问撤掉之后最大能刷的面积是都少。
### 思路
先记录木板的每个部位被哪些油漆工刷了。然后统计每个油漆工对于只有一层油漆被刷的墙面的贡献，若一块墙面被两个油漆工刷了，统计一下这两个油漆工对于这块墙面的共同贡献。
之后暴力枚举一波，若删掉任意两个油漆工会让墙面少多少。少掉的数量就是：这两个油漆工对于当且仅当刷了一面墙的贡献加上他们俩合力刷了两面墙的贡献。

为什么超过三个油漆工刷过的墙面不统计？因为刷过三次的墙面怎么算贡献删掉两个油漆工都不会对这面墙有影响。
### AC代码
```c++
#include <bits/stdc++.h>
using namespace std;
const int maxn = 5e3 + 7;
int poi[maxn][3];
int one[maxn], two[maxn][maxn];
int main(void)
{
    std::ios::sync_with_stdio(false);
    int n, m;
    cin >> n >> m;
    for (int i = 0; i < m; i++)
    {
        int x, y;
        cin >> x >> y;
        for (int j = x; j <= y; j++)
        {
            if (poi[j][0] < 2)
            {
                poi[j][++poi[j][0]] = i;
            }
            else
                poi[j][0]++;
        }
    }
    int sum = 0;
    for (int i = 1; i <= n; i++)
    {
        int t = poi[i][0];
        //  cout << t << endl;
        if (t)
            sum++;
        if (t == 1)
            one[poi[i][1]]++;
        if (t == 2)
            two[poi[i][1]][poi[i][2]]++;
    }
    int cnt = 0x3f3f3f;
    for (int i = 0; i < m; i++)
    {
        for (int j = i + 1; j < m; j++)
        {
            cnt = min(cnt, one[i] + one[j] + two[i][j]);
        }
    }
    cout << sum - cnt << endl;
    //system("pause");
    return 0;
}
```
## D.Stressful Training
### 题意
有N个学生，每个学生的电脑初始有一定量的电量，有一个充电器，每次可以给一个学生的电脑补充一定量的电，问最少需要每次补充的电量为多少即可让所有的学生都能挺过M的时间。
### 思路
二分寻找这个最小的充电电量，然后暴力贪心模拟，每次给电最少的学生充电。

怎么模拟呢？用有限队列维护每个学生电脑的电量，每个学生的电脑充一次电之后都可以继续运行一段的时间，每次统计从开始到每次充电之后学生电脑的总电量，除以耗电量，就是学生可以从开始坚持多少时间。然后若一开始学生坚持的时间小于当前时间，无疑是学生撑不到这个点了，返回false，若电量最少的学生能坚持的时间都比总时间长，那么直接范围true。可以完成模拟就说明当前这种充电的电量可以让每个学生坚持到最后，那么返回true。
### AC代码
ps: 2994 ms
```c++
#include <bits/stdc++.h>
using namespace std;
const int maxn = 3e5 + 7;
#define ll long long
ll a[maxn], b[maxn], n, k;
typedef struct node
{
    ll time, id, power;
    bool operator<(const node &a) const
    {
        return this->time == a.time ? this->id > a.id : this->time > a.time;
    }
} poi;

bool check(ll x)
{
    priority_queue<node> p;
    for (int i = 1; i <= n; i++)
    {
        p.push(poi{a[i] / b[i], (ll)i, a[i]});
    }
    for (int i = 0; i < k; i++)
    {
        poi t = p.top();
        p.pop();
        if (t.time < i)
            return false;
        if (t.time >= k)
            break;
        t.power += x;
        t.time = t.power / b[t.id];
        p.push(t);
    }
    return true;
}

int main(void)
{
    std::ios::sync_with_stdio(false);
    cin >> n >> k;
    for (int i = 1; i <= n; i++)
        cin >> a[i];
    for (int i = 1; i <= n; i++)
        cin >> b[i];
    ll l = 0, r = 1e13;
    while (l <= r)
    {
        ll mid = (l + r) >> 1;
        if (check(mid))
            r = mid - 1;
        else
            l = mid + 1;
    }
    if (r == 1e13)
        cout << "-1" << endl;
    else
        cout << l << endl;
    return 0;
}
```
## F.Clear the String
### 题意
给你一个字符串，每次可以删除一些相同的连续子串，问最少删除几次就可以把整个字符串完全删除。
### 思路
区间DP，挺简单的。
`dp[i][j] = min(dp[i][j], dp[i][mid] + dp[mid + 1][j]);`
同时，当区间两端相等的时候，可以免费删除一次。
### AC代码
```c++
#include <bits/stdc++.h>
using namespace std;
const int maxn = 1e3 + 7;
char a[maxn];
int dp[maxn][maxn];
void solve(void)
{
    int n;
    scanf("%d", &n);
    scanf("%s", a + 1);
    memset(dp, 0x3f, sizeof dp);
    for (int i = 1; i <= n; i++)
        dp[i][i] = 1;
    for (int size = 1; size <= n; size++)
    {
        for (int i = 1; i + size <= n; i++)
        {
            int j = i + size;
            if (a[i] == a[j])
                dp[i][j] = min(dp[i][j - 1], dp[i + 1][j]);
            for (int mid = i; mid <= j; mid++)
            {
                dp[i][j] = min(dp[i][j], dp[i][mid] + dp[mid + 1][j]);
            }
        }
    }
    printf("%d\n", dp[1][n]);
}
int main(void)
{
    solve();
    return 0;
}
```





*不能再沉迷与拯救华盛顿了啊！！！！*

