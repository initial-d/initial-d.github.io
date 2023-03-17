---
layout: post
title: USACO/Home on the Range
date: 2014-05-25 21:17:06
subtitle: USACO/Home on the Range
author: dym
header-img:
catalog:   true
tags:
     -算法
---

本文迁移自老博客，原始链接为 <https://seven.blog.ustc.edu.cn/usacohome-on-the-range/>

计算如下形式的数字矩形中由1组成的正方形（边长大于1）的个数。
101111
001111
111111
001111
101101
111001
解法：动态规划。f[i][j]表示由坐标（i,j）为右下角的正方形的最大边长，则：
f[i][j] = min(f[i - 1][j] , min(f[i][j - 1] , f[i-1][j-1])) + 1;
<pre class = "brush:[cpp]">
#include&lt;iostream&gt;
#include&lt;map&gt;
#include&lt;string&gt;
#include&lt;fstream&gt;
#include&lt;algorithm&gt;
#include&lt;string.h&gt;
using namespace std;
ofstream fout ("range.out");
ifstream fin ("range.in");
int cnt[251][251];
int f[251][251];
string str;
int n , M;
int ans[251];
int main()
{
    fin&gt;&gt;n;
    memset(ans , 0 ,sizeof(ans));
    memset(f , 0 , sizeof(f));
    for(int i = 1 ; i <= n; i++)
    {
        fin&gt;&gt;str;
        int len = str.size();
        for(int j = 1 ; j <= len ; j++)
        {
            if(str[j - 1] == '0')
                cnt[i][j] = 0;
            else if(str[j - 1] == '1')
                cnt[i][j] = 1;
        }
    }
    for(int i = 1 ; i <= n ; i++)
        for(int j = 1; j <= n ; j++)
        {
            if(cnt[i][j] == 1)
                f[i][j] = min(f[i - 1][j] , min(f[i][j - 1] , f[i-1][j-1])) + 1;
        }
    for(int i = 1 ; i <= n ; i++)
        for(int j = 1; j <= n ; j++)
            for(int k = 2 ; k <= f[i][j] ; k++)
                ans[k]++;
    for(int i = 1 ; i <= n ; i++)
        if(ans[i] != 0)
            fout&lt;&lt;i&lt;&lt;" "&lt;&lt;ans[i]&lt;&lt;endl;
    return 0;
}
</pre>