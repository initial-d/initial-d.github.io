---
layout: post
title: USACO/Sorting a Three-Valued Sequence
date: 2014-02-10 22:30:15
subtitle: USACO/Sorting a Three-Valued Sequence
author: dym
header-img:
catalog:   true
tags:
     -算法
---

本文迁移自老博客，原始链接为 <https://seven.blog.ustc.edu.cn/110/>

/*
给一系列数，只包含1,2,3，求将其排列成有序所需的最少交换次数
解法：首先将数字存到另一个数组然后排序，排序后的数组位置对应原数组中元素的目标位置，然后对原数组从头到尾进行扫描，并和已排序数组进行对比，如果相同则跳过，否则从当前扫描位置向后找可以用来交换的数字，优先找通过交换能使两个数字都在目标位置的数字进行交换，若没有这样的数字，则找不在目标位置且以当前扫描位置为目标位置的数字进行交换。
如果直接找不在目标位置且以当前扫描位置为目标位置的数字进行交换，结果不对，必须优先找通过交换能使两个数字都在目标位置的数字进行交换，应该用贪心算法来解释吧。
*/
<pre class = "brush:[cpp]">
#include&lt;iostream&gt;
#include&lt;algorithm&gt;
#include&lt;fstream&gt;
using namespace std;
int n,num;
int cnt[1001],ans[1001];
int main()
{
	ofstream fout ("sort3.out");
         ifstream fin ("sort3.in");
	fin&gt;&gt;n;
	for(int i = 0 ; i < n ;i++)
	{
		fin&gt;&gt;ans[i];
		cnt[i] = ans[i];
	}
	sort(ans , ans + n);
	num = 0;
	int temp;
	for(int i = 0 ; i < n; i++)
	{
		if(ans[i] == cnt[i])
			continue;
		else
		{
			bool flag = true;
			for(int j = i + 1; j < n ; j++)
			{
				if(ans[j] != cnt[j] && cnt[j] == ans[i] && cnt[i] == ans[j])
				{
					temp = cnt[i];
					cnt[i] = cnt[j];
					cnt[j] = temp;
					num++;
					flag = false;
					break;
				}
			}
			if(flag)
				for(int j = i + 1; j < n ; j++)
				{
					if(ans[j] != cnt[j] && cnt[j] == ans[i])
					{
						temp = cnt[i];
						cnt[i] = cnt[j];
						cnt[j] = temp;
						num++;
						break;
					}
				}
		}
	}
	fout&lt;&lt;num&lt;&lt;endl;
 	return 0;
}
</pre>
