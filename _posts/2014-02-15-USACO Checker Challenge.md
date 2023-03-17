---
layout: post
title: USACO/Checker Challenge
date: 2014-02-15 22:26:59
subtitle: USACO/Checker Challenge
author: dym
header-img:
catalog:   true
tags:
     -算法
---

本文迁移自老博客，原始链接为 <https://seven.blog.ustc.edu.cn/usacochecker-challenge/>

求解n皇后问题的解。
解法：回溯法求前三种解的情况；位操作法求总的步数，参考了matrix67的位操作方法。
<pre class = "brush:[cpp]">
#include&lt;stdio.h&gt;
#include&lt;iostream&gt;
#include&lt;stdlib.h&gt;
#include&lt;fstream&gt;
using namespace std;
ofstream fout ("checker.out");
ifstream fin ("checker.in");
int a[20]={0};
int num = 0;
int sum = 0;
int upperlim ;
bool flag = true;
bool fit(int i)
{
    for(int k=i-1;k>=1;k--)
    {
        if(a[k]==a[i]||abs(a[i]-a[k])==i-k)
            return false;
    }
    return true;
}
void queen(int i,int n)
{
  if(i>n)
    {
	   num++;
	   if(num <= 3){
		 fout&lt;&lt;a[1];
		 for(int j=2;j<=n;j++)
		   {
			 fout&lt;&lt;" "&lt;&lt;a[j];
		   }
		 fout&lt;&lt;endl;
	   }
	   else 
		 flag = false;
    }
    else
    {
	  for(int j=1;j<=n;j++)
        {
		  a[i]=j;
		  if(flag && fit(i))
			queen(i+1,n);
		  a[i]=0;
        }
    }
}
void test(int row , int ld , int rd){
  int pos , p;
  if(row != upperlim){
	pos = upperlim & ~(row | ld | rd);
	while(pos){
	  p = pos & -pos;
	  pos -= p;
	  test(row + p , (ld + p)&lt;&lt;1 , (rd + p)&gt;&gt;1);
	}
  }
  else
          sum++;
}
int main()
{
  int n ;
  fin&gt;&gt;n;
  queen(1,n);
  upperlim = (1 << n) - 1;
  test(0 , 0 , 0);
  fout&lt;&lt;sum&lt;&lt;endl;
  return 0;
}
</pre>

