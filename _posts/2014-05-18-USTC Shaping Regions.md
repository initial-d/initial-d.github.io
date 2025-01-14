---
layout: post
title: USTC/Shaping Regions
date: 2014-05-18 10:49:45
subtitle: USTC/Shaping Regions
author: dym
header-img:
catalog:   true
tags:
     -算法
---

本文迁移自老博客，原始链接为 <https://seven.blog.ustc.edu.cn/ustcshaping-regions/>

n个不同颜色的不透明长方形放置在一张长方形的白纸上，各个长方形的边和白纸的边平行放置，以白纸的左下角为坐标原点，问从上向下俯视能够看到的各种颜色的面积。
解法：枚举每一层，递归的用上一层对当前所枚举的层进行切割，对切割后剩下的矩形继续向上重复此操作，当到达最上层则代表可以被看到，将其面积加到此颜色所对应的面积和上。
<pre class = "brush:[cpp]">
#include&lt;iostream&gt;
#include&lt;algorithm&gt;
#include&lt;fstream&gt;
using namespace std;

ofstream fout ("rect1.out");
ifstream fin ("rect1.in");
int a , b , n;
struct rect
{
    int x1;
    int y1;
    int x2;
    int y2;
    int color;
};
rect cnt[1001];
int ans[2501];
void dfs(int x1 , int y1 , int x2 , int y2 , int color , int k)
{
    while((x2 <= cnt[k].x1 || x1 >= cnt[k].x2 || y2 <= cnt[k].y1 || y1 >= cnt[k].y2) && k <= n)
        k++;
    if(k > n)
    {
        ans[color] += (y2 - y1) * (x2 - x1);
        return;
    }
    if(x1 == x2 || y1 == y2)
        return;
    if(x1 < cnt[k].x1)
    {
        dfs(x1 , y1 , cnt[k].x1 , y2 , color , k + 1);
        x1 = cnt[k].x1;
    }
    if(x2 > cnt[k].x2)
    {
        dfs(cnt[k].x2 , y1 , x2 , y2 , color , k + 1);
        x2 = cnt[k].x2;
    }
    if(y1 < cnt[k].y1)
    {
        dfs(x1 , y1 , x2 , cnt[k].y1 , color , k + 1);
        y1 = cnt[k].y1;
    }
    if(y2 > cnt[k].y2)
    {
        dfs(x1 , cnt[k].y2 , x2 , y2 , color , k + 1);
        y2 = cnt[k].y2;
    }
}
int main()
{
    fin&gt;&gt;a&gt;&gt;b&gt;&gt;n;
    cnt[0].x1 = cnt[0].y1 = 0;
    cnt[0].x2 = a ;
    cnt[0].y2 = b;
    cnt[0].color = 1;
    for(int i = 1 ; i <= n ;i++)
    {
        fin&gt;&gt;cnt[i].x1&gt;&gt;cnt[i].y1&gt;&gt;cnt[i].x2&gt;&gt;cnt[i].y2&gt;&gt;cnt[i].color;
    }
    memset(ans , 0 , sizeof(ans));
    for(int i = 0 ; i <= n ; i++)
    {
        dfs(cnt[i].x1 , cnt[i].y1 , cnt[i].x2 , cnt[i].y2 , cnt[i].color , i + 1);
    }
    for(int i = 1 ; i <= 2500 ; i++)
    {
        if(ans[i])
            fout&lt;&lt;i&lt;&lt;" "&lt;&lt;ans[i]&lt;&lt;endl;
    }
    return 0;
}
</pre>
