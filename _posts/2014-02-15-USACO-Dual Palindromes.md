---
layout: post
title: USACO/Dual Palindromes
date: 2014-02-15 19:52:15
subtitle: USACO/Dual Palindromes
author: dym
header-img:
catalog:   true
tags:
     -算法
---

本文迁移自老博客，原始链接为 <https://seven.blog.ustc.edu.cn/usacodual-palindromes/>

读入两个十进制数N (1 <= N <= 15)S (0 < S < 10000)然后找出前N个满足大于S且在两种或两种以上进制（二进制至十进制）上是回文数的十进制数，输出到文件上
解法：和上题相似，还是枚举
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
    ofstream fout ("dualpal.out");
    ifstream fin ("dualpal.in");
    int n,m;
    fin&gt;&gt;n&gt;&gt;m;
    int o;
    bool flag;
    int r = 0;
    for(int i = m+1 ; ;i++ )
    {
        o = 0;
        flag =false;
        for(int j = 2; j <= 10 ;j++)
        {
            if(isp(change(i , j)))
            {
                o++;
                if(o >= 2)
                {
                    flag = true;
                    break;
                }
            }
        }
        if(flag)
        {
            fout&lt;&lt;i&lt;&lt;endl;
            r++;
        }
        if(r >= n)
            break;
    }
    return 0;
}
</pre>

