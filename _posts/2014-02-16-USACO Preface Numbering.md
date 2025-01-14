---
layout: post
title: USACO/Preface Numbering
date: 2014-02-16 21:43:48
subtitle: USACO/Preface Numbering
author: dym
header-img:
catalog:   true
tags:
     -算法
---

本文迁移自老博客，原始链接为 <https://seven.blog.ustc.edu.cn/usacopreface-numbering/>

传统罗马数字用单个字母表示特定的数值，给定罗马数字的组成规则，统计从1~n的数字转化为罗马数字以后，各个字母出现的总次数
解法：先预处理整数的各个位用罗马数字的表示方式，并存入数组，然后将整数转换成罗马数字并进行统计。
<pre class = "brush:[cpp]">
#include&lt;iostream&gt;
#include&lt;algorithm&gt;
#include&lt;fstream&gt;
#include&lt;vector&gt;
#include&lt;map&gt;
using namespace std;

int n;
string thousand[4] = {"","M","MM","MMM"};
string hundred[10] = {"","C","CC","CCC","CD","D","DC","DCC","DCCC","CM"};
string decade[10] = {"","X","XX","XXX","XL","L","LX","LXX","LXXX","XC"};
string unit[10] = {"","I","II","III","IV","V","VI","VII","VIII","IX"};
string preface(int i)
{
	string str;
	if(i / 1000 != 0)
	{
		str += thousand[i / 1000];
		i %= 1000;
	}
	if(i / 100 != 0)
	{
		str += hundred[i / 100];
		i %= 100;
	}
	if(i / 10 != 0)
	{
		str += decade[i / 10];
		i %= 10;
	}
	if(i / 1 != 0)
		str += unit[i / 1];
	return str;
}
map&lt;char , int&gt; ans;
void cal(string str)
{
	for(int i = 0 ; i < str.size(); i++)
		ans[str[i]]++;
}
int main()
{
	ofstream fout ("preface.out");
	ifstream fin ("preface.in");
	fin&gt;&gt;n;
	char cnt[8] = "IVXLCDM";
	for(int i = 1 ; i <= n ; i++)
		cal(preface(i));
	for(int i = 0 ; i < 7 ; i++)
	{
		if(ans[cnt[i]])
			fout&lt;&lt;cnt[i]&lt;&lt;" "&lt;&lt;ans[cnt[i]]&lt;&lt;endl;
	}
 	return 0;
}
</pre>
