---
layout: post
title: USACO/Stringsobits
date: 2014-05-20 09:40:54
subtitle: USACO/Stringsobits
author: dym
header-img:
catalog:   true
tags:
     -算法
---

本文迁移自老博客，原始链接为 <https://seven.blog.ustc.edu.cn/usacostringsobits/>

给定排好序的N(N<=31)位二进制数,输出第i小的，长度为N，且1的位数的个数小于等于L的那个二进制数。
解法：递归的输出字符,ans[n][l]表示长度为n，1的位数小于等于L的二进制数的个数，则ans[n][l] = dfs(n-1 , l)+dfs(n-1 , l-1),dfs(n , l)是求解ans[n][l]的递归函数，dfs的过程使用了记忆化搜索，递归的划分依据是首位是不是1。
<pre class = "brush:[cpp]">
#include&lt;iostream&gt;
#include&lt;algorithm&gt;
#include&lt;fstream&gt;
using namespace std;

ofstream fout ("kimbits.out");
ifstream fin ("kimbits.in");

int ans[33][33];
unsigned int n , l , i;
int dfs(int n , int l)
{
    if(n == 0 || l == 0)
        return 1;
    if(ans[n][l])
        return ans[n][l];
    ans[n][l] = dfs(n - 1 , l) + dfs(n - 1 , l - 1);
    return ans[n][l];
}
int main()
{
    fin&gt;&gt;n&gt;&gt;l&gt;&gt;i;
    i--;
    for(int j = n ; j >= 1 ;j--)
    {
        if(i && dfs(j - 1 , l) <= i)
        {
            fout&lt;&lt;1;
            i -= dfs(j - 1 , l);
            l--;
        }
        else{
            fout&lt;&lt;0;
        }
    }
    fout&lt;&lt;endl;
    return 0;
}
</pre>
