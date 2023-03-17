---
layout: post
title: USACO/Spinning Wheels
date: 2014-05-20 09:48:33
subtitle: USACO/Spinning Wheels
author: dym
header-img:
catalog:   true
tags:
     -算法
---

本文迁移自老博客，原始链接为 <https://seven.blog.ustc.edu.cn/usacospinning-wheels/>

给定有缺口的五个同心圆，各自以不同的速度旋转，问最短经过多少时间五个同心圆的缺口可以出现重叠
解法：模拟
<pre class = "brush:[cpp]">
#include&lt;iostream&gt;
#include&lt;algorithm&gt;
#include&lt;fstream&gt;
#include&lt;vector&gt;
#include&lt;queue&gt;
#include&lt;sstream&gt;
using namespace std;

ofstream fout ("spin.out");
ifstream fin ("spin.in");
struct wheel
{
    int v;
    int w;
    int start[6];
    int ext[6];
    int num;
};
wheel cnt[5];
bool vis[361];
vector&lt;int&gt; vec;
int ans;
bool flag = false;
int main()
{
    for(int i = 0; i < 5 ; i++)
    {
        fin&gt;&gt;cnt[i].v&gt;&gt;cnt[i].num;
        for(int j = 0 ; j < cnt[i].num ; j++)
            fin&gt;&gt;cnt[i].start[j]&gt;&gt;cnt[i].ext[j];
    }
    for(int t = 0 ; t < 360 ; t++)
    {
        if(flag)
            break;
        memset(vis , false , sizeof(vis));
        for(int i = 0 ; i < 5 ; i++)
        {
            vec.clear();
            for(int k = 0 ; k < cnt[i].num ; k++)
            {
                int temp = cnt[i].start[k];
                temp += cnt[i].v * t;
                for(int m = 0; m <= cnt[i].ext[k]; m++)
                {
                    if(i == 0)
                        vis[(temp + m) % 360] = true;
                    else
                    {
                        if(vis[(temp + m) % 360] == true)
                            vec.push_back((temp + m) % 360);
                    }
                }
            }
            if(i != 0)
            {
                memset(vis , false , sizeof(vis));
                for(int k = 0 ; k < vec.size() ; k++)
                {
                    vis[vec[k]] = true;
                }
            }
        }
        for(int i = 0 ; i <= 360 ; i++)
        {
            if(vis[i])
            {
                ans = t;
                flag = true;
                break;
            }
        }
    }
    if(!flag)
        fout&lt;&lt;"none"&lt;&lt;endl;
    else
        fout&lt;&lt;ans&lt;&lt;endl;
    return 0;
}
</pre>