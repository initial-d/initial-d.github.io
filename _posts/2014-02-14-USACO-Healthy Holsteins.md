---
layout: post
title: USACO/Healthy Holsteins
date: 2014-02-14 21:27:44
subtitle: USACO/Healthy Holsteins
author: dym
header-img:
catalog:   true
tags:
     -算法
---

本文迁移自老博客，原始链接为 <https://seven.blog.ustc.edu.cn/usacohealthy-holsteins/>

/*
给定v个数，然后给出g组数，每一组数都有v个数组成，从g组数中选出一个集合，使得将这个集合中各个组的对应位置的数相加以后得到一组数，这组数的每一个都不能小于初始给出的那组数的对应位置的数，要求输出最小的满足要求的集合。
解法：使用位运算进行枚举，g组数对应0~2^g-1共2^g种二进制数的组合方式。枚举每种二进制组合方式，求出符合要求的解。
*/
<pre class = "brush :[cpp]">
#include&lt;iostream&gt;
#include&lt;algorithm&gt;
#include&lt;fstream&gt;
#include&lt;vector&gt;
using namespace std;
#define M 1&lt;&lt;15

int v;
int cnt[25], ans[25];
int g , Min ,MinX;
int avl[15][1000];
vector&lt;bool&gt; bit;
void calbit(int val)
{
	while(val)
	{
		bit.insert(bit.end(), val%2);
		val /= 2;
	}
}
bool sta()
{
	for(int i = 0 ; i < 25 ; i++)
		if(ans[i] < cnt[i])
			return false;
	return true;
}
int main()
{
	ofstream fout ("holstein.out");
    ifstream fin ("holstein.in");
	fin&gt;&gt;v;
	for(int i = 0 ; i < v; i++)
		fin&gt;&gt;cnt[i];
	fin&gt;&gt;g;
	for(int i = 0 ;i < g; i++)
	{
		for(int j = 0 ;j < v; j++)
		{
			fin&gt;&gt;avl[i][j];
		}
	}
	Min = 10000000;
	int sum = 0;
	for(int i = 0 ; i < 1&lt;&lt;g ;i++)
	{
		bit.clear();
		for(int j = 0 ; j < 25; j++)
			ans[j] = 0;
		calbit(i);
		for(int j = 0 ; j < bit.size(); j++)
		{
			if(bit[j])
			{
				for(int k = 0 ; k < v;k++)
					ans[k] += avl[j][k];
			}
		}
		if(sta())
		{
			bit.clear();
			calbit(i);
			int num = 0;
			for(int k = 0 ; k < bit.size(); k++)
			{
				if(bit[k])
					num++;
			}
			if(num < Min)
			{
				Min = num;
				MinX = i;
			}
		}
	}
	vector&lt;int&gt; type;
	bit.clear();
	calbit(MinX);
	for(int k = 0 ; k < bit.size(); k++)
		if(bit[k])
			type.push_back(k + 1);
	fout&lt;&lt;Min&lt;&lt;" ";
	for(int i = 0 ; i < type.size() - 1; i++)
		fout&lt;&lt;type[i]&lt;&lt;" ";
	fout&lt;&lt;type[type.size() - 1]&lt;&lt;endl;
 	return 0;
}
</pre>
