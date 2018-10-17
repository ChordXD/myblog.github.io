---
title: 【计算几何/线段相交】POJ 2652 Pick-up sticks
copyright: true
date: 2018-08-23 00:06:07
tags:
 - 计算几何
 - 叉积运用
 - ACM
categories:
 - 计算几何
 - 基本几何学
keywords:
 - POJ 2652 Pick-up sticks
---
*POJ 2652 Pick-up sticks 解题报告。*
<!-- more -->
>Time Limit: 3000 MS Memory Limit: 65536 KB
>64-bit integer IO format: %I64d , %I64u Java class name: Main

## Description
Stan has n sticks of various length. He throws them one at a time on the floor in a random way. After finishing throwing, Stan tries to find the top sticks, that is these sticks such that there is no stick on top of them. Stan has noticed that the last thrown stick is always on top but he wants to know all the sticks that are on top. Stan sticks are very, very thin such that their thickness can be neglected.

## Input
Input consists of a number of cases. The data for each case start with 1 <= n <= 100000, the number of sticks for this case. The following n lines contain four numbers each, these numbers are the planar coordinates of the endpoints of one stick. The sticks are listed in the order in which Stan has thrown them. You may assume that there are no more than 1000 top sticks. The input is ended by the case with n=0. This case should not be processed.

## Output
For each input case, print one line of output listing the top sticks in the format given in the sample. The top sticks should be listed in order in which they were thrown.

The picture to the right below illustrates the first case from input.

![first case](https://chordblog.com/photo/2653_p1.jpg)

## Sample Input
5
1 1 4 2
2 3 3 1
1 -2.0 8 4
1 4 8 2
3 3 6 -2.0
3
0 0 1 1
1 0 2 1
2 0 3 1
0

## Sample Output
Top sticks: 2, 4, 5.
Top sticks: 1, 2, 3.

## Hint
Huge input,scanf is recommended.

## Source
Waterloo local 2005.09.17

-----------------------

## 题意
按顺序往一个平面扔一系列的线段，问最后在顶端（也就是在其之上没有其他线段与其相交）的线段有哪些。

----------------------------

## 思路
最后从下往上枚举所有线段即可，如果在其之上有线段与其相交，则移除这跟线段。
此处的相交是指范式相交，即重合，点相交都算相交。

------------------------------

## 坑点
一开始直接在输入的过程中剔除其下方所有的与其相交的线段WA。
也有可能是我一开始直接使用vector当中的earse函数所致。
据说部分情况下使用g++编译也会WA

-------------------------

## AC代码
```c++
using namespace std;
#include <algorithm>
#include <cmath>
#include <cstdio>
#include <vector>


#define EPS 1e-7

class Point
{
public:
  double x, y;
  Point(double x = 0, double y = 0) : x(x), y(y) {}
  Point operator+(Point a) { return Point(x + a.x, y + a.y); }
  Point operator-(Point a) { return Point(x - a.x, y - a.y); }
  bool operator==(Point a){if(x==a.x&&y==a.y) return true;else return false;}
  double norm(void) {return x*x + y*y;}
};
typedef struct
{
  Point a, b;
  int id;
} Line;
typedef Point Vector;
double cross(Vector a, Vector b)
{
  return a.x * b.y - a.y * b.x;
}

double dot(Vector a,Vector b)
{
  return a.x*b.x + a.y*b.y;
}

int ccw(Point p0,Point p1,Point p2)
{
    Vector a = p1 - p0;
    Vector b = p2 - p0;
    if(cross(a,b)<-EPS) return -1;
    if(cross(a,b)>EPS) return 1;
    if(dot(a,b)<-EPS) return 2;
    if(a.norm() < b.norm()) return -2;
    return 0;
}

bool isinter(Point a,Point b,Point c,Point d)
{
    if(ccw(a,b,c)*ccw(a,b,d)<=0&&ccw(c,d,a)*ccw(c,d,b)<=0)
      return true;
    return false;
}

void solve(void)
{
  int n;
  while(scanf("%d",&n),n)
  {
    vector<Line> p;
    for(int i = 0 ; i < n ; i++)
    {
      Line t;
      scanf("%lf%lf%lf%lf", &t.a.x, &t.a.y, &t.b.x, &t.b.y);
      t.id = i + 1;
      p.push_back(t);
    }
    for (int i = 0; i < p.size() ; i++)
    {
      for (int j = i + 1; j < p.size(); j++)
      {
        if(isinter(p[i].a,p[i].b,p[j].a,p[j].b))
        {
          p[i].id = 0;
          break;
        }
      }
    }
      printf("Top sticks: ");
    for(int i = 0 ; i < p.size() ; i++)
    {
      if(p[i].id!=0)
        printf("%d%c%c", p[i].id, i == p.size() - 1 ? '.' : ',', i != p.size() - 1 ? ' ' : '\n');
    }
  }
}

int main(void)
{
  solve();
  return 0;
}
```