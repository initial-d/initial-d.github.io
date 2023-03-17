---
layout: post
title: USACO/Contact
date: 2014-05-17 14:08:07
subtitle: USACO/Contact
author: dym
header-img:
catalog:   true
tags:
     -算法
---

本文迁移自老博客，原始链接为 <https://seven.blog.ustc.edu.cn/usacocontact/>

寻找长度在A到B之间（包含A和B本身）的重复得最多的比特序列 (1 <= A <= B <= 12)。输出规定个数的出现频率最多的序列。
例输入：
2 4 10
01010010010001000111101100001010011001111000010010011110010000000
输出：
23
00
15
01 10
12
100
11
11 000 001
10
010
8
0100
7
0010 1001
6
111 0000
5
011 110 1000
4
0001 0011 1100
解法：先将符合规定长度的所有比特序列枚举出，然后使用这些序列构造字典树，然后使用字典树对输入字符串进行计数，每次以输入字符串的下一个字母为起点进行枚举，然后遍历字典树得到所有子序列对应的次数，最后排序并按照规定的次序和格式进行输出，程序又长又搓：（。
<pre class = "brush :[cpp]">
#include&lt;iostream&gt;
#include&lt;algorithm&gt;
#include&lt;fstream&gt;
#include&lt;map&gt;
#include&lt;stdio.h&gt;
#include&lt;string.h&gt;
#include&lt;vector&gt;
using namespace std;
typedef pair&lt;string, int&gt; PAIR;
ofstream fout ("contact.out");
ifstream fin ("contact.in");
int a , b, n;
string str;
vector&lt;string&gt; vec;
map&lt;string , int&gt; mm;
string ss;
bool cmp(PAIR a , PAIR b)
{
    if(a.second == b.second)
    {
        if(a.first.size() == b.first.size())
            return a.first < b.first;
        return a.first.size() < b.first.size();
    }
    return a.second > b.second;
}
void input()
{
	fin&gt;&gt;a&gt;&gt;b&gt;&gt;n;
	string ttt;
	while(getline(fin , ttt))
        str += ttt;
	string temp;
	for(int i = a ; i <= b ; i++)
	{
		for(int k = 0 ; k < pow(2 , i) ;k++)
		{
			temp.clear();
			for(int j = 0 ; j < i ; j++)
				temp += '0';
			int t = k;
			for(int j = 0 ; j < i ; j++)
			{
				if((k&gt;&gt;j) & 1)
					temp[i - 1 - j] = '1';
			}
			vec.push_back(temp);
		}
	}
}
int num;
struct node
{
     int next[2];
     int number;
     void init()
     {
          number = -2;
          next[0] = next[1] = -2;
     }
};
node tree[100010];
void insert(string a)
{
     int index = 0;
     int len = a.size();
     for(int i = 0;i < len ; i++)
     {
          if(tree[index].next[a[i]-'0'] == -2)
          {
			  tree[++num].init();
			  tree[index].next[a[i]-'0'] = num;
			  index = num;
          }
          else
          {
               index = tree[index].next[a[i]-'0'];
          }
		  if(i == len - 1)
			  tree[index].number = 0;
     }
}
void dfs(int index)
{
    if(index != -2)
    {
        if(tree[index].number > 0 && tree[index].next[0] != -2 && tree[index].next[1] != -2)
            mm[ss] = tree[index].number;
        if(tree[index].next[0] != -2)
        {
            string temp = ss;
            ss += '0';
            dfs(tree[index].next[0]);
            ss = temp;
        }
        else if(tree[index].number)
            mm[ss] = tree[index].number;
        if(tree[index].next[1] != -2)
        {
            string temp = ss;
            ss += '1';
            dfs(tree[index].next[1]);
            ss = temp;
        }
        else if(tree[index].number)
            mm[ss] = tree[index].number;
    }

}

int main()
{
	input();
	tree[0].init();
	num = 0;
	for(int i = 0 ; i < vec.size() ; i++)
	{
		insert(vec[i]);
	}
	for(int i = 0 ; i < str.size() ;i++)
	{
		int j = i;
		int index = 0;
	    while(index != -2)
		{
	    	if(tree[index].number >= 0)
				tree[index].number++;
            if(str[j] == ' ')
                j++;
            if(j > str.size())
                break;
			index = tree[index].next[str[j]-'0'];
			j++;
			if(j > str.size()) //当j = str.size()时所找到的index无效，退出
                break;
		}
	}
	ss.clear();
	dfs(0);
	vector&lt;PAIR&gt; vec1;
    for(map&lt;string , int&gt;::iterator it = mm.begin(); it != mm.end(); it++)
    {
        if(it->second)
            vec1.push_back(make_pair(it->first, it->second));
    }
    sort(vec1.begin() , vec1.end() , cmp);
    int t = 1;
    int coun = 0;
    bool flag = false;
    fout&lt;&lt;vec1[0].second&lt;&lt;endl;
    fout&lt;&lt;vec1[0].first;
    coun++;
    if(vec1.size() == 1)
    {
        fout&lt;&lt;endl;
        return 0;
    }
    int i;
    for(i = 1 ; i < vec1.size();i++)
    {
        if(vec1[i].second != vec1[i - 1].second)
        {
            flag = false;
            fout&lt;&lt;endl;
            coun = 0;
            t++;
            if(t == n + 1)
                break;
            fout&lt;&lt;vec1[i].second&lt;&lt;endl;
            fout&lt;&lt;vec1[i].first;
            coun++;
        }
        else
        {
            if(flag)
            {
                fout&lt;&lt;endl;
                flag = false;
                fout&lt;&lt;vec1[i].first;
            }
            else
                fout&lt;&lt;" "&lt;&lt;vec1[i].first;
            coun++;
            if(coun == 6)
            {
                coun = 0;
                flag = true;
            }
        }
    }
    if(i == vec1.size())
        fout&lt;&lt;endl;
	return 0;
}
</pre>
