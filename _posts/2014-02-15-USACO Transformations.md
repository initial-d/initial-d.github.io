---
layout: post
title: USACO/Transformations
date: 2014-02-15 19:19:52
subtitle: USACO/Transformations
author: dym
header-img:
catalog:   true
tags:
     -算法
---

本文迁移自老博客，原始链接为 <https://seven.blog.ustc.edu.cn/usacotransformations/>

给定一块N x N（1<=N<=10)正方形。写一个程序来找出将原图案按照以下列转换方法中的一个步骤转换成新图案的最小方式(即序号最小的那个) 
1.将图案按顺时针转90度。 
2.将图案按顺时针转180度。 
3.将图案按顺时针转270度。 
4.图案在水平方向翻转 
5.图案在水平方向翻转，然后再按照1到3之间的一种再次转换。 
6.原图案不改变。 
7.无法用以上方法得到新图案。
解法：模拟操作过程，然后枚举。 
<pre class = "brush:[cpp]">
#include &lt;iostream&gt;
#include &lt;fstream&gt;
#include &lt;string&gt;
#include &lt;algorithm&gt;
using namespace std;

int n;
void roate(char tmp[11][11] , char s[11][11])
{
    for(int i = 0;i < n; i++)
        for(int j = 0;j < n; j++)
        {
            tmp[j][n - 1 - i] = s[i][j];
        }
}
void mirror(char tmp[11][11] , char s[11][11])
{
    for(int i = 0;i < n; i++)
        for(int j = 0;j < n; j++)
        {
            tmp[j][i] = s[j][n - 1 - i];
        }
}
bool same(char tmp[11][11] , char s[11][11])
{
    for(int i = 0;i < n; i++)
        for(int j = 0;j < n; j++)
        {
            if(tmp[i][j] != s[i][j])
                return false;
        }
    return true;
}
int main() {
    ofstream fout ("transform.out");
    ifstream fin ("transform.in");
    fin&gt;&gt;n;
    char s[11][11],tmp[11][11],s1[11][11],tmp1[11][11];
    for(int i = 0;i < n; i++)
        for(int j = 0;j < n; j++)
        {
            fin&gt;&gt;s[i][j];
        }
    for(int i = 0;i < n; i++)
        for(int j = 0;j < n; j++)
            fin&gt;&gt;s1[i][j];
    if(1 == 2)
        fout&lt;&lt;"hello"&lt;&lt;endl;
    else
    {
        roate(tmp , s);
        if(same(tmp , s1))
            fout&lt;&lt;'1'&lt;&lt;endl;
        else
        {
            roate(tmp1 , tmp);
            if(same(tmp1 , s1))
                fout&lt;&lt;'2'&lt;&lt;endl;
            else
            {
                roate(tmp , tmp1);
                if(same(tmp , s1))
                    fout&lt;&lt;'3'&lt;&lt;endl;
                else
                {
                    mirror(tmp , s);
                    if(same(tmp  , s1))
                        fout&lt;&lt;'4'&lt;&lt;endl;
                    else
                    {
                        roate(tmp1 , tmp);
                        if(same(tmp1 , s1))
                            fout&lt;&lt;'5'&lt;&lt;endl;
                        else
                        {
                            roate(tmp , tmp1);
                            if(same(tmp , s1))
                                fout&lt;&lt;'5'&lt;&lt;endl;
                            else
                            {
                                roate(tmp1 , tmp);
                                if(same(tmp1 , s1))
                                    fout&lt;&lt;'5'&lt;&lt;endl;
                                else if(same(s , s1))
                                    fout&lt;&lt;'6'&lt;&lt;endl;
                                else
                                    fout&lt;&lt;'7'&lt;&lt;endl;
                            }
                        }
                    }
                }
            }
        }
    }
    return 0;
}
</pre>

