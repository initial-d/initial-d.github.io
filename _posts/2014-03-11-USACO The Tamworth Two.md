---
layout: post
title: USACO/The Tamworth Two
date: 2014-03-11 19:38:44
subtitle: USACO/The Tamworth Two
author: dym
header-img:
catalog:   true
tags:
     -算法
---

本文迁移自老博客，原始链接为 <https://seven.blog.ustc.edu.cn/usacothe-tamworth-two/>

给定一个矩阵表示一张图，"."表示通路，"*"表示障碍物，农夫和牛分别从一点向同一方向出发行走，每一分钟，要么在前方没有障碍的情况下向前走一步，要么在前方有障碍的情况下顺时针旋转90度（不走）。问经过多少时间相遇或回答不能相遇。
解法：直接模拟行走过程，若相遇则输出结果；若时间超过（4*10*10）*（4*10*10）= 160000则不会相遇，160000是10*10矩阵上的所有可能状态数。
<pre class = "brush:[cpp]">
#include&lt;iostream&gt;
#include&lt;algorithm&gt;
#include&lt;fstream&gt;
#include&lt;map&gt;
#include&lt;vector&gt;
using namespace std;

char cnt[11][11];
int n;
int time1 = 0;
int cowx , cowy , farmerx , farmery , dreC , dreF;
bool flag = true;
int main()
{
	ofstream fout ("ttwo.out");
    ifstream fin ("ttwo.in");
	int temp1x ,temp1y ,temp2x ,temp2y; 
	for(int i = 1 ; i < 11 ;i++)
	{
		for(int j = 1 ; j < 11 ;j++)
		{
			fin&gt;&gt;cnt[i][j];
			if(cnt[i][j] == 'C')
			{
				cowx = i;
				cowy = j;				
			}
			if(cnt[i][j] == 'F')
			{
				farmerx = i;
				farmery = j;
			}
		}
	}
	dreC = 0;
	dreF = 0;
	while(1)
	{
		if(cowx == farmerx && cowy == farmery)
			break;
		time1++;
		if(dreC == 0)
		{
			if(cowx - 1 > 0 && cnt[cowx - 1][cowy] != '*')
			{
				cowx--;
			}
			else
			{
				dreC++;
				dreC %= 4;
			}	
		}
		else if(dreC == 1)
		{
			if(cowy + 1 <= 10 && cnt[cowx][cowy + 1] != '*')
			{
				cowy++;
			}
			else
			{
				dreC++;
				dreC %= 4;
			}	
		}
		else if(dreC == 2)
		{
			if(cowx + 1 <= 10 && cnt[cowx + 1][cowy] != '*')
			{
				cowx++;
			}
			else
			{
				dreC++;
				dreC %= 4;
			}	
		}
		else if(dreC == 3)
		{
			if(cowy - 1 > 0 && cnt[cowx][cowy - 1] != '*')
			{
				cowy--;
			}
			else
			{
				dreC++;
				dreC %= 4;
			}	
		}

		if(dreF == 0)
		{
			if(farmerx - 1 > 0 && cnt[farmerx - 1][farmery] != '*')
			{
				farmerx--;
			}
			else
			{
				dreF++;
				dreF %= 4;
			}	
		}
		else if(dreF == 1)
		{
			if(farmery + 1 <= 10 && cnt[farmerx][farmery + 1] != '*')
			{
				farmery++;
			}
			else
			{
				dreF++;
				dreF %= 4;
			}	
		}
		else if(dreF == 2)
		{
			if(farmerx + 1 <= 10 && cnt[farmerx + 1][farmery] != '*')
			{
				farmerx++;
			}
			else
			{
				dreF++;
				dreF %= 4;
			}	
		}
		else if(dreF == 3)
		{
			if(farmery - 1 > 0 && cnt[farmerx][farmery - 1] != '*')
			{
				farmery--;
			}
			else
			{
				dreF++;
				dreF %= 4;
			}	
		}
		if(time1 > 160000)
		{
			flag = false;
			break;
		}
	}
	if(flag)
		fout&lt;&lt;time1&lt;&lt;endl;
	else
		fout&lt;&lt;0&lt;&lt;endl;
}
</pre>
