---
layout: post
title: USACO/Hamming Codes
date: 2014-02-15 11:11:00
subtitle: USACO/Hamming Codes
author: dym
header-img:
catalog:   true
tags:
     -算法
---

本文迁移自老博客，原始链接为 <https://seven.blog.ustc.edu.cn/usacohamming-codes/>

找出n个在0~2^b之间的数，使得这n个数的二进制形式两两之间至少有d个二进制位不同，输出满足要求的最小的序列。
解法：枚举，从0开始，记录满足要求的数字，每到一个数字，将他和所记录的已经满足要求的数字比较，符合要求则加入记录，否则跳过，直到找到n个，这样还可以保证序列是最小的。 
<pre class = "brush:[cpp]">
#include&lt;iostream&gt;
#include&lt;algorithm&gt;
#include&lt;fstream&gt;
#include&lt;vector&gt;
#include&lt;map&gt;
using namespace std;

int n,b,d;
map&lt;int , vector&lt;bool&gt; &gt; bit;
void calbit(int val)
{
	int tmp = val;
	while(val)
	{
		bit[tmp].push_back(val%2);
		val /= 2;
	}
	while(bit[tmp].size() < 7)
		bit[tmp].push_back(0);
}
bool cal_dis(int i , int j)
{
	int num = 0;
	for(int k = 0 ; k < b; k++)
	{
		if(bit[i][k] != bit[j][k])
			num++;
	}
	if(num >= d)
		return true;
	else
		return false;
}
int main()
{
	ofstream fout ("hamming.out");
    ifstream fin ("hamming.in");
	vector&lt;int&gt; ans;
	fin&gt;&gt;n&gt;&gt;b&gt;&gt;d;
	calbit(0);
	ans.push_back(0);
	bool flag;
	int foo = 0;
	for(int i = 1 ; i < 2 &lt;&lt; b ;i++)
	{
		calbit(i);
		flag = true;
		for(int j = 0 ; j < ans.size() ; j++)
		{
			if(!cal_dis(i , ans[j]))
			{
				flag = false;
				break;
			}
		}
		if(flag)
		{
			foo++;
			ans.push_back(i);
			if(foo == n)
				break;
		}
	}
	for(int i = 0 ; i < ans.size()&&i < n; i++)
	{
		if(i % 10 == 0 && i != 0)
			fout&lt;&lt;endl;
		if(i % 10 == 0)
			fout&lt;&lt;ans[i];
		else
			fout&lt;&lt;" "&lt;&lt;ans[i];
	}
	fout&lt;&lt;endl;
 	return 0;
}
</pre>
