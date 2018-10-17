---
title: 【暴力/大数】HRBUST 1115 斐波那契数
copyright: true
date: 2018-09-03 23:28:02
tags: 
 - 水题
 - JAVA
 - 大数
categories:
 - 暴力
keywords:
 - HRUBST 1115
 - JAVA
 - 大数
---

*HRUBST 1115 解题报告*
<!-- more -->

>Time Limit: 1000 MS Memory Limit: 65536 K

## Description
菲波那契数大家可能都已经很熟悉了：f(1)=0 f(2)=1 f(n)=f(n-1)+f(n-2) n>2 因此，当需要其除以某个数的余数时，不妨加一些处理就可以得到。

## Input
输入数据为一些整数对P、K，P(1<P<5000)表示菲波那契数的序号，K(1<=K<15)表示2的幂次方。遇到两个空格隔开的0时表示结束处理。

## Output
输出其第P个菲波那契数除以2的K次方的余数。

## Sample Input
6 2
20 10
0 0

## Sample Output
1
85

## Author
万祥

-------------------------------

## 题意
RT

-----------------------

## 思路
暴力，JAVA BigInteger上来一顿预处理，然后一顿暴力跑就完了

JAVA使我快乐。

------------------------------

## AC代码
```java
import java.io.*;
import java.util.*;
import java.math.*;
public class Main{
	public static void main(String args[])
	{
		BigInteger f[] = new BigInteger[5003];
		f[1] = BigInteger.valueOf(0);
		f[2] = BigInteger.valueOf(1);
		for(int i = 3 ; i <= 5001 ; i++)
		{
			f[i] = f[i-1].add(f[i-2]);
		}
		BigInteger jc[] = new BigInteger[20];
		jc[1] = BigInteger.valueOf(2);
		for(int i = 2 ; i <= 17 ; i++)
		{
			jc[i] = jc[i-1].multiply(BigInteger.valueOf(2));
		}
		Scanner cin = new Scanner(System.in);
		boolean next = true;
		while(next)
		{
			int a = cin.nextInt();
			int b = cin.nextInt();
			if(a==0&&b==0)
				break;
			System.out.println(f[a].mod(jc[b]));
		}
	}
}
```