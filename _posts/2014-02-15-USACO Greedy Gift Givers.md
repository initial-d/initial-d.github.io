---
layout: post
title: USACO/Greedy Gift Givers
date: 2014-02-15 16:40:46
subtitle: USACO/Greedy Gift Givers
author: dym
header-img:
catalog:   true
tags:
     -算法
---

本文迁移自老博客，原始链接为 <https://seven.blog.ustc.edu.cn/usacogreedy-gift-givers/>

np个人之间互送礼物，每个人有自己的送礼对象和拥有的钱数，求最后每个人收到的钱比送出的钱多多少？
解法：直接模拟
<pre class = "brush:[cpp]">
#include &lt;iostream&gt;
#include &lt;fstream&gt;
#include &lt;string&gt;
#include &lt;algorithm&gt;
using namespace std;
int np;
struct per
{
    string name;
    int num;
    int s;
    int ans;
}gift[10];
int find(string tmp)
{
    for(int i = 0; i < np; i++)
    {
        if(gift[i].name == tmp)
            return i;
    }
}
int main() {
    ofstream fout ("gift1.out");
    ifstream fin ("gift1.in");
    string mem,giver,reciver;
    fin&gt;&t;np;
    for(int i = 0; i < np ;i++ )
    {
        fin&gt;&gt;mem;
        gift[i].name = mem;
        gift[i].num = 0;
    }
    int ini,n;
    for(int i = 0; i < np; i++)
    {
        fin&gt;&gt;giver;
        fin&gt;&gt;ini&gt;&gt;n;
        gift[find(giver)].s = ini;
        if(n != 0)
        {
            int ava = ini/n;
            for(int j = 0; j < n; j++)
            {
                fin&gt;&gt;reciver;
                gift[find(reciver)].num += ava;
            }
            gift[find(giver)].num += (ini - ava * n);
        }
    }
    for(int i = 0; i < np; i++)
    {
         gift[i].ans = gift[i].num - gift[i].s;
    }
    for(int i = 0; i < np; i++)
    {
        fout&lt;&lt;gift[i].name&lt;&lt;" "&lt;&lt;gift[i].ans&lt;&lt;endl;
    }

    return 0;
}
</pre>

