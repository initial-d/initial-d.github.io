---
layout: post
title: USACO/Factorials
date: 2014-05-20 08:46:51
subtitle: USACO/Factorials
author: dym
header-img:
catalog:   true
tags:
     -算法
---

本文迁移自老博客，原始链接为 <https://seven.blog.ustc.edu.cn/usacofactorials/>

求阶乘最后的非零位，n<=4220
解法：直接模拟，每次乘法后保留后四位数
<pre class = "brush:[cpp]">
#include&lt;iostream&gt;
#include&lt;algorithm&gt;
#include&lt;fstream&gt;
using namespace std;

ofstream fout ("fact4.out");
ifstream fin ("fact4.in");

int n;
int main()
{
    fin&gt;&gt;n;
    int t = 1;
    for(int i = 2 ; i <= n ; i++)
    {
        t = t * i;
        while(t % 10 == 0)
            t /= 10;
        t %= 10000;
    }
    fout&lt;&lt;t%10&lt;&lt;endl;
    return 0;
}
</pre>

