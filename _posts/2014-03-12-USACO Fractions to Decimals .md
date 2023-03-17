---
layout: post
title: USACO/Fractions to Decimals 
date: 2014-03-12 18:46:48
subtitle: USACO/Fractions to Decimals 
author: dym
header-img:
catalog:   true
tags:
     -算法
---

本文迁移自老博客，原始链接为 <https://seven.blog.ustc.edu.cn/usacofractions-to-decimals/>

将分数转换为小数，如果是有限小数，则直接输出结果，若果是循环小数，则找到循环节并用括号括起来表示后面的循环部分。规则如下：
1/3 = 0.(3)
22/5 = 4.4
1/7 = 0.(142857)
2/2 = 1.0
3/8 = 0.375
45/56 = 0.803(571428)
解法：模拟除法，找到循环节的关键是出现重复的余数。
<pre class = "brush:[cpp]">
#include&lt;iostream&gt;
#include&lt;algorithm&gt;
#include&lt;fstream&gt;
#include&lt;map&gt;
#include&lt;stdio.h&gt;
#include&lt;string.h&gt;
using namespace std;
ofstream fout ("fracdec.out");
ifstream fin ("fracdec.in");

int n,d ,z ,r;
map&lt;int , int&gt; bt;
int ans[1000000];
int s = 1;
bool flag = false;
int pos;
void div()
{
	bt[r] = s;
	while(r / d == 0)
	{
		r *= 10;
		if(bt[r])
		{
			flag = true;
			ans[s++] = 0;
			pos = bt[r];
			return;
		}
		ans[s++] = 0;
	}
	ans[s-1] = r / d;
	r = r % d;
	if(r == 0)
		return;
	if(bt[r])
	{
		flag = true;
		pos = bt[r];
		return;
	}
	else
		div();
}
void output()
{
	fout&lt;&lt;z&lt;&lt;".";
	int count = 0;
	if(z == 0)
		count = 1;
	while(z)
	{
		count++;
		z /= 10;
	}
	count++;
	if(flag)
	{
		for(int i = 1 ; i < s; i++)
		{
			if(i == pos)
			{
				fout&lt;&lt;"(";
				count++;
				if(count > 75)
				{
					fout&lt;&lt;endl;
					count = 0;
				}
			}
			fout&lt;&lt;ans[i];
			count++;
			if(count > 75)
			{
				fout&lt;&lt;endl;
				count = 0;
			}
		}
		fout&lt;&lt;")"&lt;&lt;endl;
	}
	else{	
		for(int i = 1 ; i < s; i++)
			fout&lt;&lt;ans[i];
		fout&lt;&lt;endl;}
}
int main()
{
	fin&gt;&gt;n&gt;&gt;d;
	z = n / d;
	r = n % d;
	if(r != 0)
	{
		div();
		output();
	}
	else
		fout&lt;&lt;z&lt;&lt;"."&lt;&lt;0&lt;&lt;endl;
}
</pre>