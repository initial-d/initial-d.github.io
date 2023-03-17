---
layout: post
title: USACO/Feed Ratios
date: 2014-05-20 10:00:08
subtitle: USACO/Feed Ratios
author: dym
header-img:
catalog:   true
tags:
     -算法
---

本文迁移自老博客，原始链接为 <https://seven.blog.ustc.edu.cn/usacofeed-ratios/>

给出目标饲料 3：4：5 和三种饲料的比例：
    1:2:3   
    3:7:1  
    2:1:2
编程找出使这三种饲料用量最少的能配出目标饲料的方案，要是不能用这三种饲料调配目标饲料，输出“NONE”。例：
8*(1:2:3) + 1*(3:7:1) + 5*(2:1:2) = (21:28:35) = 7*(3:4:5)
解法：由于各种饲料的份数都小于100，所以可以直接暴力枚举,判定时能用乘法，尽量不要用除法。
<pre class = "brush:[cpp]">
#include&lt;iostream&gt;
#include&lt;algorithm&gt;
#include&lt;fstream&gt;
using namespace std;

ofstream fout ("ratios.out");
ifstream fin ("ratios.in");

int a , b , c;
int x1 , y1 , z1;
int x2 , y2 , z2;
int x3 , y3 , z3;
int main()
{
    fin&gt;&gt;a&gt;&gt;b&gt;&gt;c;
    fin&gt;&gt;x1&gt;&gt;y1&gt;&gt;z1;
    fin&gt;&gt;x2&gt;&gt;y2&gt;&gt;z2;
    fin&gt;&gt;x3&gt;&gt;y3&gt;&gt;z3;
    bool flag = false;
    for(int i = 0 ; i < 100 && !flag; i++)
    {
        for(int j = 0 ; j < 100 && !flag;j++)
        {
            for(int k = 0 ; k < 100 && !flag;k++)
            {
                int t1 = x1 * i + x2 *j + x3 * k;
                int t2 = y1 * i + y2 *j + y3 * k;
                int t3 = z1 * i + z2 *j + z3 * k;
                int r;
                if(a && t1 && t1 % a == 0)
                {
                    r = t1 / a;
                    if(r * b == t2 && r * c == t3)
                    {
                        flag = true;
                        fout&lt;&lt;i&lt;&lt;" "&lt;&lt;j&lt;&lt;" "&lt;&lt;k&lt;&lt;" "&lt;&lt;r&lt;&lt;endl;
                        break;
                    }
                }
                else if(b && t2 && t2 % b == 0)
                {
                    r = t2 / b;
                    if(r * a == t1 && r * c == t3)
                    {
                        flag = true;
                        fout&lt;&lt;i&lt;&lt;" "&lt;&lt;j&lt;&lt;" "&lt;&lt;k&lt;&lt;" "&lt;&lt;r&lt;&lt;endl;
                        break;
                    }
                }
                else if(c && t3 && t3 % c == 0)
                {
                    r = t3 / c;
                    if(r * b == t2 && r * a == t1)
                    {
                        flag = true;
                        fout&lt;&lt;i&lt;&lt;" "&lt;&lt;j&lt;&lt;" "&lt;&lt;k&lt;&lt;" "&lt;&lt;r&lt;&lt;endl;
                        break;
                    }
                }
            }
        }
    }
    if(!flag)
        fout&lt;&lt;"NONE"&lt;&lt;endl;
    return 0;
}
</pre>
