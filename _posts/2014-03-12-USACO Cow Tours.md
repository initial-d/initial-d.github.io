---
layout: post
title: USACO/Cow Tours
date: 2014-03-12 13:48:05
subtitle: USACO/Cow Tours
author: dym
header-img:
catalog:   true
tags:
     -算法
---

本文迁移自老博客，原始链接为 <https://seven.blog.ustc.edu.cn/usacocow-tours/>

给定至少两个牧场，每个牧场由若干连通的牧区组成，给定牧区之间的连通关系和各自的坐标，问找出一条连接两个不同牧场的路径，使得连上这条路径后，这个更大的新牧场有最小的直径。（直径就是牧场中最远的两个牧区的距离）。输出在所有牧场中最小的可能的直径。
解法：首先通过给定的连通关系和坐标计算已知的路径长度。然后通过floyd计算出各个牧场内部的全源最短路路径。然后枚举不同牧场中的点相连，以两牧场内部的点到各自所选的点的最短路+所加的路径之和为新的直径（但有可能不是，因为有可能这个值会比原牧场的直径还要小，这时就以原来牧场的直径为新的直径），为了提高枚举效率，使用并查集优化。最后计算出所有直径的最小值即可。
<pre class = "brush:[cpp]">
#include&lt;iostream&gt;
#include&lt;algorithm&gt;
#include&lt;fstream&gt;
#include&lt;map&gt;
#include&lt;stdio.h&gt;
#include&lt;string.h&gt;
#include&lt;vector&gt;
#include&lt;queue&gt;
#include&lt;cmath&gt;
#include&lt;iomanip&gt;
using namespace std;
ofstream fout ("cowtour.out");
ifstream fin ("cowtour.in");

double Max = 10000000.0;
double tt =0.0;
struct point
{
	double x;
	double y;
};
point cnt[151];
int foo[151][151];
double dis[151][151];

int parent[151];
int n;

void make_set()
{
    for(int x = 1; x <= n; x++)
    {
        parent[x] = x;
    }
}

int find_set(int x)
{
    if(x != parent[x])
        parent[x] = find_set(parent[x]);
    return parent[x];
}
void union_set(int x, int y)
{
    x = find_set(x);
    y = find_set(y);
    if(x == y) return;
	parent[y] = x;    
}
void input()
{
	fin&gt;&gt;n;
	char str[151][151];
	for(int i = 1 ;i <= n ;i++)
		fin&gt;&gt;cnt[i].x&gt;&gt;cnt[i].y;
	for(int i = 1 ;i <= n;i++)
		fin&gt;&gt;str[i];
	for(int i = 1 ;i <= n;i++)
		for(int j = 0 ; j < n;j++)
			foo[i][j + 1] = str[i][j] - '0';
	for(int i = 1; i <= n;i++)
		for(int j = 1 ;j <= n;j++)
			dis[i][j] = 10000000.0;
}
double cal(point a , point b)
{
	return sqrt((a.x - b.x) * (a.x - b.x) + (a.y - b.y) * (a.y - b.y));
}
void caldis()
{
	for(int i = 1 ;i <= n;i++)
		for(int j = i ; j <= n;j++)
		{
			if(foo[i][j])
			{	
				dis[i][j] = dis[j][i] = cal(cnt[i] , cnt[j]);
				union_set(i , j);
			}		   
		}
}
void floyd()
{
	for(int k = 1 ;k <= n ;k++)
		for(int i = 1 ;i <= n;i++)
			for(int j = 1 ; j <= n;j++)
				if(foo[i][k] && foo[k][j])
					foo[i][j] = 1;
	for(int k = 1 ;k <= n ;k++)
		for(int i = 1 ;i <= n;i++)
			for(int j = 1 ; j <= n;j++)
				if(foo[i][j] && foo[i][k] && foo[k][j] && dis[i][j] > dis[i][k] + dis[k][j])
					dis[i][j] = dis[i][k] + dis[k][j];
	for(int i = 1 ;i <= n;i++)
		for(int j = 1 ; j <= n;j++)
		{
			if(foo[i][j])			
			{
				if(tt < dis[i][j])
					tt = dis[i][j];
			}

		}
}
void slove()
{
	double maxi , maxj ,temp;
	for(int i = 1 ;i <= n;i++)
		for(int j = 1 ; j <= n;j++)
		{
			if(find_set(i) != find_set(j))
			{
				dis[i][j] = dis[j][i] = cal(cnt[i] , cnt[j]);
				maxi = 0;
				maxj = 0;
				for(int k = 1 ;k <= n; k++)
				{
					if(i != k && find_set(i) == find_set(k))
					{
						if(dis[i][k] > maxi)
							maxi = dis[i][k];
					}
					if(j != k && find_set(j) == find_set(k))
					{
						if(dis[j][k] > maxj)
							maxj = dis[j][k];
					}
				}
				temp = maxi + maxj + dis[i][j];
				if(temp < Max)
				{
					Max = temp;
				}
				dis[i][j] = dis[j][i] = 10000000.0;
			}		
		}
}
void output()
{
	if(tt < Max)
		fout&lt;&lt;fixed&lt;&lt;setprecision(6)&lt;&lt;Max&lt;&lt;endl;
	else
		fout&lt;&lt;fixed&lt;&lt;setprecision(6)&lt;&lt;tt&lt;&lt;endl;
}
int main()
{
	input();
	make_set();
	caldis();
	floyd();
	slove();
	output();
}
</pre>
