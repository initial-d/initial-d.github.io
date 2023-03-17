---
layout: post
title: USACO/Zero Sum
date: 2014-03-05 20:57:50
subtitle: USACO/Zero Sum
author: dym
header-img:
catalog:   true
tags:
     -算法
---

本文迁移自老博客，原始链接为 <https://seven.blog.ustc.edu.cn/usacozero-sum/>

给定N，在顺序排列的1～N之间随意插入“+”，“-”，“ ”，使得所构成的表达式计算之和为0，输出所有的解。N<=9
解法：先生成运算符的全排列，然后排序，再将表达式转为运算式计算，若结果为0则输出
<pre class = "brush:[cpp]">
#include&lt;iostream&gt;
#include&lt;algorithm&gt;
#include&lt;string&gt;
#include&lt;vector&gt;
#include&lt;fstream&gt;
using namespace std;
vector&lt;string&gt; cnt;
string str;
int n;
string mm = "+- ";
void dfs(int num)
{
	if(num <= n - 1)
	{
		num++;
		string temp = str;
		for(int i = 0 ; i < 3; i++)
		{
			str += mm[i]; 
			dfs(num);
			str = temp;
		}
	}
	else
		cnt.push_back(str);
}

int main()
{
	ofstream fout("zerosum.out");
	ifstream fin("zerosum.in");
	fin&gt;&gt;n;
	dfs(1);
	sort(cnt.begin() , cnt.end());
	for(int i = 0 ; i < cnt.size() ;i++ )
	{
		int foo[11];
		string ct;
		int t1 = 0;
		foo[t1++] = 1;
		for(int j = 1 ; j < n ; j++)
		{
			if(cnt[i][j - 1] == '+')
			{
				foo[t1++] = j + 1;
				ct += "+";
			}
			else if(cnt[i][j - 1] == '-')
			{
				foo[t1++] = j + 1;
				ct += "-";
			}
			else
			{
				foo[t1 - 1] = foo[t1 - 1] * 10 + j + 1;
			}
		}
		int sum = foo[0];
		for(int j = 0 ; j < ct.size() ;j++)
		{
			if(ct[j] == '+')
				sum = sum + foo[j + 1];
			else if(ct[j] == '-')
				sum = sum - foo[j + 1];
		}
		if(sum == 0)
		{
			fout&lt;&lt;"1";
			for(int k = 0 ; k < cnt[i].size() ;k++)
			{
				fout&lt;&lt;cnt[i][k]&lt;&lt;k + 2;
			}
			fout&lt;&lt;endl;
		}
	}
}
</pre>




