---
layout: post
title: USACO/Score Inflation
date: 2014-03-15 11:00:27
subtitle: USACO/Score Inflation
author: dym
header-img:
catalog:   true
tags:
     -算法
---

本文迁移自老博客，原始链接为 <https://seven.blog.ustc.edu.cn/usacoscore-inflation/>

完全背包。动态规划的方程是dp[w] = Max( dp[w-o[i].wei]+o[i].val) , dp[w])，方程的形式和0-1背包是相同的，但是容量的循环的顺序不同。
<pre class = "brush:[cpp]">
#include&lt;iostream&gt;
#include&lt;algorithm&gt;
#include&lt;fstream&gt;
#include&lt;string.h&gt;
using namespace std;
ofstream fout ("inflate.out");
ifstream fin ("inflate.in");
const int nMax = 10001;
const int mMax = 10001;

struct point
{
    int val;
    int wei;
}o[mMax];
int main()
{
    int n,m,i,w,dp[nMax];
    fin&gt;&gt;m&gt;&gt;n;
    for(i = 1; i <= n; i++)
        fin&gt;&gt;o[i].val&gt;&gt;o[i].wei;
    memset(dp,0,sizeof(dp));
    for(i = 1; i <= n; i++)
        for(w =o[i].wei; w<=m; w++)
        if(dp[w] < dp[w-o[i].wei] + o[i].val)
            dp[w] = dp[w-o[i].wei] + o[i].val;
    fout&lt;&lt;dp[m]&lt;&lt;endl;
    return 0;
}
</pre>
