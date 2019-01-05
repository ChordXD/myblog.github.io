---
title: CodeForcess Hello 2019 A,B,C 解题报告
copyright: true
date: 2019-01-05 11:17:04
tags: 
 - Codeforces
 - ACM
categories:
 - 竞赛题解
 - Codeforcess
keywords:
description:
---

*CodeForcess Hello 2019 A,B,C 解题报告*
+ A.Gennady and a Card Game
+ B.Petr and a Combination Lock
+ C.Yuhao and a Parenthesis

<!-- more -->
## A.Gennady and a Card Game
题目连接：[http://codeforces.com/contest/1097/problem/A](http://codeforces.com/contest/1097/problem/A)

### 题意
简单题，找到在第二行是否有和第一行给出的牌当中相同的牌。

### 思路
RT

### AC代码
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
    char a[10];
    char b[1000];
    scanf("%s", a);
    getchar();
    gets(b);
    int ak = strlen(a);
    int bk = strlen(b);
    //cout << a << endl;
    //cout << b << endl;
    for (int i = 0; i < bk; i++)
    {
        if (a[0] == b[i])
        {
            cout << "YES" << endl;
            return 0;
        }
    }
    for (int i = 0; i < bk; i++)
    {
        if (a[1] == b[i])
        {
            cout << "YES" << endl;
            return 0;
        }
    }
    cout << "NO" << endl;
   // system("pause");
    return 0;
}
```

## B.Petr and a Combination Lock
题目链接：[http://codeforces.com/contest/1097/problem/B](http://codeforces.com/contest/1097/problem/B)

### 题意
给出每次转动密码盘的度数，问是否有一种转法可以将密码盘转为0.

### 思路
数据量很小，暴力枚举所有方法。

### AC代码
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
int a[maxn];
int n;
int flag = 0;
void bfs(int sum, int i)
{
    if (flag)
        return;
    if (i == n)
    {
        if (sum == 0)
        {
            flag = 1;
            return;
        }
        if (sum % 360 == 0)
            flag = 1;
        return;
    }
    bfs(sum + a[i], i + 1);
    bfs(sum - a[i], i + 1);
    return;
}
int main(void)
{
    std::ios::sync_with_stdio(false);

    cin >> n;
    for (int i = 0; i < n; i++)
    {
        cin >> a[i];
    }
    bfs(0, 0);
    if (flag)
    {
        cout << "YES" << endl;
    }
    else
        cout << "NO" << endl;

    //system("pause");
    return 0;
}
```

## C.Yuhao and a Parenthesis
题目链接：[http://codeforces.com/contest/1097/problem/C](http://codeforces.com/contest/1097/problem/C)

### 题意
给你多个由括号组成的字符串，问最多可以有多少个两两配对的字符串使得新配对的字符串满足括号匹配。

### 思路
先处理每一个字符串，将每一个字符串当中已经配对的括号去除。
对于处理过的字符串，无非分为三种情况：
1. 只需要右括号
2. 只需要左括号
3. 既需要左括号，又需要右括号。
4. 不需要括号，自身已经完全匹配。

+ 对于1，2种情况，我们分别把其加入两个数组当中，之后我们对其中一个数组进行排序，另一个数组只要查询排序好的数组当中有没有何其相同的元素即可，查询到了就是匹配。如我们对于需要右括号的数组进行排序，然后对需要左括号的数组进行遍历，对于每一个需要左括号的元素我们查询是否有相同需要右括号的元素，如果两个元素左右需求相同，则是匹配的，删除这个需要右括号的元素即可，因为这个元素已经匹配过了。
+ 对于第3种情况无论如何也不能匹配，省略。
+ 对于第4种情况，不需要括号的只能匹配不需要括号的元素，我们统计一下这类字符串的个数，最后个数除以2就是这类元素最大匹配的数量。

之后把1，2种情况能匹配到的数量与第四种情况匹配到的数量加和即为结果。

### AC代码
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
#include <stack>
#include <ctime>
#include <cassert>
#include <complex>
#include <string>
#include <cstring>
#include <chrono>
#include <random>
#include <queue>
#include <bitset>
using namespace std;
typedef long long ll;
typedef pair<int, int> pii;
typedef pair<ll, int> pli;
typedef pair<ll, ll> pll;
typedef long double ld;
#define mp make_pair
const int maxn = 5e5 + 7;
char a[maxn];
int main(void)
{
    int n;
    scanf("%d", &n);
    int zero = 0;
    vector<int> need_r;
    vector<int> need_l;
    for (int i = 0; i < n; i++)
    {
        int nr = 0, nl = 0;
        scanf("%s", a);
        int k = strlen(a);
        stack<char> p;
        for (int i = 0; i < k; i++)
        {
            if (a[i] == '(')
            {
                p.push(a[i]);
            }
            else
            {
                if (p.empty())
                {
                    p.push(a[i]);
                }
                else
                {
                    char tt = p.top();
                    if (tt == '(')
                    {
                        p.pop();
                    }
                    else
                    {
                        p.push(a[i]);
                    }
                }
            }
        }
        while (!p.empty())
        {
            char tt = p.top();
            p.pop();
            if (tt == '(')
                nr++;
            else
                nl++;
        }
        if (nr == 0 || nl == 0)
        {
            if (nr == 0 && nl == 0)
                zero++;
            else
            {
                if (nr == 0)
                {
                    need_l.push_back(nl);
                }
                else
                    need_r.push_back(nr);
            }
        }
    }
    int ans = zero / 2;
    sort(need_l.begin(), need_l.end());
    need_l.push_back(99999999);
    for (int i = 0; i < need_r.size(); i++)
    {
        auto it = lower_bound(need_l.begin(), need_l.end(), need_r[i]);
        if (*it == need_r[i])
        {
            ans++;
            need_l.erase(it);
        }
    }
    printf("%d\n", ans);
   // system("pause");
    return 0;
}
```