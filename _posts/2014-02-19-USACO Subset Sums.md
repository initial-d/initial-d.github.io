---
layout: post
title: USACO/Subset Sums
date: 2014-02-19 12:46:50
subtitle: USACO/Subset Sums
author: dym
header-img:
catalog:   true
tags:
     -算法
---

本文迁移自老博客，原始链接为 <https://seven.blog.ustc.edu.cn/usacosubset-sums/>

把从1到N (1 <= N <= 39) 的连续整数集合划分成两个子集合，且保证每个集合的数字和是相等的。
解法：两个集合的和相同，即将1~N的和的一半进行整数划分（即将和的一半拆成若干不同数的和）；若1~N的和为奇数，则无解。
动态规划的状态方程为：ans[N][sum] = ans[N - 1][sum - N] + ans[N - 1][sum];
ans[N][sum]表示前N个数组成sum的所有方案数，ans[N-1][sum - N]表示包含N的方案数，ans[N-1][sum]表示不包含N的方案数。
<pre class = "brush:[cpp]">
#include&lt;iostream&gt;
#include&lt;algorithm&gt;
#include&lt;fstream&gt;
using namespace std;

int n , sum ,t;
long long ans[40][800];
void dp(int sum)
{
	for(int i = 0; i <= n ; i++)
		ans[i][0] = 1;
	for(int i = 1; i <= sum ; i++)
		ans[0][i] = 0;
	ans[1][1] = 1;
	for(int i = 2; i <= n ;i++)
		for(int j = 1 ; j <= sum ; j++)
		{
			if(i > j)
				ans[i][j] = ans[j][j];
			else
				ans[i][j] = ans[i - 1][j - i] + ans[i - 1][j];
		}
}
int main()
{
	ofstream fout ("subset.out");
	ifstream fin ("subset.in");
	fin&gt;&gt;n;
	sum = 0; 
	t = 0;
	for(int i = 0; i < 40; i++)
		for(int j = 0 ; j < 800 ;j++)
			ans[i][j] = 0;
	for(int i = 1; i <= n; i++)
		sum += i;
	if(sum % 2 == 1)
		fout&lt;&lt;0&lt;&lt;endl;
	else
	{
		dp(sum / 2);
		fout&lt;&lt;ans[n][sum/2] / 2&lt;&lt;endl;
	}
 	return 0;
}
</pre>
