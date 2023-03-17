---
layout: post
title: USACO/Arithmetic Progressions
date: 2014-02-15 21:49:25
subtitle: USACO/Arithmetic Progressions
author: dym
header-img:
catalog:   true
tags:
     -算法
---

本文迁移自老博客，原始链接为 <https://seven.blog.ustc.edu.cn/usacoarithmetic-progressions/>

写一个程序来找出在双平方数集合(双平方数集合是所有能表示成p的平方+q的平方的数的集合,其中p和q为非负整数)S中长度为n的等差数列。 
解法：先打表，然后暴力枚举，加一个关键性的剪枝，见如下标记。
<pre class = "brush:[cpp]">
#include &lt;iostream&gt;
#include &lt;fstream&gt;
#include &lt;string&gt;
#include &lt;stdio.h&gt;
#include &lt;algorithm&gt;

using namespace std;
struct q
{
  int a;
  int b;
}ans[100000];
bool cmp(q x , q y)
{
  if(x.b == y.b)
	return x.a < y.a;
  else
	return x.b < y.b;
}
int t = 0;
int M , N;
int main()
{
   ofstream fout ("ariprog.out");
   ifstream fin ("ariprog.in");
   fin&gt;&gt;N;
   fin&gt;&gt;M;
   bool test[M * M * 2 + 200000] ;
   memset(test , false , sizeof(test));
   for(int i =0 ;i <= M ;i++){
	 for(int j = 0; j <= M ;j++){
	   test[i * i + j * j] = true;
	 }
   }
   for(int i = 0; i <= M * M * 2;i++){
	 if(test[i])
	   for(int j = i + 1; j <= M * M * 2 ; j++){
		 if(test[j]){
		   int cha = j - i;
		   int k = N - 2;
		   if((k * cha + j) > M * M * 2)//剪枝操作！
			 break;
		   int l;
		   for(l = 1; l <= k; l++){
			 if(!test[j + cha * l] || ((j + cha * k) > M * M * 2))
			   break;
		   }
		   if(l <= k || ((j + cha * k) > M * M * 2))
			 continue;
		   else{
			 ans[t].a = i ;
			 ans[t++].b = cha;
		   }
		 }
	   }
   }
   sort(ans , ans + t , cmp);
   if(t == 0)
	 fout&lt;&lt;"NONE"&lt;&lt;endl;
   else
	 for(int i = 0;i < t; i++ ){
	   fout&lt;&lt;ans[i].a&lt;&lt;" "&lt;&lt;ans[i].b&lt;&lt;endl;
	 }
   return 0;
} 
</pre>