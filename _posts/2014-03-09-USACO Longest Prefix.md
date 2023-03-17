---
layout: post
title: USACO/Longest Prefix
date: 2014-03-09 10:44:26
subtitle: USACO/Longest Prefix
author: dym
header-img:
catalog:   true
tags:
     -算法
---

本文迁移自老博客，原始链接为 <https://seven.blog.ustc.edu.cn/usacolongest-prefix/>

给定一个字符串集合A和一个字符串S，求使用给定集合A中的字符串所能组成的S的最长前缀是多长
解法：首先用给定集合A组成一棵trie，用来进行匹配，然后使用动态规划。dp[i]表示从S串第i个位置开始使用A中的元素所能组成的最长前缀，则dp[0]则为所求。每当从第i个位置开始匹配时，使用trie进行匹配并记录所能达到的最长前缀长度，直到无法匹配。
<pre class = "brush:[cpp]">
#include&lt;iostream&gt;
#include&lt;cstdio&gt;
#include&lt;string.h&gt;
#include&lt;string&gt;
#include&lt;fstream&gt;
using namespace std;
bool ok=true;
string a;
string cnt;
int num;
int dp[200005];
struct node
{
     int next[26];
     int number;
     void init()
     {
          number=0;
          memset(next,0,sizeof(next));
     }
};
node tree[100010];
void insert(string a)
{
     int index=0;
     int len=a.size();
     for(int i=0;i < len;i++)
     {
          if(tree[index].next[a[i]-'A']==0)
          {
               tree[++num].init();
               tree[index].next[a[i]-'A']=num;
               index=num;
          }
          else
          {
               index=tree[index].next[a[i]-'A'];
          }
		  if(i == len - 1)
			  tree[index].number = 1;
     }
}

int main()
{
	ofstream fout ("prefix.out");
    ifstream fin ("prefix.in");
	tree[0].init();
	num=0;
	while(1)
	{
		fin&gt;&gt;a;
		if(a[0] == '.')
			break;
		insert(a);
	}
	string temp;
	while(fin&gt;&gt;temp)
	{
		cnt += temp;
	}
	int count;
	memset(dp , 0 , sizeof(dp));
	for(int i = cnt.size()- 1 ; i >= 0 ; i-- )
	{
		int index=0;
		count = 0;
		int Max = 0;
		for(int j = i; j < cnt.size() ; j++)
		{
			index=tree[index].next[cnt[j]-'A'];
			if(index == 0)
				break;
			if(tree[index].number == 1)
			{
				count++;
				if(Max < count + dp[j + 1])
					Max = count + dp[j + 1];					  
			}
			else
			{
				count++;
			}
		}
		dp[i] = Max;
	}
	fout&lt;&lt;dp[0]&lt;&lt;endl;
	return 0;
}
</pre>