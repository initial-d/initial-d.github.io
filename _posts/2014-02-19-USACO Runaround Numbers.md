---
layout: post
title: USACO/Runaround Numbers
date: 2014-02-19 15:42:20
subtitle: USACO/Runaround Numbers
author: dym
header-img:
catalog:   true
tags:
     -算法
---

本文迁移自老博客，原始链接为 <https://seven.blog.ustc.edu.cn/usacorunaround-numbers/>

寻找比给定数字大的第一个循环数。定义循环数：从最左边的数字开始向右数最左边这个数(如果数到了最右边就回到最左边),你会停止在另一个新的数字(如果停在一个相同的数字上，这个数就不是循环数)，重复这样做，如果你经过每一个数字一次以后没有回到起点,你的数字不是一个循环数，在经过每个数字一次后回到起点的就是循环数。
解法：从给定数字开始循环向下枚举寻找，按规则判断是否是循环数，有重复数字的直接排除掉
<pre class = "brush:[cpp]">
#include&lt;iostream&gt;
#include&lt;algorithm&gt;
#include&lt;fstream&gt;
#include&lt;map&gt;
#include&lt;vector&gt;
using namespace std;

long long number;
vector&lt;int&gt; vec;
map&lt;int ,int&gt; Count;
map&lt;int ,int&gt; filter;
long long find()
{
	while(true)
	{
		long long temp = number + 1;
		vec.clear();
		Count.clear();
		filter.clear();
		while(temp)
		{
			vec.insert(vec.begin() , temp % 10);
			filter[temp % 10]++;
			temp /= 10;
		}
		temp = number + 1;
		bool fflag = true;
		for(map&lt;int ,int&gt;::iterator it = filter.begin() ; it != filter.end() ; it++)
			if(it->second > 1)
			{
				number++;
				fflag = false;
				break;
			}
		if(!fflag)
			continue;
		int t = 0;
		while(true)
		{
			t = (vec[t] + t) % vec.size();
			Count[vec[t]]++;
			bool flag = false; 
			bool flag1 = false;
			for(map&lt;int ,int&gt;::iterator it = Count.begin() ; it != Count.end() ; it++)
				{
					if(it->second >= 2)
					{
						flag1 = true;
						break;
					}
				}
			if(flag1)
			{
				number++;
				break;
			}
			if(t == 0)
			{
				for(int i = 0 ; i < vec.size() ;i++)
				{
					if(Count[vec[i]] != 1)
					{			 
						flag = true;
						break;
					}
				}
				if(!flag)
					return temp;
				else
				{
					number++;
					break;
				}
			}
		}
	}
}	
int main()
{
	ofstream fout ("runround.out");
	ifstream fin ("runround.in");
	fin&gt;&gt;number;
	fout&lt;&lt;find()&lt;&lt;endl;
 	return 0;
}
</pre>
