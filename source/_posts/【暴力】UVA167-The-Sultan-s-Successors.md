---
title: 【暴力】UVA167 The Sultan's Successors
copyright: true
date: 2019-02-15 12:01:48
tags:
 - 暴力
 - 递归
categories:
 - 暴力
keywords:
description:
---

## The Sultan's Successors 
The Sultan of Nubia has no children, so she has decided that the country will be split into up to k separate parts on her death and each part will be inherited by whoever performs best at some test. It is possible for any individual to inherit more than one or indeed all of the portions. To ensure that only highly intelligent people eventually become her successors, the Sultan has devised an ingenious test. In a large hall filled with the splash of fountains and the delicate scent of incense have been placed k chessboards. Each chessboard has numbers in the range 1 to 99 written on each square and is supplied with 8 jewelled chess queens. The task facing each potential successor is to place the 8 queens on the chess board in such a way that no queen threatens another one, and so that the numbers on the squares thus selected sum to a number at least as high as one already chosen by the Sultan. (For those unfamiliar with the rules of chess, this implies that each row and column of the board contains exactly one queen, and each diagonal contains no more than one.)

Write a program that will read in the number and details of the chessboards and determine the highest scores possible for each board under these conditions. (You know that the Sultan is both a good chess player and a good mathematician and you suspect that her score is the best attainable.)
<!-- more -->
## Input
Input will consist of k (the number of boards), on a line by itself, followed by k sets of 64 numbers, each set consisting of eight lines of eight numbers. Each number will be a positive integer less than 100. There will never be more than 20 boards.

## Output
Output will consist of k numbers consisting of your k scores, each score on a line by itself and right justified in a field 5 characters wide.

## Sample input
```
1
 1  2  3  4  5  6  7  8
 9 10 11 12 13 14 15 16
17 18 19 20 21 22 23 24
25 26 27 28 29 30 31 32
33 34 35 36 37 38 39 40
41 42 43 44 45 46 47 48
48 50 51 52 53 54 55 56
57 58 59 60 61 62 63 64
```
## Sample output
``
260
``

## 题意
给你一个８×８的棋盘，现在给你８个皇后，让这８个皇后站在这个棋盘的不同位置，要求每个皇后的对角线方向，行列方向不能有其他的皇后，现在问怎样安排使得这８个皇后所站位置的权值最大，输出最大的值。

## 思路
暴力
离线处理出所有的站位可能情况，然后针对不同的棋盘遍历所有的情况找出最大情况的权值即为答案。

## 坑点
代码千万条，输出第一条。
格式不规范，ＰＥ两行泪。

## AC代码
```c++
#include <bits/stdc++.h>
using namespace std;
const int maxn = 1e5 + 7;
int lieaim[1000][9];
int lie[9], r[17], l[17];
int temp[55];
int n;
int Map[100][100];
void init(int hh)
{
    if (hh == 8)
    {
        for (int i = 0; i < 8; i++)
            lieaim[n][i] = temp[i];
        n++;
    }
    else
    {
        for (int i = 0; i < 8; i++)
        {
            int tlie = i, thang = hh;
            int tr = tlie - thang + 7, tl = tlie + thang;
            if (lie[tlie] == 0 && r[tr] == 0 && l[tl] == 0)
            {
                temp[hh] = tlie;
                lie[tlie] = 1, r[tr] = 1, l[tl] = 1;
                init(hh + 1);
                lie[tlie] = 0, r[tr] = 0, l[tl] = 0;
            }
        }
    }
}

int main(void)
{
    int t;
    scanf("%d", &t);
    init(0);
    while (t--)
    {
        for (int i = 0; i < 8; i++)
            for (int j = 0; j < 8; j++)
                scanf("%d", &Map[i][j]);
        int ans = -1;
        for (int i = 0; i < n; i++)
        {
            int sum = 0;
            for (int j = 0; j < 8; j++)
                sum += Map[j][lieaim[i][j]];
            ans = sum > ans ? sum : ans;
        }
        printf("%5d\n", ans);
    }
}
```

