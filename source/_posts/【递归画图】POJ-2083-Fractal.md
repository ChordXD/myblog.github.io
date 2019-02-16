---
title: 【递归画图】POJ 2083 Fractal
copyright: true
date: 2019-02-16 13:11:58
tags:
 - 递归
 - 画图
categories:
 - 思维
keywords:
description:
---

Time Limit: 1000MS		Memory Limit: 30000K

## Description
A fractal is an object or quantity that displays self-similarity, in a somewhat technical sense, on all scales. The object need not exhibit exactly the same structure at all scales, but the same "type" of structures must appear on all scales. 
A box fractal is defined as below : 
A box fractal of degree 1 is simply 
X 
A box fractal of degree 2 is 
X X 
X 
X X 
If using B(n - 1) to represent the box fractal of degree n - 1, then a box fractal of degree n is defined recursively as following 
B(n - 1)        B(n - 1)

        B(n - 1)

B(n - 1)        B(n - 1)

Your task is to draw a box fractal of degree n.
<!-- more -->
## Input
The input consists of several test cases. Each line of the input contains a positive integer n which is no greater than 7. The last line of input is a negative integer −1 indicating the end of input.

## Output
For each test case, output the box fractal using the 'X' notation. Please notice that 'X' is an uppercase letter. Print a line with only a single dash after each test case.

## Sample Input
1
2
3
4
-1

## Sample Output
X
-
X X
 X
X X
-
X X   X X
 X     X
X X   X X
   X X
    X
   X X
X X   X X
 X     X
X X   X X
-
X X   X X         X X   X X
 X     X           X     X
X X   X X         X X   X X
   X X               X X
    X                 X
   X X               X X
X X   X X         X X   X X
 X     X           X     X
X X   X X         X X   X X
         X X   X X
          X     X
         X X   X X
            X X
             X
            X X
         X X   X X
          X     X
         X X   X X
X X   X X         X X   X X
 X     X           X     X
X X   X X         X X   X X
   X X               X X
    X                 X
   X X               X X
X X   X X         X X   X X
 X     X           X     X
X X   X X         X X   X X
-

## Source
Shanghai 2004 Preliminary

## 题意
根据样例画图。

## 思路
最小子图就是X，若当前是最小子图就在当前位置画一个X，
若不是，当前图下五个最小子图的位置，继续递归。

## 坑点
预处理 + printf TLE
预处理 + putchar 15ms
直接putchar 0ms

## AC代码
Problem: 2083		User: pengrj
Memory: 4668K		Time: 0MS
Language: G++		Result: Accepted
```c++
#include <cmath>
#include <cstdio>
using namespace std;
int a[8][1000][1000];

void print(int size, int x, int y, int i)
{
    int m = 1;
    for (int j = 0; j < i - 2; j++)
        m *= 3;

    if (i == 1)
        a[size][x][y] = 1;
    else
    {
        print(size, x, y, i - 1);
        print(size, x, y + m * 2, i - 1);
        print(size, x + m, y + m, i - 1);
        print(size, x + 2 * m, y, i - 1);
        print(size, x + 2 * m, y + 2 * m, i - 1);
    }
}

void solve(void)
{
    int n;
    while (scanf("%d", &n), n != -1)
    {
        print(n, 0, 0, n);
        int size = pow(3, n - 1);
        for (int i = 0; i < size; i++)
            for (int j = 0; j < size; j++)
            {
                a[n][i][j] == 1 ? putchar('X') : putchar(' ');
                if (j == size - 1)
                    puts("");
            }
        printf("-\n");
    }
}

int main(void)
{
    solve();
    return 0;
}
```
