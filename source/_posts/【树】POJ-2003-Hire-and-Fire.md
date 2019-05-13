---
title: 【树】POJ 2003 Hire and Fire
copyright: true
date: 2019-05-13 22:13:35
tags:
 - 数据结构
categories:
 - 数据结构
 - 树
keywords:
description:
---

Time Limit: 1000 MS Memory Limit: 30000 KB
## Description
In this problem, you are asked to keep track of the hierarchical structure of an organization's changing staff. As the first event in the life of an organization, the Chief Executive Officer (CEO) is named. Subsequently, any number of hires and fires can occur. Any member of the organization (including the CEO) can hire any number of direct subordinates, and any member of the organization (including the CEO) can be fired. The organization's hierarchical structure can be represented by a tree. Consider the example shown by Figure 1: 
![photo1](http://acm.hrbust.edu.cn/vj/oj_static/poj/2003_desc_001.jpg)
VonNeumann is the CEO of this organization. VonNeumann has two direct subordinates: Tanenbaum and Dijkstra. Members of the organization who are direct subordinates of the same member are ranked by their respective seniority. In the diagram, the seniority of such members decrease from left to right. For example Tanenbaum has higher seniority than Dijkstra. 

When a member hires a new direct subordinate, the newly hired subordinate has lower seniority than any other direct subordinates of the same member. For example, if VonNeumann (in Figure 1) hires Shannon, then VonNeumann's direct subordinates are Tanenbaum, Dijkstra, and Shannon in order of decreasing seniority. 

When a member of the organization gets fired, there are two possible scenarios. If the victim (the person who gets fired) had no subordinates, then he/she will be simply dropped from the organization's hierarchy. If the victim had any subordinates, then his/her highest ranking (by seniority) direct subordinate will be promoted to fill the resulting vacancy. The promoted person will also inherit the victim's seniority. Now, if the promoted person also had some subordinates then his/her highest ranking direct subordinate will similarly be promoted, and the promotions will cascade down the hierarchy until a person having no subordinates has been promoted. In Figure 1, if Tanenbaum gets fired, then Stallings will be promoted to Tanenbaum's position and seniority, and Knuth will be promoted to Stallings' previous position and seniority. 

Figure 2 shows the hierarchy resulting from Figure 1 after (1) VonNeumann hires Shannon and (2) Tanenbaum gets fired: 
![photo2](http://acm.hrbust.edu.cn/vj/oj_static/poj/2003_desc_002.jpg)

<!-- more -->
## Input
The first line of the input contains only the name of the person who is initially the CEO. All names in the input file consist of 2 to 20 characters, which may be upper or lower case letters, apostrophes, and hyphens. (In particular, no blank spaces.) Each name contains at least one upper case and at least one lower case letter. 

The first line will be followed by one or more additional lines. The format of each of these lines will be determined by one of the following three rules of syntax: 

[existing member] hires [new member] 
fire [existing member] 
print

Here [existing member] is the name of any individual who is already a member of the organization, [new member] is the name of an individual who is not a member of the organization as yet. The three types of lines (hires, fire, and print) can appear in any order, any number of times. 

You may assume that at any time there is at least one member (who is the CEO) and no more than 1000 members in the organization. 


## Output
For each print command, print the current hierarchy of the organization, assuming all hires and fires since the beginning of the input have been processed as explained above. Tree diagrams (such as those in Figures 1 and 2) are translated into textual format according to the following rules: 
Each line in the textual representation of the tree will contain exactly one name. 
The first line will contain the CEO's name, starting in column 1. 
The entire tree, or any sub-tree, having the form 
![](http://acm.hrbust.edu.cn/vj/oj_static/poj/2003_output_001.jpg)
will be represented in textual form as: 
![](http://acm.hrbust.edu.cn/vj/oj_static/poj/2003_output_002.jpg)
The output resulting from each print command in the input will be terminated by one line consisting of exactly 60 hyphens. There will not be any blank lines in the output.

## Sample Input
```
VonNeumann
VonNeumann hires Tanenbaum
VonNeumann hires Dijkstra
Tanenbaum hires Stallings
Tanenbaum hires Silberschatz
Stallings hires Knuth
Stallings hires Hamming
Stallings hires Huffman
print
VonNeumann hires Shannon
fire Tanenbaum
print
fire Silberschatz
fire VonNeumann
print
```

## Sample Output
```
VonNeumann
+Tanenbaum
++Stallings
+++Knuth
+++Hamming
+++Huffman
++Silberschatz
+Dijkstra
------------------------------------------------------------
VonNeumann
+Stallings
++Knuth
+++Hamming
+++Huffman
++Silberschatz
+Dijkstra
+Shannon
------------------------------------------------------------
Stallings
+Knuth
++Hamming
+++Huffman
+Dijkstra
+Shannon
------------------------------------------------------------
```
## Source
Rocky Mountain 2004

## 题意
现在有一个树，父节点是孩子节点的上司，孩子节点从左到右级别依次递减。
现在有三个操作，一个操作是打印这棵树，第二个操作是删除一个节点，之后该节点的孩子节点的级别最大的节点代替删除掉的节点，并且代替之后该孩子节点删除，对该孩子节点的删除操作同上，直到某一个节点没有孩子节点。
第三个操作是对于任意的一个节点增加一个孩子节点，并将其放置在树的最右端（优先级最校）。

## 思路
模拟嘛，一顿模拟就完了。
List操作不熟欸。
删除操作实际上就是把删除节点的所有左第一个儿子（包括儿子的儿子们）向上提升一个层次。

## AC代码
```c++
#include<iostream>
#include<list>
#include<map>
using namespace std;

const int maxn = 1e5 + 7;
struct Poi
{
    Poi *f;
    list<Poi *>son;
    string name;
    Poi()
    {
        f = NULL;
    }
};

map<string,Poi*>p;

void fire(string n)
{
    Poi *now = p[n];
    while(now->son.size()!=0)
    {
        now->name = now->son.front()->name;
        p[now->name] = now;
        now = now->son.front();
    }
    now->f->son.remove(now);
    delete now;
}

void print(Poi *now,int deep)
{
    for(int i = 0 ; i < deep ; i++)
        cout<<"+";
    cout<<now->name<<endl;
    list<Poi*>::iterator it = now->son.begin();
    while(it!=now->son.end())
    {
        print(*it,deep+1);
        it++;
    }
}

void add(string a,string b)
{
    Poi *f = p[a];
    Poi *s = new Poi();
    s->name = b;
    p[b] = s;
    f->son.push_back(s);
    s->f = f;
}

int main(void)
{
    string boss;
    cin>>boss;
    Poi *Boss = new Poi();
    Boss->name = boss;
    string a,b,c;
    p[boss] = Boss;
    while(cin>>a){
        if(a == "print"){
            print(Boss,0);
            for(int i = 0 ; i< 60 ; i++)
                cout<<"-";
            cout<<endl;
        }
        else if(a == "fire"){
            cin>>b;
            fire(b);
        }
        else{
            cin>>b>>c;
            add(a,c);
        }
    }
    return 0;
}

```