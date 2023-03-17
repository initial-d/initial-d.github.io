---
layout: post
title: USACO/Barn Repair
date: 2014-02-15 20:41:29
subtitle: USACO/Barn Repair
author: dym
header-img:
catalog:   true
tags:
     -算法
---

本文迁移自老博客，原始链接为 <https://seven.blog.ustc.edu.cn/usacobarn-repair/>

给出:可能买到的木板最大的数目，牛棚的总数，牛棚里牛的总数，和牛所在的牛棚的编号，计算拦住所有有牛的牛棚所需木板的最小总长度。  
解法：贪心，注意输入数据的无序性，注意细节（m=1的情况）
<pre class = "brush : [cpp]">
#include &lt;iostream&gt;
#include &lt;fstream&gt;
#include &lt;string&gt;
#include &lt;algorithm&gt;
using namespace std;

int m , s ,c;
int ans[200];
struct sep
{
    int dis;
    int num;
};
bool cmp(sep a , sep b)
{
    return a.dis > b.dis;
}
bool cmp1(sep a , sep b)
{
    return  a.num < b.num;
}
bool cmp2(int a,int b)
{
    return a < b;
}
int main() {
    ofstream fout ("barn1.out");
    ifstream fin ("barn1.in");
    fin&gt;&gt;m&gt;&gt;s&gt;&gt;c;
    if(m >= c)
    {
        fout&lt;&lt;c&lt;&lt;endl;
    }
    else
    {
        sep tmp[200];
        sep tmp1[200];
        int totall = 0;
        int t = 0;
        int s = 0;
        for(int i = 0; i < c; i++)
        {
            fin&gt;&gt;ans[i];
        }
        sort(ans , ans + c ,cmp2);
        for(int i = 0; i < c;i++)
        {
            if(i > 0)
            {
                tmp[t].dis = ans[i] - ans[i-1];
                tmp[t++].num = i;
            }
        }
        sort(tmp , tmp + t, cmp);
        if(m == 1)
            tmp1[s++] = tmp[t - 1];
        else
        {
            while(m-- > 1)
            {
                tmp1[s] = tmp[s];
                s++;
            }
        }
        sort(tmp1 , tmp1 , cmp1);
        for(int i = 0 ; i < s ;i++)
        {
            if(m == 1)
                totall += ans[c-1] - ans[0];
            else if(i == 0)
            {
                totall += ans[tmp1[i].num - 1] - ans[0] + 1;
            }
            else
                totall += ans[tmp1[i].num - 1] - ans[tmp1[i - 1].num] + 1;
        }
        totall += ans[c - 1] - ans[tmp1[s - 1].num] + 1;
        fout&lt;&lt;totall&lt;&lt;endl;
    }
    return 0;
}
</pre>
