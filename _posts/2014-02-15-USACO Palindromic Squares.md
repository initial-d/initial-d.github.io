---
layout: post
title: USACO/Palindromic Squares
date: 2014-02-15 19:46:10
subtitle: USACO/Palindromic Squares
author: dym
header-img:
catalog:   true
tags:
     -算法
---

本文迁移自老博客，原始链接为 <https://seven.blog.ustc.edu.cn/usacopalindromic-squares/>

给定一个进制B(2<=B<=20,由十进制表示)，输出所有的大于等于1小于等于300（十进制下）且它的平方用B进制表示时是回文数的数。
解法：枚举加字符串转换操作
<pre class = "brush:[cpp]">
#include &lt;iostream&gt;
#include &lt;fstream&gt;
#include &lt;string&gt;
#include &lt;algorithm&gt;
using namespace std;

string change(int a , int b)
{
    char tmp[20];
    string hi;
    int t = 0;
    int ans[20];
    while(1)
    {
        ans[t++] = a % b;
        a = a / b;
        if(a == 0)
            break;
    }
    for(int i = t - 1; i >= 0;i--)
    {
        if(ans[i] <= 9)
            tmp[t - 1 -i] =  ans[i] + '0';
        else
        {
            tmp[t - 1 -i] = 'A' + ans[i] - 10;
        }
    }
    tmp[t] = '\0';
    hi = string(tmp);
    return hi;
}
bool isp(string a)
{
    int len = a.size();
    for(int i = 0; i < len; i++)
    {
        if(a[i] != a[len - i -1])
            return false;
    }
    return true;
}
int main() {
    ofstream fout ("palsquare.out");
    ifstream fin ("palsquare.in");
    int n;
    fin&gt;&gt;n;
    for(int i = 1 ;i <= 300 ;i++ )
    {
        if(isp(change(i * i , n)))
            fout&lt;&lt;change(i , n)&lt;&lt;" "&lt;&lt;change(i * i , n)&lt;&lt;endl;
    }
    return 0;
}
</pre>

