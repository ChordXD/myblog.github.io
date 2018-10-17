---
title: 【计算几何/线段与直线的位置关系】POJ 3304 Segments
copyright: true
date: 2018-08-19 23:44:26
tags:
 - ACM
 - 计算几何
categories:
 - 计算几何
 - 基本几何学
keywords:
 - POJ 2204
 - 计算几何
 - POJ 3304 Segments
---
*POJ 3304 Segments 解题报告*
<!-- more -->
> Time Limit: 1000 MS Memory Limit: 65536 KB
> 64-bit integer IO format: %I64d , %I64u Java class name: Main

## Description
Given n segments in the two dimensional space, write a program, which determines if there exists a line such that after projecting these segments on it, all projected segments have at least one point in common.

## Input
Input begins with a number T showing the number of test cases and then, T test cases follow. Each test case begins with a line containing a positive integer n ≤ 100 showing the number of segments. After that, n lines containing four real numbers x1 y1 x2 y2 follow, in which (x1, y1) and (x2, y2) are the coordinates of the two endpoints for one of the segments.

## Output
For each test case, your program must output “Yes!”, if a line with desired property exists and must output “No!” otherwise. You must assume that two floating point numbers a and b are equal if |a - b| < 10-8.

## Sample Input
3
2
1.0 2.0 3.0 4.0
4.0 5.0 6.0 7.0
3
0.0 0.0 0.0 1.0
0.0 1.0 0.0 2.0
1.0 1.0 2.0 1.0
3
0.0 0.0 0.0 1.0
0.0 2.0 0.0 3.0
1.0 1.0 2.0 1.0

## Sample Output
Yes!
Yes!
No!

## Source
Amirkabir University of Technology Local Contest 2006

-------------------------------

## 题意
给你一些线段，问是否存在一条直线，使这些线段在直线上的投影都能有一部分重合。

-----------------------------------

## 思路
如果其在某一条线段上的投影都能重合一部分，那么必然有一条直线可以穿过所有线段，不难想出这是个充要条件。那么现在问题就转化成了是否存在一条直线可以穿过所有的线段。

不难发现，一系列线段可以被某一条直线穿过，那么其中一条满足条件的直线必能穿过其中两条线段的两个端点。

由于数据非常小，我们可以暴力枚举所有线段的两个端点，然后用其两个端点构成的直线来去判断是否可以穿过其他的线段即可。

判断线段是否与直线相交用叉积就行。

必然，当n==1或者n==2的时候必然绝对满足题意。

----------------------------------------

{% note warning %}
注意：如果两个线段的某两个端点重合，则得跳过由这两个重合端点所构成的直线。

因为这重合的两点无法构成直线，自然不能考虑。
{% endnote %}

---------------------------------------

## AC代码
```c++
using namespace std;
#include <cstdio>
#include <cmath>
#include <vector>
#define eps 1e-8
int n;
class Point
{
public:
  double x, y;
  Point(double x = 0, double y = 0) : x(x), y(y) {}
  Point operator+(Point a) { return Point(x + a.x, y + a.y); }
  Point operator-(Point a) { return Point(x - a.x, y - a.y); }
};

int sgn(double x)
{
    if(fabs(x) < eps)return 0;
    if(x < 0) return -1;
    return 1;
}

class Line
{
public:
  Line() {}
  Line(Point a, Point b) : x(a), y(b) {}
  Point x, y;
};
typedef Point Vector;
typedef Line Segment;
vector<Segment> p;
double cross(Vector a, Vector b) { return a.x * b.y - a.y * b.x; }

int is_clock(Point p0, Point p1, Point p2)
{
  Vector a = p1 - p0;
  Vector b = p2 - p0;
  // printf("%.2f %.2f\n", a.x, a.y);
  // printf("%.2f %.2f\n", b.x, b.y);
  if (cross(a, b) < -eps)
    return 1;
  if (cross(a, b) > eps)
    return 2;
  return 0;
}

bool line_seg(Line a, Segment p)
{
  if (is_clock(a.x, a.y, p.x) == 0 || is_clock(a.x, a.y, p.y) == 0)
    return true;
  if (is_clock(a.x, a.y, p.x) == 1 && is_clock(a.x, a.y, p.y) == 2)
    return true;
  if (is_clock(a.x, a.y, p.x) == 2 && is_clock(a.x, a.y, p.y) == 1)
    return true;
  return false;
}

double dist(Point a,Point b)
{
  return sqrt(((a.x - b.x)*(a.x - b.x)) + ((a.y - b.y)*(a.y - b.y)));
}

int check(Line now)
{
  if(sgn(dist(now.x,now.y))==0) ///注意所选两点重合的情况。
    return false;
  for (int i = 0; i < n; i++)
  {
    if (line_seg(now, p[i]) == false)
      return false;
  }
  return true;
}

int main(void)
{
  int t;
  scanf("%d", &t);
  while (t--)
  {
    scanf("%d", &n);
    p.clear();
    for (int i = 0; i < n; i++)
    {
      Segment t;
      scanf("%lf%lf%lf%lf", &t.x.x, &t.x.y, &t.y.x, &t.y.y);
      p.push_back(t);
    }
    int flag = 0;
    if (n == 1||n == 2)
    {
        puts("Yes!");
        continue;
    }

    for (int i = 0; i < n; i++)
    {
      for (int j = i + 1; j < n; j++)
      {
        if (check(Line(p[i].x, p[j].x)) || check(Line(p[i].x, p[j].y)) || check(Line(p[i].y, p[j].x)) || check(Line(p[i].y, p[j].y)))
        {
          flag = 1;
          goto mark;
        }
      }
    }
  mark:;
    if (flag)
      puts("Yes!");
    else
      puts("No!");
  }
}
```