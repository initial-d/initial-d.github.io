---
layout: post
title: USACO/castle
date: 2014-02-06 01:09:49
subtitle: USACO/castle
author: dym
header-img:
catalog:   true
tags:
     -算法
---

本文迁移自老博客，原始链接为 <https://seven.blog.ustc.edu.cn/usacocastle/>

求矩形城堡有多少个房间，每个房间有多大。要把一面单独的墙（指两个单位间的墙）拆掉以形成一个更大的房间，则移除哪面墙可以得到面积最大的新房间且最大面积为多少。
思路：使用dfs判断连通分量的个数并染色，然后根据着色情况枚举得出可去除的墙
要求：选择墙时注意有优先顺序
<pre class="brush:[cpp]">
#include &lt;iostream&gt;
#include &lt;fstream&gt;
#include &lt;string&gt;
#include &lt;algorithm&gt;
#include &lt;map&gt;
using namespace std;

struct castle {
	bool visited;
	int num;
	int wall;
	int color;
};
int m, n, Max;
int counter, color;
int nmax, ni, nj, nk;
map&lt;int, int&gt; mark;
castle cnt[51][51];
void wall(int wall , bool iswall[]) {
	if (wall & 1)
		iswall[0] = false;
	if (wall & 2)
		iswall[1] = false;
	if (wall & 4)
		iswall[2] = false;
	if (wall & 8)
		iswall[3] = false;
}
void dfs(int i, int j) {
	counter++;
	bool iswall[4] = { true, true, true, true };
	cnt[i][j].visited = true;
	cnt[i][j].color = color;
	wall(cnt[i][j].wall , iswall);
	for (int k = 0; k < 4; k++) {
		if (iswall[k]) {
			if (k == 0  && !cnt[i][j - 1].visited)
				dfs(i, j - 1);
			if (k == 1  && !cnt[i - 1][j].visited)
				dfs(i - 1, j);
			if (k == 2  && !cnt[i][j + 1].visited)
				dfs(i, j + 1);
			if (k == 3  && !cnt[i + 1][j].visited)
				dfs(i + 1, j);
		}
	}
}
int main() {
	ofstream fout ("castle.out");
	ifstream fin ("castle.in");
	fin &gt;&gt; n &gt;&gt; m;
	color = 0;
	Max = 0;
	for (int i = 0; i < m; i++)
		for (int j = 0; j < n; j++) {
			fin &gt;&gt; cnt[i][j].wall;
			cnt[i][j].visited = false;
			cnt[i][j].num = 0;
		}
	for (int i = 0; i < m; i++)
		for (int j = 0; j < n; j++) {
			counter = 0;
			if (!cnt[i][j].visited) {
				dfs(i, j);
				mark[color] = counter;
				if (Max < counter)
					Max = counter;
				color++;
			}

		}
	for (int i = 0; i < m; i++)
		for (int j = 0; j < n; j++)
			cnt[i][j].num = mark[cnt[i][j].color];
	nmax = 0;
	int temp;
	for (int j = 0; j < n; j++)
        for (int i = m - 1; i >= 0; i--)
        {
		bool iswall[4] = { true, true, true, true };
		wall(cnt[i][j].wall , iswall);
		for (int k = 0; k < 4; k++) {
			if (!iswall[k]) {
				if (k == 0  && j - 1 >= 0 &&cnt[i][j - 1].color != cnt[i][j].color) {
					temp = cnt[i][j].num + cnt[i][j - 1].num;
					if (temp > nmax) {
						nmax = temp;
						ni = i + 1;
						nj = j + 1;
						nk = k;
					}
				}
				if (k == 1 && i - 1 >= 0 && cnt[i - 1][j].color != cnt[i][j].color) {
					temp = cnt[i][j].num + cnt[i - 1][j].num;
					if (temp > nmax) {
						nmax = temp;
						ni = i + 1;
						nj = j + 1;
						nk = k;
					}
				}
				if (k == 2 && j + 1 < n && cnt[i][j + 1].color != cnt[i][j].color) {
					temp = cnt[i][j].num + cnt[i][j + 1].num;
					if (temp > nmax) {
						nmax = temp;
						ni = i + 1;
						nj = j + 1;
						nk = k;
					}
				}
				if (k == 3 && i + 1 < m &&cnt[i + 1][j].color != cnt[i][j].color) {
					temp = cnt[i][j].num + cnt[i + 1][j].num;
					if (temp > nmax) {
						nmax = temp;
						ni = i + 1;
						nj = j + 1;
						nk = k;
					}
				}
			}
		}
	}
	fout&lt;&lt;color&lt;&lt;endl&lt;&lt;Max&lt;&lt;endl&lt;&lt;nmax&lt;&lt;endl&lt;&lt;ni&lt;&lt;" "&lt;&lt;nj&lt;&lt;" ";
	if(nk == 0)
	    fout&lt;&lt;"W"&lt;&lt;endl;
	else if(nk == 1)
	    fout&lt;&lt;"N"&lt;&lt;endl;
	else if(nk == 2)
            fout&lt;&lt;"E"&lt;&lt;endl;
	else
	    fout&lt;&lt;"S"&lt;&lt;endl;
	return 0;
}
</pre>
