---
layout: post
title: USACO/Sweet Butter
date: 2014-05-20 10:18:37
subtitle: USACO/Sweet Butter
author: dym
header-img:
catalog:   true
tags:
     -算法
---

本文迁移自老博客，原始链接为 <https://seven.blog.ustc.edu.cn/usacosweet-butter/>

给定一个图，求图上所有点中到几个给定点的距离和的最小值。
解法：邻接表实现的spfa
<pre class = "brush:[cpp]">
#include&lt;iostream&gt;
#include&lt;algorithm&gt;
#include&lt;fstream&gt;
#include&lt;stdio.h&gt;
#include&lt;string.h&gt;
#include&lt;queue&gt;;
using namespace std;

ofstream fout ("butter.out");
ifstream fin ("butter.in");
#define INF 1000000050
int niu[501];

bool vis[801];
struct no
{
     int u,v,nxt;
}node[10005];
int edge[10005];
int inq[10005];
int n,p,q;
long long sum,dis[10005];
int a,b,d,c;
void init()
{
     for(int i=1;i<=2*c;i++)
     {
          edge[i]=0;
     }
}
void spfa(no node[] ,int edge[] , int sou)
{
     int i;
     for(int j=1;j<=p;j++)
          inq[j]=false;
     queue&lt;int&gt; que;
     que.push(sou);
     inq[sou] = true;
     for (i=1; i<=p; i++)
     {
          dis[i] = INF;
     }
     dis[sou] = 0;
     while (!que.empty())
     {
          int s = que.front();
          que.pop();
          inq[s] = false;
          int p=edge[s];
          while( p!=0)
          {
               int v=node[p].v;
               int u=node[p].u;
               if (dis[v] > dis[s] + u)
               {
                    dis[v] = dis[s] + u;
                    if (!inq[v])
                    {
                         que.push(v);
                         inq[v] = true;
                    }
               }
               p=node[p].nxt;
          }
     }
}
int main()
{
    fin&gt;&gt;n&gt;&gt;p&gt;&gt;c;
    init();
    for(int i = 0 ; i < n ;i++)
        fin&gt;&gt;niu[i];
    for(int i=1; i <= 2 * c;)
    {
        fin&gt;&gt;a&gt;&gt;b&gt;&gt;d;
        node[i].v=b;
        node[i].u=d;
        node[i].nxt=edge[a];
        edge[a]=i++;

        node[i].v=a;
        node[i].u=d;
        node[i].nxt=edge[b];
        edge[b]=i++;
    }
    int Min = INF;
    for(int i = 1; i <= p; i ++)
    {
        sum = 0;
        spfa(node , edge , i);
        for(int j = 0 ; j < n ; j++)
        {
            sum += dis[niu[j]];
        }
        if(Min > sum)
            Min = sum;
    }
    fout&lt;&lt;Min&lt;&lt;endl;
    return 0;
}
</pre>
