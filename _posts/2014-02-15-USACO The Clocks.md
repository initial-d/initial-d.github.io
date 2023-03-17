---
layout: post
title: USACO/The Clocks
date: 2014-02-15 21:42:13
subtitle: USACO/The Clocks
author: dym
header-img:
catalog:   true
tags:
     -算法
---

本文迁移自老博客，原始链接为 <https://seven.blog.ustc.edu.cn/usacothe-clocks/>

求找一个最小的移动顺序将所有的指针指向12点。给出了9种不同的旋转指针的方法。选择1到9号移动方法，将会使在对应的受影响的时钟的指针顺时针旋转90度。 
解法：枚举，一共4的9次方种情况，感觉题目的数据有点弱，没有判断最小的，循环结束输出结果就过了。
<pre class = "brush:[cpp]">
#include &lt;iostream&gt;
#include &lt;fstream&gt;
#include &lt;string&gt;
#include &lt;stdio.h&gt;
#include &lt;math.h&gt;
#include &lt;string.h&gt;
#include &lt;algorithm&gt;
#include &lt;bitset&gt;
using namespace std;

int cent[10],cent1[10];
char chn[11][6] = {"ABDE" , "ABC" , "BCEF" , "ADG", "BDEFH", "CFI", "DEGH","GHI", "EFHI"};
void change(int n,int m)
{
  char* tmp = chn[n - 1];
  while(m--){
	int t =  0;
	while(tmp[t]!= '\0')
	  {
		cent[tmp[t] - 'A'] = cent[tmp[t] - 'A'] + 3;
		if(cent[tmp[t] - 'A'] > 12)
		  cent[tmp[t] - 'A'] = cent[tmp[t] - 'A'] % 12;
		t++;
	  }
  }
}
bool check()
{
  for(int i = 0; i < 3; i++)
	for(int j = 0 ; j < 3; j++){
	  if(cent[i * 3 + j] != 12)
		return false;
	}
  return true;
}
int ans[10];
void find(){
  int Max = 100;
  for(int k = 0; k < 4; k++){
	for(int h = 0; h < 4; h++){
	  for(int g = 0; g < 4; g++){
		for(int f = 0;  f< 4; f++){
		  for(int e = 0; e < 4; e++){
			for(int d = 0; d < 4; d++){		 
			  for(int c = 0; c < 4; c++){
				for(int b = 0; b < 4; b++){
				  for(int a = 0; a < 4; a++){
					change(1,a);
					change(2,b);
					change(3,c);
					change(4,d);
					change(5,e);
					change(6,f);
					change(7,g);
					change(8,h);
					change(9,k);
					if(check()){
					  ans[0] = 1 * a ;
					  ans[1] = 2 * b ;
					  ans[2] = 3 * c ;
					  ans[3] = 4 * d ;
					  ans[4] = 5 * e ;
					  ans[5] = 6 * f ;
					  ans[6] = 7 * g ;
					  ans[7] = 8 * h ;
					  ans[8] = 9 * k ;
					}
					else{
					  for(int o = 0; o < 9 ; o++){
						cent[o] = cent1[o];
					  }
					  continue;
					}
				  }}}}}}}}}
}
int main()
{
   ofstream fout ("clocks.out");
   ifstream fin ("clocks.in");
   for(int i = 0; i < 3; i++)
	 for(int j = 0 ; j < 3; j++){
	   fin&gt;&gt;cent[i * 3 + j];
	   cent1[i * 3 + j] = cent[i * 3 + j];
	 }
   find();
   int  res[30];
   int q = 0;
   for(int i = 0 ;i < 9 ;i++){
	 if(ans[i] != 0){
	   int z = ans[i] / (i + 1);
	   while(z--){
		 res[q++] = (i + 1);
	   }
	 }  
   }
   for(int i = 0; i < q - 1; i++)
	 fout&lt;&lt;res[i]&lt;&lt;" ";
   fout&lt;&lt;res[q - 1];
   fout&lt;&lt;endl;
} 
</pre>