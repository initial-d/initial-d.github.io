---
layout: post
title: USACO/Humble Numbers
date: 2014-05-14 22:05:57
subtitle: USACO/Humble Numbers
author: dym
header-img:
catalog:   true
tags:
     -算法
---

本文迁移自老博客，原始链接为 <https://seven.blog.ustc.edu.cn/usacohumble-numbers/>

寻找第n个丑数，丑数集合中每个数从小到大排列，每个丑数都是给定素数集合中的数的乘积，第N个“丑数”就是在能由素数集合中的数相乘得来的第n小的数。
解法：模拟,维护每一个素数所需乘的最小值
<pre class ="brush:[cpp]">
#include&lt;iostream&gt;
#include&lt;algorithm&gt;
#include&lt;map&gt;
#include&lt;vector&gt;
using namespace std;
ofstream fout ("humble.out");
ifstream fin ("humble.in");

int k,n,temp;
vector&lt;int&gt; prim;
vector&lt;int&gt; cosor;
vector&lt;long long&gt; vec;
map&lt;long , long&gt; ss;
long long a[100003];
int main()
{
	fin&gt;&gt;k&gt;&gt;n;
	for(int i = 0 ;i < k ;i++)
	{
		fin&gt;&gt;temp;
		prim.push_back(temp);
		cosor.push_back(1);
		vec.push_back(0);
	}
	a[1]=1;
	for(int i = 2;i <= n+1;)
	{
		int t;
		for(int j = 0 ; j < k ; j++)
		{
			vec[j] = a[cosor[j]] * prim[j];
		}
		long long Min = 9223372036854775807;
		t = 0;
		for(int j = 0; j < k ;j++)
		{
			if(vec[j] <  Min)
			{
				Min = vec[j];
				t = j;
			}		 
		}
		if(ss[Min])
		{
			cosor[t]++;
			continue;
		}
		ss[Min] = Min;
		a[i] = Min;
		cosor[t]++;
		i++;
	}
	fout&lt;&lt;a[n+1]&lt;&lt;endl;
	return 0;
}
</pre>
