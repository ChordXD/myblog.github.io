---
title: 【树】POJ 1330 Nearest Common Ancestors
copyright: true
date: 2019-05-12 23:48:31
tags:
 - 数据结构
categories:
 - 数据结构
 - 树
keywords:
description:
---

Time Limit: 1000MS		Memory Limit: 10000K
## Description
A rooted tree is a well-known data structure in computer science and engineering. An example is shown below: 
![格子刷油漆](<http://poj.org/images/1330_1.jpg>)
In the figure, each node is labeled with an integer from {1, 2,...,16}. Node 8 is the root of the tree. Node x is an ancestor of node y if node x is in the path between the root and node y. For example, node 4 is an ancestor of node 16. Node 10 is also an ancestor of node 16. As a matter of fact, nodes 8, 4, 10, and 16 are the ancestors of node 16. Remember that a node is an ancestor of itself. Nodes 8, 4, 6, and 7 are the ancestors of node 7. A node x is called a common ancestor of two different nodes y and z if node x is an ancestor of node y and an ancestor of node z. Thus, nodes 8 and 4 are the common ancestors of nodes 16 and 7. A node x is called the nearest common ancestor of nodes y and z if x is a common ancestor of y and z and nearest to y and z among their common ancestors. Hence, the nearest common ancestor of nodes 16 and 7 is node 4. Node 4 is nearer to nodes 16 and 7 than node 8 is. 

For other examples, the nearest common ancestor of nodes 2 and 3 is node 10, the nearest common ancestor of nodes 6 and 13 is node 8, and the nearest common ancestor of nodes 4 and 12 is node 4. In the last example, if y is an ancestor of z, then the nearest common ancestor of y and z is y. 

Write a program that finds the nearest common ancestor of two distinct nodes in a tree. 
<!-- more -->

## Input
The input consists of T test cases. The number of test cases (T) is given in the first line of the input file. Each test case starts with a line containing an integer N , the number of nodes in a tree, 2<=N<=10,000. The nodes are labeled with integers 1, 2,..., N. Each of the next N -1 lines contains a pair of integers that represent an edge --the first integer is the parent node of the second integer. Note that a tree with N nodes has exactly N - 1 edges. The last line of each test case contains two distinct integers whose nearest common ancestor is to be computed.

## Output
Print exactly one line for each test case. The line should contain the integer that is the nearest common ancestor.

## Sample Input
2
16
1 14
8 5
10 16
5 9
4 6
8 4
4 10
1 13
6 15
10 11
6 7
10 2
16 3
8 1
16 12
16 7
5
2 3
3 4
3 1
1 5
3 5

## Sample Output
4
3

## Source
Taejon 2002

## 题意
给出一棵树，问你给出的两个节点最近的共同祖先节点是谁。

## 思路
很简单。
1. 首先找到跟节点。具体方法与并查集的思想差不多，每个节点存储自己老爹的编号，然后唯一那个没有老爹的（就是存储的编号是自己的那个节点就是根节点）。
2. 之后处理出所有节点的深度。任意一个节点的深度等于父节点深度+1，其所有儿子节点的深度等于其自身节点深度+1，那么我们可以从父节点出发，写一个dfs，处理出所有节点的深度。
3. 然后也非常简单，对于任意两个节点，如果其不同，则找其的老爹节点，并更新这两个节点的值。至于找谁老爹的节点呢？毫无疑问是找深度更深的节点的老爹节点，并更新。直到这俩节点相同就说明找到了他们共同最近的老爹节点。因为必然老爹节点的深度相同嘛。

## AC代码
```c++
#include<iostream>
#include<vector>
#include<cstring>
using namespace std;
const int maxn = 1e5 + 7;
int DEEP[maxn];
vector<int>p[maxn]; //存每个节点的儿子节点。
int f[maxn];
void dfs(int n,int deep) // 处理出所有节点的深度。
{
    DEEP[n] = deep;
    for(int i = 0 ; i < p[n].size() ; i++){
        dfs(p[n][i],deep+1);
    }
}
const int INF = 0x3f3f3f3f;
int main(void)
{
    int t;
    cin>>t;
    while(t--){
        int n;
        cin>>n;
        /*千万别忘了初始化*/
        for(int i = 0 ; i <= n ;  i++){
            p[i].clear();
            f[i] = i;
        }
        memset(DEEP,INF,sizeof DEEP);
        for(int i = 0 ; i < n - 1 ;  i++){
            int x,y;
            cin>>x>>y;
            f[y] = x;
            p[x].push_back(y);
        }
        //找到根节点
        int root = 1;
        while(f[root]!=root){
            root = f[root];
        }
        dfs(root,0);
        int x,y;
        cin>>x>>y;
        /*不同则找层数更深的老爹，并更新*/
        while(x!=y){
            if(DEEP[x] > DEEP[y]) x = f[x];
            else y = f[y];
        }
        cout<<x<<endl; 
    }
    return 0;
}
```