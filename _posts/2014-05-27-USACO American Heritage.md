---
layout: post
title: USACO/American Heritage
date: 2014-05-27 22:42:11
subtitle: USACO/American Heritage
author: dym
header-img:
catalog:   true
tags:
     -算法
---

本文迁移自老博客，原始链接为 <https://seven.blog.ustc.edu.cn/usacoamerican-heritage/>

根据二叉树中序遍历和前序遍历输出后序遍历
解法：递归重建树，然后后序遍历
<pre class = "brush:[cpp]">
#include&lt;iostream&gt;
#include&lt;vector&gt;
#include&lt;fstream&gt;
#include&lt;string.h&gt;
using namespace std;
ofstream fout ("heritage.out");
ifstream fin ("heritage.in");
struct NODE
{
    NODE *pleft;
    NODE *pright;
    char value;
};
void rebuild(char* pre , char* inorder , int nlen , NODE** root)
{
    if(pre == NULL || inorder == NULL)
        return;
    NODE* temp = new NODE;
    temp->value = *pre;
    temp->pleft = NULL;
    temp->pright = NULL;
    if(*root == NULL)
        *root = temp;
    if(nlen == 1)
        return;
    char* pio = inorder;
    char* ple = inorder;
    int ntl = 0;
    while(*pre != *ple)
    {
        if(pre == NULL || ple == NULL)
            return;
        ntl++;
        if(ntl > nlen)
            break;
        ple++;
    }
    int nll = 0;
    nll = (int)(ple - pio);
    int nrl = 0;
    nrl = nlen - nll - 1;
    if(nll > 0)
        rebuild(pre + 1 , inorder , nll , &((*root)->pleft));
    if(nrl > 0)
        rebuild(pre + nll + 1 , inorder + nll + 1, nrl , &((*root)->pright));
}
void posvis(NODE *root)
{
    if(root == NULL)
        return ;
    posvis(root->pleft);
    posvis(root->pright);
    fout&lt;&lt;root->value;
}
int main()
{
    char p[30] , i[30];
    NODE* Root = NULL;
    fin&gt;&gt;i;
    fin&gt;&gt;p;
    rebuild(p , i , strlen(p) , &Root);
    posvis(Root);
    fout&lt;&lt;endl;
    return  0;
}
</pre>