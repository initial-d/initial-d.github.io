---
layout: post
title: USACO/Broken Necklace
date: 2014-02-15 17:11:00
subtitle: USACO/Broken Necklace
author: dym
header-img:
catalog:   true
tags:
     -算法
---

本文迁移自老博客，原始链接为 <https://seven.blog.ustc.edu.cn/usacobroken-necklace/>

有白红蓝三种颜色组成的项链，其中白色可以被当做红色，也可以被当做蓝色，选某个地方切断项链，然后从切断后的每个端点分别向内收集同颜色的珠子直到遇到一个不同的颜色珠子，求收集到到的珠子的最大数目。
解法：假设有三条同样的项链首尾相连接在一起，然后枚举即可。
<pre class="brush:[cpp]">
#include &lt;iostream&gt;
#include &lt;fstream&gt;
#include &lt;string&gt;
#include &lt;algorithm&gt;
using namespace std;

string a,str;
int max1,max2,num1 = 0,num2 = 0,num3 = 0,num4 = 0,len;
int n1,n2,n3,n4;
int main() {
    ofstream fout ("beads.out");
    ifstream fin ("beads.in");
    fin&gt;&gt;len;
    fin&gt;&gt;a;
    str = a + a + a;
    max1 = 0;
    max2 = 0;
    for(int i = len; i < 2 * len; i++)
    {
        int j = i;
        n1 = 0;
        n2 = 0;
        n3 = 0;
        n4 = 0;
        num1 = 0;
        num2 = 0;
        num3 = 0;
        num4 = 0;
        if(str[j] == 'r')
        {
            while((str[j] != 'b') && (j >= 0) && (j < 3 * len))
            {
                n1++;
                j++;
            }
            while((str[j] != 'r') && (j >= 0) && (j < 3 * len))
            {
                num1++;
                j++;
            }
            j = i;
            while((str[j] != 'b') && (j >= 0) && (j < 3 * len))
            {
                n2++;
                j--;
            }
            while((str[j] != 'r') && (j >= 0) && (j < 3 * len))
            {
                num2++;
                j--;
            }
            max1 = max(num1 + n1 + n2 - 1, num2 + n1 + n2 - 1);
        }
        else if(str[j] == 'b')
        {
            while((str[j] != 'r') && (j >= 0) && (j < 3 * len))
            {
                n1++;
                j++;
            }
            while((str[j] != 'b') && (j >= 0) && (j < 3 * len))
            {
                num1++;
                j++;
            }
            j = i;
            while((str[j] != 'r') && (j >= 0) && (j < 3 * len))
            {
                n2++;
                j--;
            }
            while((str[j] != 'b') && (j >= 0) && (j < 3 * len))
            {
                num2++;
                j--;
            }
            max1 = max(num1 + n1 + n2 - 1, num2 + n1 + n2 - 1);
        }
        else if(str[j] == 'w')
        {
            while((str[j] == 'w') && (j >= 0) && (j < 3 * len))
            {
                n3++;
                j++;
            }
            if(str[j] == 'r')
            {
                while((str[j] != 'b') && (j >= 0) && (j < 3 * len))
                {
                    num3++;
                    j++;
                }
                while((str[j] != 'r') && (j >= 0) && (j < 3 * len))
                {
                    num3++;
                    j++;
                }
            }
            else if(str[j] == 'b')
            {
                while((str[j] != 'r') && (j >= 0) && (j < 3 * len))
                {
                    num3++;
                    j++;
                }
                while((str[j] != 'b') && (j >= 0) && (j < 3 * len))
                {
                    num3++;
                    j++;
                }
            }
            j = i;
            while((str[j] == 'w') && (j >= 0) && (j < 3 * len))
            {
                n4++;
                j--;
            }
            if(str[j] == 'r')
            {
                while((str[j] != 'b') && (j >= 0) && (j < 3 * len))
                {
                    num4++;
                    j--;
                }
                while((str[j] != 'r') && (j >= 0) && (j < 3 * len))
                {
                    num4++;
                    j--;
                }
            }
            else if(str[j] == 'b')
            {
                while((str[j] != 'r') && (j >= 0) && (j < 3 * len))
                {
                    num4++;
                    j--;
                }
                while((str[j] != 'b') && (j >= 0) && (j < 3 * len))
                {
                    num4++;
                    j--;
                }
            }
            max1 = max(num3 + n3 + n4 - 1, num4 + n3 + n4 - 1);
        }
        if(max2 < max1)
            max2 = max1;
        if(max2 > len)
            max2 = len;
    }
    fout&lt;&lt;max2&lt;&lt;endl;
    return 0;
}
</pre>

