---
layout: post
title: USACO/Milking Cows
date: 2014-02-15 17:26:05
subtitle: USACO/Milking Cows
author: dym
header-img:
catalog:   true
tags:
     -算法
---

本文迁移自老博客，原始链接为 <https://seven.blog.ustc.edu.cn/usacomilking-cows/>

有N个农民(1 <= N <= 5000)挤N头牛的工作时间列表，计算出以下两个时间段(均以秒为单位): 
1.最长至少有一人在挤奶的时间段 
2.最长的无人挤奶的时间段 
解法：对区间排序优化，然后穷举
<pre class = "brush:[cpp]">
#include &lt;iostream&gt;
#include &lt;fstream&gt;
#include &lt;string&gt;
#include &lt;algorithm&gt;
using namespace std;

struct interval
{
    int beg;
    int end;
};
bool cmp(interval a , interval b)
{
    return a.beg < b.beg;
}
int n;
int main() {
    ofstream fout ("milk2.out");
    ifstream fin ("milk2.in");
    fin&gt;&gt;n;
    interval cowlist[n] , tmp[n];
    for(int i = 0; i < n ;i++)
    {
        fin&gt;&gt;cowlist[i].beg&gt;&gt;cowlist[i].end;
    }
    sort(cowlist , cowlist + n , cmp);
    tmp[0] = cowlist[0];
    int t = 0;
    int tmp1,tmp2;
    for(int i = 1; i < n; i++)
    {
        if((cowlist[i].beg >= tmp[t].beg) && (cowlist[i].beg <= tmp[t].end))
        {
            if(cowlist[i].end > tmp[t].end)
                tmp[t].end = cowlist[i].end;
        }
        else
        {
            tmp[++t] = cowlist[i];
        }
    }
    int max1 = 0 , max2 = 0;
    for(int i = 0; i <= t ; i++)
    {
        tmp1 = tmp[i].end - tmp[i].beg;
        if(tmp1 > max1)
            max1 = tmp1;
        if(i < t)
            tmp2 = tmp[i + 1].beg - tmp[i].end ;
        if((tmp2 > max2) && (i < t))
            max2 = tmp2;
    }
    fout&lt;&lt;max1&lt;&lt;" "&lt;&lt;max2&lt;&lt;endl;
    return 0;
}
</pre>

