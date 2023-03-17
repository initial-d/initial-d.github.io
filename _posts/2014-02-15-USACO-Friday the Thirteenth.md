---
layout: post
title: USACO/Friday the Thirteenth
date: 2014-02-15 16:51:56
subtitle: USACO/Friday the Thirteenth
author: dym
header-img:
catalog:   true
tags:
     -算法
---

本文迁移自老博客，原始链接为 <https://seven.blog.ustc.edu.cn/usacofriday-the-thirteenth/>

给出数字N，计算从1900年1月1日至1900+N-1年12月31日中十三号落在周一到周日的次数，N为正整数且不大于400
1.1900年1月1日是星期一
2.4,6,11和9月有30天，其他月份除了2月都有31天，闰年2月有29天,平年2月有28天
3.能被4整除的非世纪年为闰年
4.能被400整除的世纪年为闰年
解法：根据以上条件累加计数即可
<pre class= "brush:[cpp]">
#include &lt;iostream&gt;
#include &lt;fstream&gt;
#include &lt;string&gt;
#include &lt;algorithm&gt;
using namespace std;

int n;
int cusor;
int hash[7] = {0};
bool isleap(int m)
{
    if((m % 400) == 0)
        return true;
    else if(((m % 4) == 0) && ((m % 100) != 0))
        return true;
    else
        return false;//不要忘记缺省情况，否则会出错。
}
int main() {
    ofstream fout ("friday.out");
    ifstream fin ("friday.in");
    cusor = 6;
    hash[cusor]++;
    fin&gt;&gt;n;
    for(int i = 2; i <= n * 12; i++ )
    {
        int tmp = (i - 1) % 12;
        if(tmp == 2)
        {
            if(isleap(1900 + (i - 1)/12))
            {
                cusor = (29 + cusor) % 7;
                hash[cusor]++;
            }
            else
            {
                cusor = (28 + cusor) % 7;
                hash[cusor]++;
            }
        }
        else if((tmp == 0) || (tmp == 1) ||( tmp == 3) ||(tmp == 5) ||(tmp == 7) || (tmp == 8) ||(tmp == 10))
        {
            cusor = (31 + cusor) % 7;
            hash[cusor]++;
        }
        else
        {
            cusor = (30 + cusor) % 7;
            hash[cusor]++;
        }
    }
    fout&lt;&lt;hash[6]&lt;&lt;" "&lt;&lt;hash[0]&lt;&lt;" "&lt;&lt;hash[1]&lt;&lt;" "&lt;&lt;hash[2]&lt;&lt;" "&lt;&lt;hash[3]&lt;&lt;" "&lt;&lt;hash[4]&lt;&lt;" "&lt;&lt;hash[5]&lt;&lt;endl;
    return 0;
}
</pre>

