---
layout: post
title: USACO/Calf Flac
date: 2014-02-15 20:52:14
subtitle: USACO/Calf Flac
author: dym
header-img:
catalog:   true
tags:
     -算法
---

本文迁移自老博客，原始链接为 <https://seven.blog.ustc.edu.cn/usacocalf-flac/>

寻找最长的回文，寻找回文时忽略标点符号、空格(但应该保留下来以便做为答案输出),只用考虑字母'A'-'Z'和'a'-'z'。
解法：先处理一下字符串，使其变成全小写字母，然后枚举判断。
<pre class = "brush:[cpp]">
#include&lt;iostream&gt;
#include&lt;stdio.h&gt;
#include&lt;string.h&gt;
using namespace std;
struct posi
{
    int n;
    char a;
}hi[20001];
int main()
{
    FILE *fin = fopen ("calfflac.in", "r");
    FILE *fout = fopen ("calfflac.out", "w");
    char str[20001];
    char str1[20001];
    char tmp[81];
    char c;
    while( fgets (tmp, 80, fin) != NULL)
    {
       strcat(str, tmp);
    }
    int len = strlen(str);
    str[len] = '\0';
    int t =0 ;
    for(int i = 0; i < len ;i++)
    {
        if((isupper(str[i])) || (islower(str[i])))
        {
            hi[t].a = tolower(str[i]);
            hi[t].n = i;
            str1[t++] = tolower(str[i]);
        }
    }
    str1[t] = '\0';
    int num  = 1,tem;
    int maxn = 0;
    for(int i = 0; i < t - 1 ; i++)
    {
        int r;
        if(str1[i] == str1[i + 1])
        {
            num = 2;
            r = 1;
            while((i - r >= 0) && (i + 1 + r < t))
            {
                if(str1[i - r] == str1[i + 1 + r])
                {
                    num += 2;
                    r++;
                }
                else
                {
                    break;
                }
            }
        }
        if(num > maxn)
        {
            maxn = num;
            tem = i;
        }
        num = 1;
        int r1 = 1;
        while((i - r1 >= 0) && (i + r1 < t))
        {
            if(str1[i - r1] == str1[i + r1])
            {
                num += 2;
                r1++;
            }
            else
            {
                break;
            }
        }
        if(num > maxn)
        {
            maxn = num;
            tem = i;
        }
    }
    fprintf(fout,"%d\n" , maxn);
    if(maxn % 2 == 0)
    {
        int s = hi[tem - (maxn / 2 ) + 1].n;
        int e = hi[tem + (maxn / 2)].n;
        for(int i = s;i <= e; i++)
        {
            fprintf(fout,"%c", str[i]);
        }
    }
    else
    {
        int s = hi[tem - (maxn / 2 )].n;
        int e = hi[tem + (maxn /2)].n;
        for(int i = s;i <= e; i++)
        {
            fprintf(fout,"%c", str[i]);
        }
    }
    fprintf(fout,"\n");
    fclose(fin);
    fclose(fout);
    return 0;
}
</pre>
