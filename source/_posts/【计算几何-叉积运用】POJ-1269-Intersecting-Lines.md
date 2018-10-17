---
title: 【计算几何/叉积运用】POJ 1269 Intersecting Lines
copyright: true
date: 2018-08-20 23:55:39
tags:
 - ACM
 - 计算几何
 - 叉积运用
categories:
 - 计算几何
 - 基本几何学
keywords:
 - POJ 1269 Intersecting Lines
 - 计算几何
 - Chord
---

*POJ 1269 Intersecting Lines 解题报告*
<!-- more -->
> Time Limit: 1000 MS
>Memory Limit: 10000 KB

## Description
We all know that a pair of distinct points on a plane defines a line and that a pair of lines on a plane will intersect in one of three ways: 1) no intersection because they are parallel, 2) intersect in a line because they are on top of one another (i.e. they are the same line), 3) intersect in a point. In this problem you will use your algebraic knowledge to create a program that determines how and where two lines intersect.

Your program will repeatedly read in four points that define two lines in the x-y plane and determine how and where the lines intersect. All numbers required by this problem will be reasonable, say between -1000 and 1000.

## Input
The first line contains an integer N between 1 and 10 describing how many pairs of lines are represented. The next N lines will each contain eight integers. These integers represent the coordinates of four points on the plane in the order x1y1x2y2x3y3x4y4. Thus each of these input lines represents two lines on the plane: the line through (x1,y1) and (x2,y2) and the line through (x3,y3) and (x4,y4). The point (x1,y1) is always distinct from (x2,y2). Likewise with (x3,y3) and (x4,y4).

## Output
There should be N+2 lines of output. The first line of output should read INTERSECTING LINES OUTPUT. There will then be one line of output for each pair of planar lines represented by a line of input, describing how the lines intersect: none, line, or point. If the intersection is a point then your program should output the x and y coordinates of the point, correct to two decimal places. The final line of output should read “END OF OUTPUT”.

##Sample Input

5

0 0 4 4 0 4 4 0
5 0 7 6 1 0 2 3
5 0 7 6 3 -6 4 -3
2 0 2 27 1 5 18 5
0 3 4 0 1 2 2 5

## Sample Output
INTERSECTING LINES OUTPUT
POINT 2.00 2.00
NONE
LINE
POINT 2.00 5.00
POINT 1.07 2.20
END OF OUTPUT

## Source
Mid-Atlantic 1996

-------------------------

## 题意
给你两条直线，询问这两条直线的位置关系。
如果相交，输出相交点的坐标。

--------------------------------------

## 思路
如果两条直线平行那么这两条直线上的向量也互相平行，我们只需要求求一下这两个直线上向量的叉积，为零就是平行或者重合。
对于重合的判定，我们只需要选择这一根直线上的两点分别与另一条直线上的一点做向量，如果新得到的两个向量仍然是叉积为0则这两条直线重合。
剩下的情况就是相交的说。
对于相交点的求法有很多，下面给出一种方法。

对于任意一条直线上我们都可以找到两点

(x0,y0),(x1,y1)。

对于每条直线我们都可以得到

a = y0 – y1
b = x1 – x0
c = x0y1 – x1y0

那么两条直线交点的坐标值就是

D = a0b1 – a1b0， (D为0时，表示两直线平行)
x = (b0c1 – b1c0)/D
y = (a1c0 – a0c1)/D

{% note success %}
据说输出格式为 %.2lf 会WA
据说在g++的环境下 也会WA
**但是我的代码没有遇上上面任何问题。**
{% endnote %}

--------------------------------

## AC代码
```c++
#include <cstdio>
using namespace std;
#define eps 1e-6

int sgn(double a) { return a < -eps ? -1 : a < eps ? 0 : 1; }

class Point {
public:
  double x, y;
  Point(double x = 0, double y = 0) : x(x), y(y) {}
  Point operator+(Point a) { return Point(x + a.x, y + a.y); }
  Point operator-(Point b) { return Point(x - b.x, y - b.y); }
};

typedef Point Vector;

struct Line {
  Point a, b;
};

double cross(Vector a, Vector b) { return a.x * b.y - a.y * b.x; }

void Line_Line(Line l1, Line l2) {
  Vector a = l1.a - l1.b;
  Vector b = l2.a - l2.b;
  double ttans = cross(a,b);
  if (sgn(ttans) == 0) {
    Vector t1 = l1.a - l2.a;
    Vector t2 = l1.b - l2.a;
    if (cross(t1, t2) == 0) {
      puts("LINE");
    } else {
      puts("NONE");
    }
  } else {
    double a0 = l1.a.y - l1.b.y;
    double b0 = l1.b.x - l1.a.x;
    double c0 = l1.a.x * l1.b.y - l1.b.x * l1.a.y;
    double a1 = l2.a.y - l2.b.y;
    double b1 = l2.b.x - l2.a.x;
    double c1 = l2.a.x * l2.b.y - l2.b.x * l2.a.y;
    double ansd = a0 * b1 - a1 * b0;
    double ansx = (b0 * c1 - b1 * c0) / ansd;
    double ansy = (a1 * c0 - a0 * c1) / ansd;
    printf("POINT %.2f %.2f\n", ansx, ansy);
  }
}

void solve(void) {
  int t;
  scanf("%d", &t);
  puts("INTERSECTING LINES OUTPUT");
  while (t--) {
    Line a, b;
    scanf("%lf%lf%lf%lf", &a.a.x, &a.a.y, &a.b.x, &a.b.y);
    scanf("%lf%lf%lf%lf", &b.a.x, &b.a.y, &b.b.x, &b.b.y);
    Line_Line(a, b);
  }
  puts("END OF OUTPUT");
}

int main(void) {
  solve();
  return 0;
}
```