---
layout: post
title: USACO/Riding the Fences
date: 2014-05-20 15:38:27
subtitle: USACO/Riding the Fences
author: dym
header-img:
catalog:   true
tags:
     -算法
---

本文迁移自老博客，原始链接为 <https://seven.blog.ustc.edu.cn/usacoriding-the-fences/>

打印欧拉路。因为需要打印进制转换后最小的数字序列，所以每次要选最小的开始递归，最后逆序输出。这里使用邻接矩阵比邻接表方便，因为使用邻接矩阵不需要再排序。
<pre class = "brush:[cpp]">
#include&lt;iostream&gt;
#include&lt;stdio.h&gt;
#include&lt;string.h&gt;
#include&lt;fstream&gt;
#include&lt;queue&gt;
#define INF 1000000050
using namespace std;
ofstream fout ("fence.out");
ifstream fin ("fence.in");
int cnt[501][501];
int du[501];
int ans[501];
int F;
int tt;
void euler(int s)
{
    for(int k = 1 ; k <= 500 ; k++)
    {
        if(cnt[s][k] > 0)
        {
            cnt[s][k]--;
            cnt[k][s]--;
            euler(k);
        }
    }
    tt++;
    ans[tt] = s;

}
int main()
{
    memset(cnt , 0 , sizeof(cnt));
    memset(du , 0 , sizeof(du));
    memset(ans , 0 , sizeof(ans));
    fin&gt;&gt;F;
    int a , b;
    for(int i = 0 ; i < F; i++)
    {
        fin&gt;&gt;a&gt;&gt;b;
        cnt[a][b]++;
        cnt[b][a]++;
        du[a]++;
        du[b]++;
    }
    int i , j;
    tt = 0;
    for(i = 0 ; i <= 500 ; i++)
        if(du[i] % 2 == 1)
            break;
    if(i == 501)
    {
        for(j = 0 ; j <= 500 ;j++)
            if(du[j])
                break;
        euler(j);
    }
    else
        euler(i);
    for(i = tt ; i > 0 ; i--)
        fout&lt;&lt;ans[i]&lt;&lt;endl;
    return 0;
}
</pre>

