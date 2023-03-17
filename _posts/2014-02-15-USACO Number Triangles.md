---
layout: post
title: USACO/Number Triangles
date: 2014-02-15 22:03:26
subtitle: USACO/Number Triangles
author: dym
header-img:
catalog:   true
tags:
     -算法
---

本文迁移自老博客，原始链接为 <https://seven.blog.ustc.edu.cn/usaconumber-triangles/>

数字金字塔,写一个程序来查找从最高点到底部任意处结束的路径，使路径经过数字的和最大。
解法：最直接的动态规划
<pre class="brush:[cpp]">
#include &lt;iostream&gt;
#include &lt;fstream&gt;
#include &lt;string&gt;
#include &lt;algorithm&gt;
using namespace std;

int r;
int foo[1001][1001];
int ans[1001][1001];
int main()
{
  ofstream fout ("numtri.out");
  ifstream fin ("numtri.in");
  fin&gt;&gt;r;
  for(int i = 0; i < r; i++)
	for(int j = 0; j < i + 1 ; j++){
	  fin&gt;&gt;foo[i][j];
	}
  for(int i = r - 1 ; i >= 0 ; i-- )
	for(int j = 0; j < i + 1 ; j++){
	  if(i == (r - 1)){
		ans[i][j] = foo[i][j];
	  }
	  else{
		ans[i][j] = max(ans[i + 1][j] , ans[i + 1][j + 1]) + foo[i][j];
	   }
	}
  fout&lt;&lt;ans[0][0]&lt;&lt;endl;
  return 0;
}
</pre>