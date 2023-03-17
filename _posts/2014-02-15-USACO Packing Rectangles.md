---
layout: post
title: USACO/Packing Rectangles
date: 2014-02-15 21:21:42
subtitle: USACO/Packing Rectangles
author: dym
header-img:
catalog:   true
tags:
     -算法
---

本文迁移自老博客，原始链接为 <https://seven.blog.ustc.edu.cn/usacopacking-rectangles/>

给了4个矩形块，找出一个最小的封闭矩形将这4个矩形块放入，不可相互重叠。 
<a href="https://seven.blog.ustc.edu.cn/wp-content/uploads/2014/02/pack1.gif"><img src="https://seven.blog.ustc.edu.cn/wp-content/uploads/2014/02/pack1-300x67.gif" alt="pack[1]" width="300" height="67" class="alignnone size-medium wp-image-151" /></a>
解法：枚举，参考了别人的分析。见：http://starforever.blog.hexun.com/2097115_d.html
<pre class = "brush :[cpp]">
#include &lt;iostream&gt;
#include &lt;fstream&gt;
#include &lt;string&gt;
#include &lt;stdio.h&gt;
#include &lt;math.h&gt;
#include &lt;string.h&gt;
#include &lt;algorithm&gt;
#include &lt;bitset&gt;

using namespace std;

struct rec{
  int a;
  int b;
}Rec1[4] , Rec2[4];
struct ans{
  int w1;
  int w2;
  int w3;
  int w4;
  int h1;
  int h2;
  int h3;
  int h4;
};
ans tmp[5000];
struct res{
  int square;
  int w;
  int h;
}result[20000],Rset[10000];
bool cmp(res a , res b)
{
  return a.square < b.square;
}
bool cmp1(res a , res b)
{
  return a.w < b.w;
}
int o;
int max2(int a , int b){
  if(a > b)
	return a;
  else
	return b;
}
int max3(int a , int b , int c){
  return max2(a , max2(b , c));
}
int max4(int a ,int b ,int c, int d ){
  return max2(a , max3(b , c , d));
}
int main()
{
   ofstream fout ("packrec.out");
   ifstream fin ("packrec.in");
   for(int i = 0 ; i < 4;i++){
  	fin&gt;&gt;Rec1[i].a&gt;&gt;Rec1[i].b;
   }
   int t = 0;	 
   int r=0;
   int q[4] = {0 , 1 , 2, 3};
   do
	 {
	   for(int j = 0 ; j < 4 ;j++){
		 Rec2[j] = Rec1[q[j]];
	   }
	   for(int m = 0 ; m < 16 ; m++){
		 bitset&lt;4&gt;b(m);
		 for(int j = 0 ; j < 4 ;j++){
		   Rec2[j] = Rec1[q[j]];
		 }
		 for(int k = 0 ; k < 4 ; k++){
		   if(b.test(k)){
			 int c =  Rec2[k].a;
			 Rec2[k].a = Rec2[k].b;
			 Rec2[k].b = c;
		   }
		 }
		 tmp[t].w1 = Rec2[0].a;
		 tmp[t].h1 = Rec2[0].b;
		 tmp[t].w2 = Rec2[1].a;
		 tmp[t].h2 = Rec2[1].b;
		 tmp[t].w3 = Rec2[2].a;
		 tmp[t].h3 = Rec2[2].b;
		 tmp[t].w4 = Rec2[3].a;
		 tmp[t++].h4 = Rec2[3].b;
	   }
	 }while(next_permutation(q,q+4));
   int p = 0;
   int w,h,sq;
   for(int i = 0; i < t ; i++){
	 w = tmp[i].w1 + tmp[i].w2 + tmp[i].w3 + tmp[i].w4;
	 h = max4(tmp[i].h1,tmp[i].h2,tmp[i].h3,tmp[i].h4);
	 sq = w * h;
	 result[p].w = w;
	 result[p].h = h;
	 result[p++].square = sq;
	 w = max2(tmp[i].w1 + tmp[i].w2 + tmp[i].w3 , tmp[i].w4);
	 h = max3(tmp[i].h1,tmp[i].h2,tmp[i].h3) + tmp[i].h4;
	 sq = w * h;
	 result[p].w = w;
	 result[p].h = h;
	 result[p++].square = sq;
	 w = max2(tmp[i].w1 + tmp[i].w2 , tmp[i].w3) + tmp[i].w4;
	 h = max3(tmp[i].h1 + tmp[i].h3 , tmp[i].h2+tmp[i].h3 , tmp[i].h4);
	 sq = w * h;
	 result[p].w = w;
	 result[p].h = h;
	 result[p++].square = sq;
	 w = tmp[i].w1 + tmp[i].w2 + max2(tmp[i].w3 , tmp[i].w4);
	 h = max3(tmp[i].h1,tmp[i].h3 + tmp[i].h4 , tmp[i].h2);
	 sq = w * h;
	 result[p].w = w;
	 result[p].h = h;
	 result[p++].square = sq;
	
	 h = max2(tmp[i].h1 + tmp[i].h3,tmp[i].h2 + tmp[i].h4);
          if(tmp[i].h3 >= tmp[i].h2 + tmp[i].h4){
	    w = max3(tmp[i].w1 , tmp[i].w3 + tmp[i].w2 , tmp[i].w3 + tmp[i].w4);
	 }
	 else  if((tmp[i].h3 > tmp[i].h4) && (tmp[i].h3 < tmp[i].h2 + tmp[i].h4)){
	    w = max3(tmp[i].w1 + tmp[i].w2 , tmp[i].w3 + tmp[i].w2 , tmp[i].w3 + tmp[i].w4);
	 }
	 else  if((tmp[i].h4 > tmp[i].h3) && (tmp[i].h4 < tmp[i].h1 + tmp[i].h3)){
	    w = max3(tmp[i].w1 + tmp[i].w2 , tmp[i].w1 + tmp[i].w4 , tmp[i].w3 + tmp[i].w4);
	 }
	 else  if(tmp[i].h4 >= tmp[i].h1 + tmp[i].h3){
	    w = max3(tmp[i].w2 , tmp[i].w1 + tmp[i].w4 , tmp[i].w3 + tmp[i].w4);
	 }
	 else  if(tmp[i].h3 == tmp[i].h4){
	    w = max2(tmp[i].w1 + tmp[i].w2 , tmp[i].w3 + tmp[i].w4);
	 }
	 sq = w * h;
	 result[p].w = w;
	 result[p].h = h;
	 result[p++].square = sq;
   }
										  
   sort(result, result + p , cmp);
   if(result[0].w > result[0].h)
	 {
	   Rset[0].w = result[0].h;
	   Rset[0].h = result[0].w;
	   Rset[0].square = result[0].square;
	 }
   else
	 {
	   Rset[0].w = result[0].w;  
	   Rset[0].h = result[0].h;
	   Rset[0].square = result[0].square;
	 }
   o = 1;
   for(int i = 1; i < p; i++ ){
	 if((result[i].square == result[0].square)){
	   if(result[i].w > result[i].h)
		 {
		   Rset[o].w = result[i].h;
		   Rset[o].h = result[i].w;
		   Rset[o++].square = result[i].square;
		 }
	   else
		 {
		   Rset[o].w = result[i].w;  
		   Rset[o].h = result[i].h;
		   Rset[o++].square = result[i].square;
		 }
	 }
	 else{
	    break;
	 }
   }
   sort(Rset , Rset+o  , cmp1);
   fout&gt;&gt;Rset[0].square&gt;&gt;endl;
   fout&gt;&gt;Rset[0].w&gt;&gt;" "&gt;&gt;Rset[0].h&gt;&gt;endl;
   for(int i = 1; i < o; i++){
	 if(Rset[i].w != Rset[i-1].w)
	   fout&gt;&gt;Rset[i].w&gt;&gt;" "&gt;&gt;Rset[i].h&gt;&gt;endl;

   }
   return 0;
}
</pre>