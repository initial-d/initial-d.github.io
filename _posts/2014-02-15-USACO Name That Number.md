---
layout: post
title: USACO/Name That Number
date: 2014-02-15 19:37:53
subtitle: USACO/Name That Number
author: dym
header-img:
catalog:   true
tags:
     -算法
---

本文迁移自老博客，原始链接为 <https://seven.blog.ustc.edu.cn/usaconame-that-number/>

读取一串数字，然后将数字翻译成与之映射的字母。由于一个数字映射多个字母，所以会产生多个字符串，将其中存在于dict.txt中的字符串输出。
解法：由于文件中共不到五千个字符串，所以可以反向的枚举。枚举文件中的所有字符串，若产生的数字和输入数字相同，则输出。
<pre class = "brush:[cpp]">
#include &lt;iostream&gt;
#include &lt;fstream&gt;
#include &lt;string&gt;
#include &lt;algorithm&gt;
using namespace std;
int main() {
    ofstream fout ("namenum.out");
    ifstream fin ("namenum.in");
    ifstream fin1 ("dict.txt");
    string str;
    int len;
    string num;
    bool flag = false;
    fin&gt;&gt;num;
    while(fin1&gt;&gt;str)
    {
        string tmp;
        len = str.size();
        for(int i = 0; i < len; i++)
        {
            if((str[i] == 'A') || (str[i] == 'B') || (str[i] == 'C'))
            {
                tmp += "2";
            }
            else if((str[i] == 'D') || (str[i] == 'E') || (str[i] == 'F'))
            {
                tmp += "3";
            }
            else if((str[i] == 'G') || (str[i] == 'H') || (str[i] == 'I'))
            {
                tmp += "4";
            }
            else if((str[i] == 'J') || (str[i] == 'K') || (str[i] == 'L'))
            {
                tmp += "5";
            }
            else if((str[i] == 'M') || (str[i] == 'N') || (str[i] == 'O'))
            {
                tmp += "6";
            }
            else if((str[i] == 'P') || (str[i] == 'R') || (str[i] == 'S'))
            {
                tmp += "7";
            }
            else if((str[i] == 'T') || (str[i] == 'U') || (str[i] == 'V'))
            {
                tmp += "8";
            }
            else if((str[i] == 'W') || (str[i] == 'X') || (str[i] == 'Y'))
            {
                tmp += "9";
            }
        }
        if(num == tmp)
        {
            fout&lt;&lt;str&lt;&lt;endl;
            flag = true;
        }
    }
    if(!flag)
        fout&lt;&lt;"NONE"&lt;&lt;endl;
    return 0;
}
</pre>
