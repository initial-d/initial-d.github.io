---
layout: post
title: USACO/Agri-Net
date: 2014-03-13 20:23:58
subtitle: USACO/Agri-Net
author: dym
header-img:
catalog:   true
tags:
     -算法
---

本文迁移自老博客，原始链接为 <https://seven.blog.ustc.edu.cn/usacoagri-net/>

裸的最小生成树，prim。
<pre class = "brush:[cpp]">
#include&lt;iostream&gt;
#include&lt;algorithm&gt;
#include&lt;fstream&gt;
#include&lt;stdio.h&gt;
using namespace std;
ofstream fout ("agrinet.out");
ifstream fin ("agrinet.in");

const int INF = 1&lt;&lt;30;
int n;
int ans;
int Min;
int g[101][101];
bool vis[101];
int dis[101];
void prim(int v)
{
	memset(vis , 0 ,sizeof(vis));
	for(int i = 1 ; i <= n ;i++)
	{
		dis[i] = g[v][i];
	}
	vis[1] = true;
	for(int i = 1 ;  i <= n;i++)
	{		
		Min = INF;
		int k;
		for(int j = 1 ; j <= n;j++)
		{
			if(!vis[j] && dis[j] < Min)
			{
				Min = dis[j];
				k = j;
			}
		}
		if(Min < INF)
		{
			vis[k] = true;
			ans += Min;
		}
		for(int j = 1 ; j <= n;j++)
		{
			if(!vis[j] && dis[j] > g[k][j])
			{
				dis[j] = g[k][j];
			}
		}
	}
}
int main()
{
	fin&gt;&gt;n;
	for(int i = 1; i <= n ;i++)
	{
		for(int j = 1 ;j <= n;j++)
		{
			fin&gt;&gt;g[i][j];			
		}
	}
	ans = 0;
	prim(1);
	fout&lt;&lt;ans&lt;&lt;endl;
	return 0;
}
</pre>