---
layout: post
title: USACO/Camelot
date: 2014-05-25 22:02:00
subtitle: USACO/Camelot
author: dym
header-img:
catalog:   true
tags:
     -算法
---

本文迁移自老博客，原始链接为 <https://seven.blog.ustc.edu.cn/usacocamelot/>

棋盘上有一个王和n个骑士，王可以走直线或对角线，一次一格，骑士走日字。
问所有的棋子汇集到一格中所需的最少步数，
走的过程中，骑士可以接到王，然后骑士带着王一起走，此时只算骑士的步数。


解法：先预处理出所有的骑士到所有格的最短距离，使用bfs。
枚举所有的格子为终点，其中再枚举王的四周两层之内的格子为骑士接王的汇合点，
找到到此汇合点最近的骑士，算出接王所需的最少步骤，
加到到对应终点的距离和上。最终找出最少步数，格子不可达时或距离和已经大于当前最优值时可以剪枝
<pre class = "brush:[cpp]">
#include&lt;iostream&gt;
#include&lt;queue&gt;
#include&lt;fstream&gt;
using namespace std;
#define MAXR 32
#define MAXC 27
ofstream fout ("camelot.out");
ifstream fin ("camelot.in");
int kstep[MAXR][MAXC][MAXR][MAXC];
bool vis[MAXR][MAXC];
int path[8][2] = { {-1,-2},{-2,-1},{-2,1},{-1,2},{1,2},{2,1},{2,-1},{1,-2} };
struct point
{
    int r;
    int c;
};
point king , knight[MAXR * MAXC];
int R,C,cnt;
int ans = 1&lt;&lt;30;
void init()
{
    for(int i=1; i<=R; i++)
        for(int j=1; j<=C; j++)
            for(int i1=1; i1<=R; i1++)
                for(int j1=1; j1<=C; j1++)
                    kstep[i][j][i1][j1]=1<<25;
}
void restore()
{
    for(int i=1; i<=R; i++)
        for(int j=1; j<=C; j++)
            vis[i][j]=false;
}
int KingWay(int r1 , int c1)
{
    int row = king.r - r1;
    if(row<0)row =- row;
    int column = king.c - c1;
    if(column<0)column=-column;
    return row > column ? row : column;
}
void BFS()
{
    int cR,cC;
    point tt , tt1 , tt2;
    init();
    for(int r1=1; r1<=R; r1++)
        for(int c1=1; c1<=C; c1++)
        {
            queue&lt;point&gt; que;
            restore();
            tt.c = c1;
            tt.r = r1;
            que.push(tt);
            vis[r1][c1]=true;
            kstep[r1][c1][r1][c1]=0;
            while(!que.empty())
            {
                tt1 = que.front();
                que.pop();
                for(int k=0; k<8; k++)
                {
                    cR=tt1.r + path[k][0];
                    cC=tt1.c + path[k][1];
                    if(cR>0&&cR<=R&&cC>0&&cC<=C&&
                            !vis[cR][cC])
                    {
                        tt2.c = cC;
                        tt2.r = cR;
                        que.push(tt2);
                        kstep[r1][c1][cR][cC]=kstep[r1][c1][tt1.r][tt1.c] + 1;
                        vis[cR][cC]=true;
                    }
                }
            }
        }
}
void input()
{
    char str;
    int input;
    fin&gt;&gt;R&gt;&gt;C;
    fin&gt;&gt;str&gt;&gt;input;
    king.r = input;
    king.c = str-'A'+1;
    while(fin&gt;&gt;str)
    {
        fin&gt;&gt;input;
        knight[cnt].r = input;
        knight[cnt++].c = str-'A'+1;
    }
}
void solve()
{
    int total,diff,kr,kc,tmp;
    bool cut;
    BFS();
    for(int i=1; i<=R; i++)
        for(int j=1; j<=C; j++)
        {
            total=0;
            cut=false;
            for(int k=0; k < cnt; k++)
            {
                if(kstep[knight[k].r][knight[k].c][i][j] >= (1&lt;&lt;25))
                {
                    cut=true;
                    break;
                }
                else 
                    total+=kstep[knight[k].r][knight[k].c][i][j];
            }
            if(cut||total>ans)continue;
            diff=KingWay(i,j);
            for(int r=-2; r<=2; r++)
                for(int c=-2; c<=2; c++)
                {
                    kr=king.r+r;
                    kc=king.c+c;
                    if(kr > 0 && kr <= R && kc > 0 && kc <= C)
                    {
                        for(int k = 0; k < cnt; k++)
                        {
                            tmp = kstep[knight[k].r][knight[k].c][kr][kc] +
                                kstep[kr][kc][i][j] + KingWay(kr,kc) -
                                kstep[knight[k].r][knight[k].c][i][j];
                            if(diff > tmp)
                                diff=tmp;
                        }
                   }
                }
            total+=diff;
            if(ans>total)
                ans=total;
        }
    fout&lt;&lt;ans&lt;&lt;endl;
}
int main()
{
    input();
    solve();
    return 0;
}
</pre>
