---
layout: post
title: USACO/Stamps
date: 2014-05-17 15:16:57
subtitle: USACO/Stamps
author: dym
header-img:
catalog:   true
tags:
     -算法
---

本文迁移自老博客，原始链接为 <https://seven.blog.ustc.edu.cn/usacostamps/>

知道一个 N 枚邮票的面值集合（如，{1 分，3 分}）和一个上限 K —— 表示信封上能够贴 K 张邮票。计算从 1 到 M 的最大连续可贴出的邮资，输出M。
解法：动态规划，另ans[i]表示贴出i所需的最少邮票数，则有：
ans[i] = min(ans[i - cnt[j]] + 1),其中cnt表示邮票面值，ans[0] = 0
<pre class = "brush:[cpp]">
#include&lt;iostream&gt;
#include&lt;algorithm&gt;
#include&lt;fstream&gt;
using namespace std;
ofstream fout ("stamps.out");
ifstream fin ("stamps.in");
int k , n ;
int cnt[51];
int ans[2000010];
int main()
{
    fin&gt;&gt;k&gt;&gt;n;
    int m = 0;
    for(int i = 0 ; i < n ; i++)
    {
        fin&gt;&gt;cnt[i];
        if(cnt[i] > m)
            m = cnt[i];
    }
    sort(cnt , cnt + n);
    memset(ans , 1&lt;&lt;30 , sizeof(ans));
    ans[0] = 0;
    for(int i = 1 ; i <= k * m + 1;i++)
    {
        int Min = 1&lt;&lt;30;
        for(int j = 0 ; j < n ; j++)
        {
            if(i - cnt[j] >= 0)
            {
                int temp = ans[i - cnt[j]] + 1;
                if(Min > temp)
                    Min = temp;
            }
            else
                break;
        }
        ans[i] = Min;
        if(Min > k)
        {
            fout&lt;&lt;i - 1&lt;&lt;endl;
            break;
        }
    }
    return 0;
}
</pre>
