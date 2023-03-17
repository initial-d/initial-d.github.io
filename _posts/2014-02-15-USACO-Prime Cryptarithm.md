---
layout: post
title: USACO/Prime Cryptarithm
date: 2014-02-15 20:59:06
subtitle: USACO/Prime Cryptarithm
author: dym
header-img:
catalog:   true
tags:
     -算法
---

本文迁移自老博客，原始链接为 <https://seven.blog.ustc.edu.cn/usacoprime-cryptarithm/>

寻找牛式：用给定的那n个数字来取代*，可以使式子成立的话，这个式子就是牛式。      
      * * *
   x    * *
    -------
      * * *         <-- partial product 1
    * * *           <-- partial product 2
    -------
    * * * *

解法：暴力搜索，注意一定的剪枝条件：数字的位数，还可以找其他条件优化。
<pre class = "brush:[cpp]">
#include &lt;iostream&gt;
#include &lt;fstream&gt;
#include &lt;string&gt;
#include &lt;algorithm&gt;
using namespace std;

int n;
int test[10];
bool search1[10];
int a,b,c,d,e;
int ans;
bool check(int m){
  while(m != 0){
	int tmp = m % 10;
	if(!search1[tmp]){
	  return false;
	}
	m = m / 10;
  }
  return true;
}
int main()
{
  ofstream fout ("crypt1.out");
  ifstream fin ("crypt1.in");
  fin&gt;&gt;n;
  for(int i = 0; i < n ;i++){
	fin&gt;&gt;test[i];
	search1[test[i]] = true;
  }
  ans = 0;
  for(int i = 0 ; i < n ; i++)
	 for(int j = 0 ; j < n ; j++)
	    for(int k = 0 ; k < n ; k++)
		   for(int l = 0 ; l < n ; l++)
			 for(int h = 0 ; h < n ; h++){
			   int tmp1, tmp2 ,tmp3;
			   tmp1 = test[i] * 100 + test[j] * 10 + test[k];
			   tmp2 = test[l] * 10 + test[h];
			   if(check(tmp1 * test[h]) && (tmp1 * test[h] <= 999)){
				 if(check(tmp1 * test[l]) && (tmp1 * test[l] <= 999)){
				   tmp3 = tmp2 * tmp1;
				   if(check(tmp3) && (tmp3 <= 9999) ){
					 ans++;
					 //cout<<tmp3<<endl;
				   }
				 }
			   }
			 }			  
  fout&lt;&lt;ans&lt;&lt;endl;
  return 0;
}
</pre>
