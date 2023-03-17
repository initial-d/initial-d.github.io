---
layout: post
title: USACO/Controlling Companies
date: 2014-03-09 12:48:59
subtitle: USACO/Controlling Companies
author: dym
header-img:
catalog:   true
tags:
     -算法
---

本文迁移自老博客，原始链接为 <https://seven.blog.ustc.edu.cn/usacocontrolling-companies/>

有一些公司之间存在控制关系，若A有B的50%以上的股份，称A控制B，控制关系定义如下：
1.一个公司控制自己
2.如果A控制B，B控制C，则A控制C
3.如果A控制k个公司，这k个公司所占B公司的股份和若大于50%，则A控制B
给定公司间的股份占有情况，求出所有的控制关系。
解法：弗洛伊德求传递闭包，由于存在上述第3种关系，所以需要更新关系图然后继续求传递闭包，直到图上的关系不再更新，达到一个不动点（此处使用了不动点的思想）。
<pre class ="brush:[cpp]">
#include&lt;iostream&gt;
#include&lt;cstdio&gt;
#include&lt;string.h&gt;
#include&lt;string&gt;
#include&lt;fstream&gt;
using namespace std;
ofstream fout ("concom.out");
ifstream fin ("concom.in");

bool cnt[101][101];
int con[101][101];
int n;
void floyd()
{
	for(int k = 1 ; k < 101 ; k++)
		for(int i = 1 ; i < 101 ; i++)
			for(int j = 1 ; j < 101 ;j++)
			{
				if(cnt[i][k] && cnt[k][j])
					cnt[i][j] = 1;
			}
}
bool test()
{
	bool flag = true;
	for(int i = 1 ; i < 101 ; i++)
	{
		for(int j = 1 ; j < 101 ;j++)
		{
			if(!cnt[i][j])
			{
				int tt = con[i][j];
				for(int k = 1 ; k < 101 ; k++)
				{
					if(cnt[i][k] && con[k][j] && i != k)
					{
						con[i][j] += con[k][j];
					}
				}
				if(con[i][j] > 50)
				{
					flag = false;
					cnt[i][j] = 1;
				}
				con[i][j] = tt;
			}
		}
	}
	if(flag)
		return true;
	else
		return false;
}
void display()
{
	for(int i = 1 ; i < 101 ; i++)
		for(int j = 1 ; j < 101 ;j++)
		{
			if(cnt[i][j] && i != j)
				fout&lt;&lt;i&lt;&lt;" "&lt;&lt;j&lt;&lt;endl;
		}
}
int main()
{
	for(int i = 1 ; i < 101 ; i++)
		for(int j = 1 ; j < 101 ;j++)
		{
			if(i == j)
			{
				cnt[i][j] = 1;
			}
			else
			{
				cnt[i][j] = 0;
				con[i][j] = 0;
			}
		}
	fin&gt;&gt;n;
	int a , b ,c;
	for(int i = 0 ; i < n ;i++)
	{
		fin&gt;&gt;a&gt;&gt;b&gt;&gt;c;
		con[a][b] = c;
		if(c > 50)
			cnt[a][b] = 1;
	}
	floyd();
	while(!test())
	{
		floyd();
	}
	display();
	return 0;
}
</pre>
