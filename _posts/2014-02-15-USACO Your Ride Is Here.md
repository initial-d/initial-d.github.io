---
layout: post
title: USACO/Your Ride Is Here
date: 2014-02-15 16:29:49
subtitle: USACO/Your Ride Is Here
author: dym
header-img:
catalog:   true
tags:
     -算法
---

本文迁移自老博客，原始链接为 <https://seven.blog.ustc.edu.cn/usacoyour-ride-is-here/>

usaco的第一个题，不需要什么技巧。熟悉一下输入输出。
<pre class = "brush:[cpp]">
#include &lt;iostream&gt;
#include &lt;fstream&gt;
#include &lt;string&gt;

using namespace std;

long long work(string tmp)
{
    int i = 0;
    long long ans = 1;
    int len = tmp.size();
    while(i < len)
    {
        ans = ans * (tmp[i] - 'A' + 1);
        i++;
    }
    return ans;
}
int main() {
    ofstream fout ("ride.out");
    ifstream fin ("ride.in");
    string a,b;
    fin&gt;&gt;a;
    fin&gt;&gt;b;
    if((work(a)%47) == (work(b)%47))
        fout&lt;&lt;"GO"&lt;&lt;endl;
    else
        fout&lt;&lt;"STAY"&lt;&lt;endl;
    return 0;
}
</pre>

