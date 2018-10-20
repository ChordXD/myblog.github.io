---
title: 【计算几何/叉积+二分】POJ 2318 TOYS
copyright: true
date: 2018-08-18 23:26:11
tags: 
 - 计算几何
 - 叉积运用
 - 二分
 - ACM
categories:
 - 计算几何
 - 基本几何学
keywords: 
 - POJ 2318
 - POJ 2319
 - 计算几何例题
 - Chord
---

*POJ 2318 解题报告 /附赠 POJ 2319 AC代码和思路*
<!-- more -->
> Time Limit: 2000 MS Memory Limit: 65536 KB
> 64-bit integer IO format: %I64d , %I64u Java class name: Main

## Description
Calculate the number of toys that land in each bin of a partitioned toy box.
Mom and dad have a problem - their child John never puts his toys away when he is finished playing with them. They gave John a rectangular box to put his toys in, but John is rebellious and obeys his parents by simply throwing his toys into the box. All the toys get mixed up, and it is impossible for John to find his favorite toys.

John’s parents came up with the following idea. They put cardboard partitions into the box. Even if John keeps throwing his toys into the box, at least toys that get thrown into different bins stay separated. The following diagram shows a top view of an example toy box.

For this problem, you are asked to determine how many toys fall into each partition as John throws them into the toy box.

## Input
The input file contains one or more problems. The first line of a problem consists of six integers, n m x1 y1 x2 y2. The number of cardboard partitions is n (0 < n <= 5000) and the number of toys is m (0 < m <= 5000). The coordinates of the upper-left corner and the lower-right corner of the box are (x1,y1) and (x2,y2), respectively. The following n lines contain two integers per line, Ui Li, indicating that the ends of the i-th cardboard partition is at the coordinates (Ui,y1) and (Li,y2). You may assume that the cardboard partitions do not intersect each other and that they are specified in sorted order from left to right. The next m lines contain two integers per line, Xj Yj specifying where the j-th toy has landed in the box. The order of the toy locations is random. You may assume that no toy will land exactly on a cardboard partition or
outside the boundary of the box. The input is terminated by a line consisting of a single 0.

## Output
The output for each problem will be one line for each separate bin in the toy box. For each bin, print its bin number, followed by a colon and one space, followed by the number of toys thrown into that bin. Bins are numbered from 0 (the leftmost bin) to n (the rightmost bin). Separate the output of different problems by a single blank line.

## Sample Input
5 6 0 10 60 0
3 1
4 3
6 8
10 10
15 30
1 5
2 1
2 8
5 5
40 10
7 9
4 10 0 10 100 0
20 20
40 40
60 60
80 80
5 10
15 10
25 10
35 10
45 10
55 10
65 10
75 10
85 10
95 10
0

## Sample Output
0: 2
1: 1
2: 1
3: 1
4: 0
5: 1

0: 2
1: 2
2: 2
3: 2
4: 2

## Hint
As the example illustrates, toys that fall on the boundary of the box are “in” the box.

## Source
Rocky Mountain 2003

-----------------------------------------

## 题意
给你一个箱子的位置，并给你其中用来分隔的纸板的两头坐标，然后给你一些点集，问你这些点集落在由纸板所划分出的箱子区域的哪一个，最后输出分割区域当中的每一个区域里的点集数量。

----------------------------------------

## 思路
对于任何一个点，要么在一个纸板的左边，要么在一个纸板的右边，我们只要保证所有纸板都是一个方向的，就可以用叉积判断出任何一个点对于任意一个纸板其的相对位置。纸板顺序又是一定的，不难看出我们可以使用二分查找快速找到与点最贴近的纸板的位置。

注意一个细节：对于二分查找结果的操作，我们只要让最后查找完成为l或者下标为r的纸板与点再次运用叉积进行判断左右即可。由于不难发现分隔出来的箱子的编号与其右边最近的纸板的编号一致，在最接近纸板的左侧即 ans[l]++，否则ans[l+1]++。

--------------------------------

## 坑点
题目要求每一组数据之后换行，请务必注意。

--------------------------------------

## AC代码
```c++
using namespace std;
#include <cstdio>
#include <vector>
#include <cstring>
#define maxn 5006
int box[maxn];
class Point
{
  public:
    double x, y;
    Point(double x = 0, double y = 0) : x(x), y(y) {}
    Point operator+(Point a) { return Point(x + a.x, y + a.y); }
    Point operator-(Point a) { return Point(x - a.x, y - a.y); }
};

typedef Point Vector;
typedef struct line
{
    Point f, s;
} Line;

double cross(Vector a, Vector b)
{
    return a.x * b.y - a.y * b.x;
}

bool isclock(Point p0, Point p1, Point p2)
{
    Vector a = p1 - p0;
    Vector b = p2 - p0;
    if (cross(a, b) < 0)
        return true;
    else
        return false;
}

int main(void)
{
    int n, m;
    Point a, b;
    while (scanf("%d",&n),n)
    {
        scanf("%d%lf%lf%lf%lf", &m, &a.x, &a.y, &b.x, &b.y);
        vector<Line> lline;
        for (int i = 0; i < n; i++)
        {
            Line t;
            double u,l;
            scanf("%lf%lf", &u,&l);
            t.f.x = l;
            t.f.y = b.y;
            t.s.x = u;
            t.s.y = a.y;
            lline.push_back(t);
        }
        vector<Point> ppoint;
        for (int i = 0; i < m; i++)
        {
            Point t;
            scanf("%lf%lf", &t.x, &t.y);
            ppoint.push_back(t);
        }
        memset(box, 0, sizeof box);
        for (int i = 0; i < ppoint.size(); i++)
        {
            int mid;
            int l = 0, r = lline.size() - 1;
            while (l < r)
            {
                mid = (l + r) / 2;
                if (isclock(lline[mid].f, lline[mid].s, ppoint[i]))
                    l = mid + 1;
                else
                    r = mid - 1;
            }
            if(isclock(lline[l].f,lline[l].s,ppoint[i]))
            {
                box[l + 1]++;
            }
            else
                box[l]++;
        }
        //printf("yes\n");
        for(int i = 0 ; i <= n ;i++)
        {
            printf("%d: %d\n", i, box[i]);
        }
        puts("");
    }
    return 0;
}
```

----------------------------------

同时，POJ 2398 也是相同的解法。

但是 POJ 2398中间插入的纸板顺序不确定

所以要排序之后再二分。

2398要求按顺序输出玩具数量为k(k!=0)的盒子的数量，用map再处理一波即可。

-----------------------------------

下面给出POJ 2398 的AC代码

```c++
using namespace std;
#include <cstdio>
#include <vector>
#include <cstring>
#include <map>
#include <algorithm>
#define maxn 5006
int box[maxn];

class Point
{
  public:
    double x, y;
    Point(double x = 0, double y = 0) : x(x), y(y) {}
    Point operator+(Point a) { return Point(x + a.x, y + a.y); }
    Point operator-(Point a) { return Point(x - a.x, y - a.y); }
};

typedef Point Vector;
typedef struct line
{
    Point f, s;
    bool operator < (const line &a) const
    {
        if(f.x == a.f.x)
            return s.x < a.s.x;
        else
            return s.x < a.s.x;
    }
} Line;

double cross(Vector a, Vector b)
{
    return a.x * b.y - a.y * b.x;
}

bool isclock(Point p0, Point p1, Point p2)
{
    Vector a = p1 - p0;
    Vector b = p2 - p0;
    if (cross(a, b) < 0)
        return true;
    else
        return false;
}

int main(void)
{
    int n, m;
    Point a, b;
    while (scanf("%d",&n),n)
    {
        scanf("%d%lf%lf%lf%lf", &m, &a.x, &a.y, &b.x, &b.y);
        vector<Line> lline;
        for (int i = 0; i < n; i++)
        {
            Line t;
            double u,l;
            scanf("%lf%lf", &u,&l);
            t.f.x = l;
            t.f.y = b.y;
            t.s.x = u;
            t.s.y = a.y;
            lline.push_back(t);
        }
        sort(lline.begin(), lline.end());
        vector<Point> ppoint;
        for (int i = 0; i < m; i++)
        {
            Point t;
            scanf("%lf%lf", &t.x, &t.y);
            ppoint.push_back(t);
        }
        memset(box, 0, sizeof box);
        for (int i = 0; i < ppoint.size(); i++)
        {
            int mid;
            int l = 0, r = lline.size() - 1;
            while (l < r)
            {
                mid = (l + r) / 2;
                if (isclock(lline[mid].f, lline[mid].s, ppoint[i]))
                    l = mid + 1;
                else
                    r = mid - 1;
            }
            if(isclock(lline[l].f,lline[l].s,ppoint[i]))
            {
                box[l + 1]++;
            }
            else
                box[l]++;
        }
        //printf("yes\n");
        int maxnum = -0x3f3f3f3f;
        map<int, int>q;
        for(int i = 0 ; i <= n ;i++)
        {
            q[box[i]]++;
            if(box[i] > maxnum)
                maxnum = box[i];
        }
        printf("Box\n");
        for(int i = 1 ; i <= maxnum ; i++)
        {
            if(q[i]!=0)
            {
                printf("%d: %d\n",i,q[i]);
            }
        }
    }
    return 0;
}
```