---
layout: post
title: USACO/Bessie Come Home
date: 2014-03-12 16:25:16
subtitle: USACO/Bessie Come Home
author: dym
header-img:
catalog:   true
tags:
     -算法
---

本文迁移自老博客，原始链接为 <https://seven.blog.ustc.edu.cn/usacobessie-come-home/>

给定一些点，标号为A-Z、a-z，给定点之间的距离，求从Z到所有其他的大写字母代表的点的最短路径中的最小值。
解法：Dijkstra，注意处理重复边
<pre class = "brush:[cpp]">
#include&lt;iostream&gt;
#include&lt;algorithm&gt;
#include&lt;fstream&gt;
#include&lt;stdio.h&gt;
#include&lt;string.h&gt;
using namespace std;
ofstream fout ("comehome.out");
ifstream fin ("comehome.in");
const int INF = 1&lt;&lt;25;
int cnt[55][55];
int dis[55];
bool vis[55];
int n;
void input()
{
	for(int i = 1 ; i <= 52 ;i++){
		for(int j = 1 ; j <= 52 ;j++)
			cnt[i][j] = INF;
	}
	fin&gt;&gt;n;
	char a,b;
	int c , a1, b1;
	for(int i = 0 ;i < n ;i++)
	{
		fin&gt;&gt;a&gt;&gt;b&gt;&gt;c;
		if(isupper(a))
			a1 = a - 'A' + 1;
		else if(islower(a))
			a1 = a - 'a' + 1 + 26;
		if(isupper(b))
			b1 = b - 'A' + 1;
		else if(islower(b))
			b1 = b - 'a' + 1 + 26;
		if(cnt[a1][b1] > c)
			cnt[a1][b1] = cnt[b1][a1]  = c;
	}
	memset(vis , false ,sizeof(vis));
	for(int i = 1 ; i <= 52 ;i++)
		dis[i] = INF;
	dis[26] = 0;
}
void dijstra()
{
	int t = 0;
	while(t++ < 52)
	{
		int Max = INF;
		int pos;
		for(int i = 1 ; i <= 52 ;i++)
		{
			if(!vis[i] && dis[i] <= Max)
			{
				Max = dis[i];
				pos = i;
			}
		}
		vis[pos] = true;
		for(int j = 1 ; j <= 52 ;j++)
		{
			if(dis[pos] + cnt[pos][j] < dis[j])
			{
				dis[j] = dis[pos] + cnt[pos][j];
			}
		}
	}
}
void output()
{
	int Min = INF;
	int pos;
	for(int i = 1 ; i <= 26 ;i++)
	{
		if(i != 26 && dis[i] < Min)
		{
			Min = dis[i];
			pos = i;
		}
	}
	fout&lt;&lt;(char)(pos + 'A' - 1)&lt;&lt;" "&lt;&lt;Min&lt;&lt;endl;
}

int main()
{
	input();
	dijstra();
	output();
}
</pre>