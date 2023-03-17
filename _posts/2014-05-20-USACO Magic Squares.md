---
layout: post
title: USACO/Magic Squares
date: 2014-05-20 10:09:30
subtitle: USACO/Magic Squares
author: dym
header-img:
catalog:   true
tags:
     -算法
---

本文迁移自老博客，原始链接为 <https://seven.blog.ustc.edu.cn/usacomagic-squares/>

给定一个固定长度的字符串，并给定在这个字符串上的3种操作，问通过不停地使用三种基本操作，原字符串能变换为目标字符串所需的最少操作步数，并输出操作序列。
解法：bfs , 注意将整型数组转化为string的技巧
<pre class = "brush:[cpp]">
/*
ID: duyimin1
PROG: msquare
LANG: C++
*/
#include&lt;iostream&gt;
#include&lt;algorithm&gt;
#include&lt;fstream&gt;
#include&lt;map&gt;
#include&lt;stdio.h&gt;
#include&lt;string.h&gt;
#include&lt;vector&gt;
#include&lt;queue&gt;
#include&lt;set&gt;
#include&lt;sstream&gt;
using namespace std;

ofstream fout ("msquare.out");
ifstream fin ("msquare.in");

struct q
{
    string temp;
    string op;
};
queue&lt;q&gt; que;
set&lt;string&gt; ss;
string str;
string A(string str)
{
    string t(str);
    for(int i = 0 ; i < 8 ; i++)
    {
        t[i] = str[7 - i];
    }
    return t;
}
string B(string str)
{
    string t;
    t = str[3] + str.substr(0 , 3) + str.substr(5 , 3) + str[4];
    return t;
}
string C(string str)
{
    string t(str);
    char c1 = t[2];
    t[2] = t[1];
    char c2 = t[5];
    t[5] = c1;
    char c3 = t[6];
    t[6] = c2;
    t[1] = c3;
    return t;
}
void bfs(string str)
{
    string be = "12345678";
    q b;
    b.temp = be;
    que.push(b);
    ss.insert(be);
    while(!que.empty())
    {
        q s = que.front();
        que.pop();
        if(s.temp == str)
        {
            fout&lt;&lt;s.op.size()&lt;&lt;endl;
            fout&lt;&lt;s.op&lt;&lt;endl;
            return;
        }
        q s1 = s;
        s1.temp = A(s1.temp);
        if(ss.find(s1.temp) == ss.end())
        {
            s1.op += 'A';
            que.push(s1);
            ss.insert(s1.temp);
        }
        s1 = s;
        s1.temp = B(s1.temp);
        if(ss.find(s1.temp) == ss.end())
        {
            s1.op += 'B';
            que.push(s1);
            ss.insert(s1.temp);
        }
        s1 = s;
        s1.temp = C(s1.temp);
        if(ss.find(s1.temp) == ss.end())
        {
            s1.op += 'C';
            que.push(s1);
            ss.insert(s1.temp);
        }
    }
}
string input()
{
    int i = 0;
    int cc;
    string s;
    stringstream sss(s);
    while(i++ < 8)
    {
        fin&gt;&gt;cc;
        sss&lt;&lt;cc;
    }
    return sss.str();
}
int main()
{
    str = input();
    bfs(str);
    return 0;
}
</pre>