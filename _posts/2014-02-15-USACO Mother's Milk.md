---
layout: post
title: USACO/Mother's Milk
date: 2014-02-15 21:58:23
subtitle: USACO/Mother's Milk
author: dym
header-img:
catalog:   true
tags:
     -算法
---

本文迁移自老博客，原始链接为 <https://seven.blog.ustc.edu.cn/usacomothers-milk/>

三个容量分别是A,B,C升的桶，A,B,C分别是三个从1到20的整数。最初，A和B桶都是空的，而C桶是装满牛奶的。农民执行把牛奶从一个桶倒到另一个桶中的操作，直到被灌桶装满或原桶空了。找出当A桶是空的时候，C桶中牛奶所剩量的所有可能性。
解法：宽度优先搜索
<pre class = "brush:[cpp]">
#include&lt;iostream&gt;
#include&lt;string&gt;
#include&lt;string.h&gt;
#include&lt;queue&gt;
#include&lt;fstream&gt;
#include&lt;algorithm&gt;
using namespace std;
struct milk{
  int a;
  int b;
  int c;
}foo1,foo;
bool vis[21][21];
queue&lt;milk&gt; q;

void bfs(){
  milk foo2,foo3 ;
  while(!q.empty()){
	foo2 = q.front();
	foo3 = foo2;
	q.pop();
	vis[foo2.a][foo2.b] = true;
	if(foo.b - foo2.b > 0){
	  if(foo.b - foo2.b >= foo2.a){
		foo2.b += foo2.a;
		foo2.a = 0;
		if(!vis[foo2.a][foo2.b])
		  q.push(foo2);
	  }
	  else{
		foo2.a -= (foo.b - foo2.b);
		foo2.b = foo.b;
		if(!vis[foo2.a][foo2.b])
		  q.push(foo2);
	  }
	}
	foo2 = foo3;
	if(foo.a - foo2.a > 0){
	  if(foo.a - foo2.a >= foo2.b){
		foo2.a += foo2.b;
		foo2.b = 0;
		if(!vis[foo2.a][foo2.b])
		  q.push(foo2);
	  }
	  else{
		foo2.b -= (foo.a - foo2.a);
		foo2.a = foo.a;
		if(!vis[foo2.a][foo2.b])
		  q.push(foo2);
	  }
	}
	foo2 = foo3;
	if(foo.a - foo2.a > 0){
	  if(foo.a - foo2.a >= foo2.c){
		foo2.a += foo2.c;
		foo2.c = 0;
		if(!vis[foo2.a][foo2.b])
		  q.push(foo2);
	  }
	  else{
		foo2.c -= (foo.a - foo2.a);
		foo2.a = foo.a;
		if(!vis[foo2.a][foo2.b])
		  q.push(foo2);
	  }
	}
	foo2 = foo3;
	if(foo.c - foo2.c > 0){
	  if(foo.c - foo2.c >= foo2.a){
		foo2.c += foo2.a;
		foo2.a = 0;
		if(!vis[foo2.a][foo2.b])
		  q.push(foo2);
	  }
	  else{
		foo2.a -= (foo.c - foo2.c);
		foo2.c = foo.c;
		if(!vis[foo2.a][foo2.b])
		  q.push(foo2);
	  }
	}
	foo2 = foo3;
	if(foo.b - foo2.b > 0){
	  if(foo.b - foo2.b >= foo2.c){
		foo2.b += foo2.c;
		foo2.c = 0;
		if(!vis[foo2.a][foo2.b])
		  q.push(foo2);
	  }
	  else{
		foo2.c -= (foo.b - foo2.b);
		foo2.b = foo.b;
		if(!vis[foo2.a][foo2.b])
		  q.push(foo2);
	  }
	}
	foo2 = foo3;
	if(foo.c - foo2.c > 0){
	  if(foo.c - foo2.c >= foo2.b){
		foo2.c += foo2.b;
		foo2.b = 0;
		if(!vis[foo2.a][foo2.b])
		  q.push(foo2);
	  }
	  else{
		foo2.b -= (foo.c - foo2.c);
		foo2.c = foo.c;
		if(!vis[foo2.a][foo2.b])
		  q.push(foo2);
	  }
	}
  }
}
int main()
{
  ofstream fout ("milk3.out");
  ifstream fin ("milk3.in");
  fin&gt;&gt;foo.a&gt;&gt;foo.b&gt;&gt;foo.c;
  foo1.a = 0;
  foo1.b = 0;
  foo1.c  = foo.c;
  memset(vis , false , sizeof(vis));
  q.push(foo1);
  bfs();
  int ans[21];
  int t = 0;
  for(int i = 0; i < 21; i++){
	if(vis[0][i])
	  ans[t++] = foo.c - i;
  }
  sort(ans , ans + t);
  fout&lt;&lt;ans[0];
  for(int i = 1;  i < t; i++)
	fout&lt;&lt;" "&lt;&lt;ans[i];
  fout&lt;&lt;endl;
  return 0;
}
</pre>
