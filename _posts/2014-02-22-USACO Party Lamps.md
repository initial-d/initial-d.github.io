---
layout: post
title: USACO/Party Lamps
date: 2014-02-22 11:27:57
subtitle: USACO/Party Lamps
author: dym
header-img:
catalog:   true
tags:
     -算法
---

本文迁移自老博客，原始链接为 <https://seven.blog.ustc.edu.cn/usacoparty-lamps/>

1~N 共N盏灯，有四个按钮，
按钮1：当按下此按钮，将改变所有的灯：本来亮着的灯就熄灭，本来是关着的灯被点亮。 
按钮2：当按下此按钮，将改变所有奇数号的灯。
按钮3：当按下此按钮，将改变所有偶数号的灯。
按钮4：当按下此按钮，将改变所有序号是3*K+1(K>=0)的灯。例如：1,4,7...
给定操作次数和经过若干操作后某些灯的状态。求出所有灯最后可能的与所给出信息相符的状态，并且没有重复，初始时全亮。
解法：根据鸽巢原理，大于四次操作后某些按钮上必然被重复按，按两次相当于不按，所以将次数每次减2，直到小于等于4，然后再求出该操作次数所对应的可能状态，然后选出符合条件的并排序输出。
<pre class = "brush:[cpp]">
#include&lt;iostream&gt;
#include&lt;algorithm&gt;
#include&lt;fstream&gt;
#include&lt;map&gt;
#include&lt;vector&gt;
using namespace std;

vector&lt;string&gt;temp;	
int n , c;
int open[100];
int close[100];
void add0()
{
	string str;
	for(int i = 0 ;i < n ;i++)
		str += '1';
	temp.push_back(str);
}
void add1()
{
	string str , str1 , str2 ,str3;
	for(int i = 0 ;i < n ;i++)
		str += '0';
	temp.push_back(str);
	for(int i = 0 ;i < n ;i++)
	{
		if(i % 2 == 1)
			str1 += '1';
		else
			str1 += '0';
	}
	temp.push_back(str1);
	for(int i = 0 ;i < n ;i++)
	{
		if(i % 2 == 1)
			str2 += '0';
		else
			str2 += '1';
	}
	temp.push_back(str2);
	for(int i = 0 ;i < n ;i++)
	{
		if(i % 3 == 0)
			str3 += '0';
		else
			str3 += '1';
	}
	temp.push_back(str3);

}

void odd3()
{
	string str5 , str6;
	for(int i = 0 ;i < n ;i++)
	{
		if(i % 3 == 0)
			str5 += '0';
		else
			str5 += '1';
	}
	for(int i = 0 ;i < n ;i++)
	{
		if(i % 2 ==0)
		{
			if(str5[i] == '1')
				str5[i] = '0';
			else
				str5[i] = '1';
		}
	}
	temp.push_back(str5);
	for(int i = 0 ;i < n ;i++)
	{
		if(i % 3 == 0)
			str6 += '0';
		else
			str6 += '1';
	}
	for(int i = 0 ;i < n ;i++)
	{
		if(i % 2 == 1)
		{
			if(str6[i] == '1')
				str6[i] = '0';
			else
				str6[i] = '1';
		}
	}
	temp.push_back(str6);
}

void two()
{
	string str , str1 , str2 ,str3 , str4,str5,str6;
	for(int i = 0 ;i < n ;i++)
	{
		if(i % 2 == 1)
			str1 += '1';
		else
			str1 += '0';
	}
	temp.push_back(str1);
	for(int i = 0 ;i < n ;i++)
	{
		if(i % 2 == 1)
			str2 += '0';
		else
			str2 += '1';
	}
	temp.push_back(str2);
	for(int i = 0 ;i < n ;i++)
	{
		if(i % 3 == 0)
			str3 += '1';
		else
			str3 += '0';
	}
	temp.push_back(str3);
	for(int i = 0 ;i < n ;i++)
		str4 += '0';
	temp.push_back(str4);
	odd3();
}

void add2()
{
	string str , str1 , str2 ,str3 , str4,str5,str6;
	for(int i = 0 ;i < n ;i++)
		str += '1';
	temp.push_back(str);
	two();
}
void add3()
{
	add1();
	string str , str1;
	for(int i = 0 ;i < n ;i++)
		str += '1';
	temp.push_back(str);
	odd3();
	for(int i = 0 ;i < n ;i++)
	{
		if(i % 3 == 0)
			str1 += '0';
		else
			str1 += '1';
	}
	temp.push_back(str1);
}
void add4()
{
	string str , str1;
	for(int i = 0 ;i < n ;i++)
		str += '1';
	temp.push_back(str);
	two();
	for(int i = 0 ;i < n ;i++)
	{
		if(i % 3 == 0)
			str1 += '0';
		else
			str1 += '1';
	}
	temp.push_back(str1);
}
int main()
{
	ofstream fout ("lamps.out");
	ifstream fin ("lamps.in");
	fin&gt;&gt;n;
	fin&gt;&gt;c;
	int t , k1 = 0 , k2 = 0;
	while(fin&gt;&gt;t && t != -1)
		open[k1++] = t;
	while(fin&gt;&gt;t && t != -1)
		close[k2++] = t;
	while(c > 4)
		c -= 2;
	if(c == 0)
		add0();
	else if(c == 1)
		add1();
	else if(c == 2)
		add2();
	else if(c == 3)
		add3();
	else 
		add4();
	sort(temp.begin() , temp.end());
	int hi = 0;
	for(int i = 0 ; i < temp.size() ;i++)
	{
		bool flag = true;
		for(int j = 0 ;j < k1 ; j++)
			if(temp[i][open[j] - 1] != '1')
				flag = false;
		for(int j = 0 ;j < k2 ; j++)
			if(temp[i][close[j] - 1] != '0')
				flag = false;
		if(flag)
		{
			fout&lt;&lt;temp[i]&lt;&lt;endl;
			hi++;
		}
	}
	if(hi == 0)
		fout&lt;&lt;"IMPOSSIBLE"&lt;&lt;endl;
 	return 0;
}
</pre>
