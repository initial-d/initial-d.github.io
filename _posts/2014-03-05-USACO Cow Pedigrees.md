---
layout: post
title: USACO/Cow Pedigrees
date: 2014-03-05 19:27:03
subtitle: USACO/Cow Pedigrees
author: dym
header-img:
catalog:   true
tags:
     -算法
---

本文迁移自老博客，原始链接为 <https://seven.blog.ustc.edu.cn/usacocow-pedigrees/>

二叉树总共有N个节点(3 <= N < 200)。有如下性质:每一个节点的度是0或2。树的高度等于K(1 < K <100)。高度是从根到最远的那个叶子所需要经过的结点数;叶子是指没有孩子的节点。求有多少不同的结构，输出结果是数的结构数模9901？
解法：记忆化搜索，先搜树高小于等于k的种类数，再搜树高小于等于k-1的种类数，两者相减即可，用数组记录搜索的中间结果，避免重复计算。注意偶数节点的树结构种类是0.奇数节点的树可以利用对称性减少计算次数。另外由于种类数可能比较多，所以中间结果直接模9901再存储，最后输出时记得先加9901再模，否则容易出现负数。
<pre class = "brush:[cpp]">
#include&lt;iostream&gt;
#include&lt;algorithm&gt;
#include&lt;string&gt;
#include&lt;fstream&gt;
using namespace std;
int n , k;
int f[201];
int high ;
int matrix[100][200];
int  dfs(int m)
{
	if(m == 1)
		return 1;
	if(m == 2)
		return 0;
	int temp = high;
	f[m] = 0;
	if(matrix[k - high][m])
		return matrix[k - high][m];
	if(m % 2 == 1)
	{
		for(int j = 1 ; j <= (m - 2) / 2 ;j++)
		{
			if(high + 1 < k)
			{
				high++;
				if(!matrix[k - high][j] && j % 2 != 0)
					matrix[k - high][j] = dfs(j);
				if(!matrix[k - high][m - j - 1] && (m - j - 1) % 2 != 0)
					matrix[k - high][m - j - 1] = dfs(m - j - 1);
				f[m] += matrix[k - high][j] * matrix[k - high][m - j - 1] % 9901;
			}
			else
				break;
			high = temp;
		}
		f[m] *= 2;
		if(high + 1 < k)
		{
			high++;
			if(!matrix[k - high][(m - 2) / 2 + 1] && ((m - 2) / 2 + 1) % 2 != 0)
				matrix[k - high][(m - 2) / 2 + 1] = dfs((m - 2) / 2 + 1);
			f[m] += matrix[k - high][(m - 2) / 2 + 1] * matrix[k - high][(m - 2) / 2 + 1] % 9901;
		}
		high = temp;
		matrix[k - high][m] = f[m];
	}
	return f[m] % 9901;
}
int main()
{
	ofstream fout("nocows.out");
	ifstream fin("nocows.in");
	fin&gt;&gt;n&gt;&gt;k;
	for(int i = 0; i < 201 ; i++)
		f[i] = 0;
	f[1] = 1;
	f[2] = 0;
	high = 0;
	int t1 = dfs(n);
	for(int i = 0; i < 201 ; i++)
		f[i] = 0;
	f[1] = 1;
	f[2] = 0;
	high = 0;
	k--;
	int t2 = dfs(n);
	fout&lt;&lt;(t1 + 9901 - t2) % 9901&lt;&lt;endl;
}
</pre>
