---
layout: post
title: USACO/A Game
date: 2014-05-25 21:29:41
subtitle: USACO/A Game
author: dym
header-img:
catalog:   true
tags:
     -算法
---

本文迁移自老博客，原始链接为 <https://seven.blog.ustc.edu.cn/usacoa-game/>

双人游戏:N个正整数的序列，由玩家1开始，两人轮流从序列的任意一端取一个数，取数后该数字被去掉并累加到本玩家的得分中，当数取尽时，游戏结束。以最终得分多者为胜。编一个执行最优策略的程序，最优策略就是使玩家在与最好的对手对弈时，能得到的在当前情况下最大的可能的总分的策略。程序要始终为第二位玩家执行最优策略。
解法:动态规划，dp[i][j]为剩余序列为i~j时玩家2所能得到的最大分数，则当前轮到玩家一时，dp[i][j] = min(dp[i+1][j] , dp[i][j-1])，当前轮到玩家二时，dp[i][j] = max(dp[i+1][j] + cnt[i], dp[i][j-1] + cnt[j])
<pre class = "brush:[cpp]">
#include&lt;iostream&gt;
#include&lt;stdio.h&gt;
#include&lt;string.h&gt;
#include&lt;fstream&gt;
#include&lt;queue&gt;
#define INF 1000000050
using namespace std;
ofstream fout ("game1.out");
ifstream fin ("game1.in");

int n;
int cnt[201];
int dp[201][201];
int sum = 0;
int main()
{
    fin&gt;&gt;n;
    for(int i = 1 ; i <= n; i++)
    {
        fin&gt;&gt;cnt[i];
        sum += cnt[i];
    }
    for(int i = n ; i >= 1 ; i--)
        for(int j = i ; j <= n ;j++)
    {
        if((n - (j - i + 1)) & 1 == 1)
            dp[i][j] = max(dp[i+1][j] + cnt[i], dp[i][j-1] + cnt[j]);
        else
            dp[i][j] = min(dp[i+1][j] , dp[i][j-1]);
    }
    fout&lt;&lt;sum - dp[1][n]&lt;&lt;" "&lt;&lt;dp[1][n]&lt;&lt;endl;
    return 0;
}
</pre>
