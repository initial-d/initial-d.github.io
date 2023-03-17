---
layout: post
title: USACO/Superprime Rib
date: 2014-02-15 22:17:58
subtitle: USACO/Superprime Rib
author: dym
header-img:
catalog:   true
tags:
     -算法
---

本文迁移自老博客，原始链接为 <https://seven.blog.ustc.edu.cn/usacosuperprime-rib/>

写一个程序对给定的 N (1<=N<=8),求出所有的特殊质数。 数字1不被看作一个质数。举例来说: 7331是质数，733是质数，73 是质数，7 也是质数。 7331 被叫做长度 4 的特殊质数。
解法：注意最高位必为2 3 5 7，其他位必为1 3 7 9，然后枚举即可，这里重复用的队列进行枚举。
<pre class = "brush:[cpp]">
#include &lt;iostream&gt;
#include &lt;fstream&gt;
#include &lt;string&gt;
#include &lt;algorithm&gt;
#include &lt;queue&gt;
using namespace std;
int n;
int k;
queue &lt;int&gt; q;
bool isp(int x)
{
	if((x%5==0&&x!=5)||x%3==0)
		return false;
	if(x%11==0&&x!=11)
		return false;
	if(x%2==0)
		return false;
	int m=sqrt(x);
	for(int i=2;i<=m;i++)
		if(x%i==0)
			return false;
	return true;
}
void func(int num)
{
  while(num--){
	if(isp(q.front() * 10 + 1))	{
	  q.push(q.front() * 10 + 1);
	  k++;
	}
	if(isp(q.front() * 10 + 3))	{
	  q.push(q.front() * 10 + 3);
	  k++;
	}
	if(isp(q.front() * 10 + 7))	{
	  q.push(q.front() * 10 + 7);
	  k++;
	}
	if(isp(q.front() * 10 + 9))	{
	  q.push(q.front() * 10 + 9);
	  k++;
	}
	q.pop();
  }
}
void findsp(int n){
  k = 0;
  func(4);
  if(n == 2)
	return ;
  int h = k;
  k = 0;
  func(h);
  if(n == 3)
	return ;
  h = k;
  k = 0;
  func(h);
  if(n == 4)
	return ;
  h = k;
  k = 0;
  func(h);
  if(n == 5)
	return ;
  h = k;
  k = 0;
  func(h);
  if(n == 6)
	return ;
  h = k;
  k = 0;
  func(h);
  if(n == 7)
	return ;
  h = k;
  k = 0;
  func(h);
  if(n == 8)
	return ;
}
int main(){
  ofstream fout ("sprime.out");
  ifstream fin ("sprime.in");
  fin&gt;&gt;n;
  if(n == 1){
	fout&lt;&lt;2&lt;&lt;endl;
	fout&lt;&lt;3&lt;&lt;endl;
	fout&lt;&lt;5&lt;&lt;endl;
	fout&lt;&lt;7&lt;&lt;endl; 
  }
  else{
	q.push(2);
	q.push(3);
	q.push(5);
	q.push(7);
	findsp(n);
  }
  while(!q.empty()){
	fout&lt;&lt;q.front()&lt;&lt;endl;
	q.pop();
  }
  return 0;
}
</pre>
